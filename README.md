"start:windows": "ng serve --port 44449 --ssl --ssl-cert %APPDATA%\\ASP.NET\\https\\%npm_package_name%.pem --ssl-key %APPDATA%\\ASP.NET\\https\\%npm_package_name%.key && start msedge --auto-open-devtools-for-tabs https://localhost:44449",




"start:default": "ng serve --port 44449 --ssl --ssl-cert $HOME/.aspnet/https/${npm_package_name}.pem --ssl-key $HOME/.aspnet/https/${npm_package_name}.key && (sleep 5 && open -a \"Microsoft Edge\" --args --auto-open-devtools-for-tabs \"https://localhost:44449\")",



const fs = require('fs');
const spawn = require('child_process').spawn;
const path = require('path');

const baseFolder = 
  process.env.APPDATA !== undefined && process.env.APPDATA !== ''
    ? `${process.env.APPDATA}/ASP.NET/https`
    : `${process.env.HOME}/.aspnet/https`;

console.log('Base folder for certificates:', baseFolder);

const certificateArg = process.argv.map(arg => arg.match(/--name=(?<value>.+)/i)).filter(Boolean)[0];
const certificateName = certificateArg ? certificateArg.groups.value : process.env.npm_package_name;

console.log('Certificate name:', certificateName);

if (!certificateName) {
  console.error('Invalid certificate name. Run this script in the context of an npm/yarn script or pass --name=<<app>> explicitly.');
  process.exit(-1);
}

const certFilePath = path.join(baseFolder, `${certificateName}.pem`);
const keyFilePath = path.join(baseFolder, `${certificateName}.key`);

console.log('Certificate file path:', certFilePath);
console.log('Key file path:', keyFilePath);

if (!fs.existsSync(certFilePath) || !fs.existsSync(keyFilePath)) {
  console.log('Certificate or key file not found. Generating new certificate...');
  spawn('dotnet', [
    'dev-certs',
    'https',
    '--export-path',
    certFilePath,
    '--format',
    'Pem',
    '--no-password',
  ], { stdio: 'inherit', })
  .on('exit', (code) => {
    console.log(`Certificate generation process exited with code ${code}`);
    process.exit(code);
  });
} else {
  console.log('Certificate and key files already exist.');
}
