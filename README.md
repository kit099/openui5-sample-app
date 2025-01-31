![OpenUI5 logo](http://openui5.org/images/OpenUI5_new_big_side.png)

# openui5-sample-app
> [OpenUI5](https://github.com/SAP/openui5) sample app of Ankit using the [UI5 Tooling](https://github.com/SAP/ui5-tooling).

[![REUSE status](https://api.reuse.software/badge/github.com/SAP/openui5-sample-app)](https://api.reuse.software/info/github.com/SAP/openui5-sample-app)

## Prerequisites
- The **UI5 CLI** of the [UI5 Tooling](https://github.com/SAP/ui5-tooling#installing-the-ui5-cli).
    - For installation instructions please see: [Installing the UI5 CLI](https://github.com/SAP/ui5-tooling#installing-the-ui5-cli).

## Getting started
1. Clone this repository and navigate into it
    ```sh
    git clone https://github.com/SAP/openui5-sample-app.git
    cd openui5-sample-app
    ```
1. Install all dependencies
    ```sh
    npm install
    ```

1. Start a local server and run the application (http://localhost:8080/index.html)
    ```sh
    ui5 serve -o index.html
    ```

## Testing
* Run [ESLint](https://eslint.org/) code validation
    ```sh
    npm run lint
    ```
* Start the [Karma Test Runner](https://karma-runner.github.io/latest/index.html) with the [UI5 Plugin](https://github.com/SAP/karma-ui5) and execute the tests automatically after every change
    ```sh
    npm run watch
    ```
* Run both ESLint and Karma in CI mode
    ```sh
    npm test
    ```
## Building
### Option 1: Standard preload build
1. Execute the build
    ```sh
    ui5 build -a
    ```
1. Run the result
    1. Run a local HTTP server on the build results (`/dist` directory)  
	(**Note:** This script is using the [local-web-server](https://www.npmjs.com/package/local-web-server) npm module, but you can use any HTTP server for that)
        ```sh
        npm run serve-dist
        ```
    1. Open the app at http://localhost:8000

### Option 2: Self-contained build
1. (Optional) Remove previous build results
   ```sh
   rm -rf ./dist
   ```
1. Execute the `self-contained` build to create a bundle with all of your applications runtime dependencies
    ```sh
    ui5 build self-contained -a
    ```
1. Run the result
    1. Run a local HTTP server on the build results (`/dist` directory)  
	(**Note:** This script is using the [local-web-server](https://www.npmjs.com/package/local-web-server) npm module, but you can use any HTTP server for that)
        ```sh
        npm run serve-dist
        ```
    1. Open the app at http://localhost:8000

## Working with local dependencies

For local development of your applications' dependencies (like OpenUI5 libraries) you can link them by using npm. This will allow you to make changes to your applications' dependencies locally and see the impact in your application immediately.

### Preparation
The following needs to be done just once per setup.

1. Clone the OpenUI5 repository and navigate into it
    **Note:** The UI5 version must be 1.65.0 or higher, you can check that in the root `package.json` file
    ```sh
    git clone https://github.com/SAP/openui5.git
    cd openui5
    ```
1. Install all dependencies (this also links all OpenUI5 libraries between each other)
    ```sh
    npm install
    ```
1. Make all projects available as global links. **Note**: The OpenUI5 project uses [wsrun](https://github.com/whoeverest/wsrun) to link all libraries with one command. See [Linking Projects](https://sap.github.io/ui5-tooling/pages/Overview/#linking-projects) for general information about project linking.  
    In the OpenUI5 root directory, execute:
    ```sh
    npm run link-all
    ```
2. The UI5 Tooling currently does not support linking of framework libraries defined in the `ui5.yaml` (see [[RFC] 0006 Local Dependency Resolution](https://github.com/SAP/ui5-tooling/pull/157)). Therefore you need to first remove them from there. Instead, you need to add the dependencies via npm, so that they can be linked in the next step.  
	In the application directory, execute:
    ```sh
    ui5 remove sap.f sap.m sap.ui.core themelib_sap_fiori_3
    ```
    ```sh
    npm install @openui5/sap.f
    npm install @openui5/sap.m
    npm install @openui5/sap.ui.core
    npm install @openui5/themelib_sap_fiori_3
    ```

### Linking
1. In your application directory: Link the required OpenUI5 libraries
    ```sh
    npm link @openui5/sap.f
    npm link @openui5/sap.m
    npm link @openui5/sap.ui.core
    npm link @openui5/themelib_sap_fiori_3
    ```

You can now make changes in your local OpenUI5 repository and see the impact directly when serving or building your application.

### Unlinking
To return to using the OpenUI5 npm packages

1. Remove the dependencies via npm
    ```sh
    npm uninstall @openui5/sap.f
    npm uninstall @openui5/sap.m
    npm uninstall @openui5/sap.ui.core
    npm uninstall @openui5/themelib_sap_fiori_3
    ```
2. Re-add the libraries to the framework section of the `ui5.yaml`
    ```sh
    ui5 add sap.f sap.m sap.ui.core themelib_sap_fiori_3
    ```
