{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Chrome against localhost (apps)",
      "type": "pwa-chrome",
      "request": "launch",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}/apps/marvel-core/src",
      "sourceMaps": true,
      "sourceMapPathOverrides": {
        "webpack:///src/*": "${webRoot}/*"
      }
    }
  ]
}