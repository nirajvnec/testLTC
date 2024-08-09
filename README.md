const os = require('os');

// Get the full username including domain (if part of the username)
const fullUsername = os.userInfo().username;

// Assuming the username is in the format DOMAIN\username, split by the backslash
const domain = fullUsername.includes('\\') ? fullUsername.split('\\')[0] : 'No domain detected';

console.log('Domain:', domain);
