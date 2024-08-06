"start:windows": "ng serve --port 44449 --ssl --ssl-cert %APPDATA%\\ASP.NET\\https\\%npm_package_name%.pem --ssl-key %APPDATA%\\ASP.NET\\https\\%npm_package_name%.key && start msedge --auto-open-devtools-for-tabs https://localhost:44449",



"start:default": "ng serve --port 44449 --ssl --ssl-cert $HOME/.aspnet/https/${npm_package_name}.pem --ssl-key $HOME/.aspnet/https/${npm_package_name}.key && (sleep 5 && open -a \"Microsoft Edge\" --args --auto-open-devtools-for-tabs \"https://localhost:44449\")",
