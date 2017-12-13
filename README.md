# universal-react-electron

A universal React + Electron boilerplate.

Please make sure you have Electron installed globally.

1. `npm install`, of course.
2. `npm run electron-dev` to create a dev script.
3. `npm run preelectron-pack` to build the React app, then `npm run electron-pack` to build the application package.

**Introduce some of the vital packages:**

* [electron-builder](https://www.electron.build/) - A complte solution to package and build a ready-for distribution Electron app for MacOS, Windows and Linux with auto-update support out of the box.
* [concurrently](https://github.com/kimmobrunfeldt/concurrently) - Runs command concurrently. I use this package to run React and Electron concurrently in one command - check out the `electron-dev` script under `package.json`.
* [wait-on](https://github.com/jeffbski/wait-on) - A command line utility and Node.js API that will wait for files, ports, sockets and http(s) resources to become available. I use this package to wait for the React server to begin running before starting the Electron app - see the `electron-dev` as well.

**Things worth noticing:**

* You need to build the React app before building the Electron app. The `preelectron-pack` command is for the former, followed by `electron-pack`. Notice the latter needs to specify the `build` property, because of a minor confict between **create-react-app** and **electron-builder** - they both use the `build` folder for a different purpose. Manually configure **electron-builder**'s correct folders for build step by adding `build` section to  `package.json` can solve this conflict.
* Considering that we will serve the `build/index.html` instead of using `localhost:3000` under production, we need the **electron-is-dev** to help determine if Electron is running in development, and make some tweaks in the `mainWindow.loadURL` functionality:

    mainWindow.loadURL(isDev ? ‘http://localhost:3000' : `file://${path.join(__dirname, ‘../build/index.html’)}`);