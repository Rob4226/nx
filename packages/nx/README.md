# NativeScript Plugin for Nx

<p align="center"><img src="https://raw.githubusercontent.com/nativescript/nx/master/tools/assets/images/nx-nativescript.jpg" width="600"></p>

<div align="center">

[![License](https://img.shields.io/npm/l/@nativescript/core.svg?style=flat-square)]()
[![NPM Version](https://badge.fury.io/js/%40nativescript%2Fnx.svg)](https://www.npmjs.com/@nativescript/nx)

</div>

> Requires at least NativeScript CLI v8.x.x or higher. You can confirm your CLI version by running `ns -v`.

If updating from Nx <= 19 to >=20, [see the migration guide](https://github.com/NativeScript/nx/wiki/Migrate-guide-for-Nx-19-to-20) for updates to your `project.json` and commands.

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [NativeScript Plugin for Nx](#nativescript-plugin-for-nx)
  - [Table of Contents](#table-of-contents)
  - [Getting started](#getting-started)
    - [Create a new Nx workspace](#create-a-new-nx-workspace)
    - [Init workspace](#init-workspace)
    - [Install NativeScript plugin](#install-nativescript-plugin)
    - [Create an app](#create-an-app)
      - [`--framework [angular|vanilla]`](#--framework-angularvanilla)
      - [`--groupByName`](#--groupbyname)
      - [Develop on simulators and devices](#develop-on-simulators-and-devices)
      - [Configuration options](#configuration-options)
      - [Run with a specific configuration](#run-with-a-specific-configuration)
      - [Run tests](#run-tests)
      - [Create a build](#create-a-build)
      - [Clean](#clean)
  - [Create NativeScript library](#create-nativescript-library)
    - [`--groupByName`](#--groupbyname-1)
  - [Using NativeScript plugins](#using-nativescript-plugins)
    - [Installing NativeScript plugins at app-level](#installing-nativescript-plugins-at-app-level)
    - [Installing NativeScript plugins at workspace-level](#installing-nativescript-plugins-at-workspace-level)
    - [Known issues](#known-issues)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Getting started

### Create a new Nx workspace

```sh
# Using npm
npx create-nx-workspace@latest

# Using yarn
yarn create-nx-workspace@latest
```

At the prompts, you can use:

```sh
✔ Where would you like to create your workspace? · {your-workspace-name}

? Which stack do you want to use? … 
None:          Configures a minimal structure without specific frameworks or technologies.
# Choose "None"

? Package-based monorepo, integrated monorepo, or standalone project?
Integrated Monorepo:        Nx creates a monorepo that contains multiple projects.
# Choose "Integrated"

? Do you want Nx Cloud to make your CI fast? 
# Choice is completely up to you
```

### Init workspace

Initialize a TypeScript project -- This will ensure a `tsconfig.base.json` is created to begin building your workspace.

```sh
npx nx g @nx/js:init
```

### Install NativeScript plugin

```sh
# Using npm
npm install --save-dev @nativescript/nx

# Using yarn
yarn add -D @nativescript/nx
```

*Note: If you get a Warning on peer dependencies you can ignore.* For example:

```bash
npm WARN ERESOLVE overriding peer dependency
npm WARN While resolving: @swc-node/core@1.13.1
npm WARN Found: @swc/core@1.3.107
npm WARN node_modules/@swc/core
npm WARN   peer @swc/core@">= 1.3" from @swc-node/register@1.8.0
```

### Create an app

```sh
# Using npm
npx nx g @nativescript/nx:app <app-name> [...options]

# Using yarn
yarn nx g @nativescript/nx:app <app-name> [...options]
```

This will generate: 

```
apps/nativescript-<app-name>
```

The NativeScript Nx plugin will prefix apps by default to help distinguish them against other apps in your workspace for clarity. 

#### `--framework [angular|vanilla]`

You will be prompted to choose a framework when this flag is ommitted.

Use this option to explicitly choose a specific frontend framework integration app.

This setting will be saved with plugin settings the first time it's used to automatically choose this frontend framework integration for subsequent usages and with other generators without having to specify the flag again.

#### `--groupByName`

If you prefer you can also provide a flag to suffix instead:

```sh
npx nx g @nativescript/nx:app <app-name> --groupByName
```

This will generate: 

```
apps/<app-name>-nativescript
```

#### Develop on simulators and devices

**Android:**

```sh
npx nx run <app-name>:android
```

**iOS:** (Mac only)

```sh
npx nx run <app-name>:ios
```

#### Configuration options

A custom executor is provided via `@nativescript/nx:build` with the following options:

```
"debug": {
  "type": "boolean",
  "default": true,
  "description": "Use 'ns debug' instead of 'ns run'. Defaults to true"
},
"device": {
  "type": "string",
  "description": "Device identifier to run app on.",
  "alias": "d"
},
"emulator": {
  "type": "boolean",
  "default": false,
  "description": "Explicitly run with an emulator or simulator"
},
"noHmr": {
  "type": "boolean",
  "default": false,
  "description": "Disable HMR"
},
"uglify": {
  "type": "boolean",
  "default": false,
  "description": "Enable uglify during the webpack build"
},
"verbose": {
  "type": "boolean",
  "default": false,
  "description": "Enable verbose logging"
},
"release": {
  "type": "boolean",
  "default": false,
  "description": "Enable release mode during build using the --release flag"
},
"forDevice": {
  "type": "boolean",
  "default": false,
  "description": "Build in device mode using the --for-device flag"
},
"production": {
  "type": "boolean",
  "default": false,
  "description": "Build in production mode using the --env.production flag"
},
"copyTo": {
  "type": "string",
  "description": "When building, copy the package to this location."
},
"provision": {
  "type": "string",
  "description": "(iOS Only) When building, use this provision profile name."
},
"aab": {
  "type": "boolean",
  "default": false,
  "description": "(Android Only) When building, create an Android App Bundle (.aab file)."
},
"keyStorePath": {
  "type": "string",
  "description": "(Android Only) When building, use the keystore file at this location."
},
"keyStorePassword": {
  "type": "string",
  "description": "(Android Only) When building, use this keystore password."
},
"keyStoreAlias": {
  "type": "string",
  "description": "(Android Only) When building, use this keystore alias."
},
"keyStoreAliasPassword": {
  "type": "string",
  "description": "(Android Only) When building, use this keystore alias password."
}
```

The options follow the [NativeScript command line option flags](https://docs.nativescript.org/development-workflow.html#run).

Here's an example app config:

```json
"nativescript-mobile": {
  "projectType": "application",
  "sourceRoot": "apps/nativescript-mobile/src",
  "prefix": "",
  "targets": {
    "build": {
      "executor": "@nativescript/nx:build",
      "options": {
        "noHmr": true,
        "uglify": true,
        "forDevice": true,
        "release": true,
        "android": {
          "copyTo": "./dist/app.apk",
          "keyStorePath": "/path/to/android.keystore",
          "keyStoreAlias": "app",
          "keyStorePassword": "pass",
          "keyStoreAliasPassword": "pass"
        },
        "ios": {
          "copyTo": "./dist/app.ipa"
        }
      },
      "configurations": {
        "production": {
          "production": true,
          "release": true,
          "android": {
            "keyStorePassword": "productionpw"
          },
          "fileReplacements": [
            {
              "replace": "./src/environments/environment.ts",
              "with": "./src/environments/environment.prod.ts"
            }
          ]
        }
      }
    },
    "debug": {
      "executor": "@nativescript/nx:debug",
      "options": {
        "noHmr": true,
        "uglify": false,
        "release": false,
        "forDevice": false,
        "prepare": false
      },
      "configurations": {
        "prod": {
           "fileReplacements": [
            {
              "replace": "./src/environments/environment.ts",
              "with": "./src/environments/environment.prod.ts"
            }
          ]
        }
      }
    },
    "prepare": {
      "executor": "@nativescript/nx:prepare",
      "options": {
        "noHmr": true,
        "production": true,
        "uglify": true,
        "release": true,
        "forDevice": true,
        "prepare": true
      },
      "configurations": {
        "prod": {
           "fileReplacements": [
            {
              "replace": "./src/environments/environment.ts",
              "with": "./src/environments/environment.prod.ts"
            }
          ]
        }
      }
    },
    "clean": {
      "executor": "@nativescript/nx:clean",
      "options": {}
    },
    "lint": {
      "executor": "@nx/linter:eslint",
      "options": {
        "lintFilePatterns": [
          "apps/nativescript-app/**/*.ts",
          "apps/nativescript-app/src/**/*.html"
        ]
      }
    },
    "test": {
      "executor": "@nativescript/nx:test",
      "outputs": [
        "coverage/apps/nativescript-app"
      ],
      "options": {
        "coverage": true
      },
      "configurations": {}
    }
  }
}
```

#### Run with a specific configuration

**Android:**

```sh
npx nx debug <app-name> android -c=prod
```

**iOS:** (Mac only)

```sh
npx nx debug <app-name> ios -c=prod
```

#### Run tests

**Android:**

```sh
npx nx test <app-name> android
```

**iOS:** (Mac only)

```sh
npx nx test <app-name> ios
```

You can generate coverage reports by using the flag with iOS or Android, for example:

```sh
npx nx test <app-name> ios --coverage
```

You can also set this option in the config, for example:

```json
"test": {
  "executor": "@nativescript/nx:test",
  "outputs": ["coverage/apps/nativescript-mobile"],
  "options": {
    "coverage": true // can set to always be on for both platforms
  },
  "configurations": {
    "android": {
      "coverage": false // or can override per platform if needed
    },
    "ios": {
      "coverage": true
    }
  }
}
```

#### Create a build

Instead of running the app on a simulator or device you can create a build for the purposes of distribution/release. Various release settings will be needed for iOS and Android which can be passed as additional command line arguments. [See more in the NativeScript docs here](https://docs.nativescript.org/releasing.html#overview). Any additional cli flags as stated in the docs can be passed on the end of the `nx build` command that follows.

Build with an environment configuration enabled (for example, with `prod`):

**Android:**

```sh
npx nx build <app-name> android --c=prod
```

You can pass additional NativeScript CLI options as flags on the end of you build command.

* example of building AAB bundle for upload to Google Play:

```
npx nx build <app-name> android --c=prod \
  --aab \
  --key-store-path=<path-to-your-keystore> \
  --key-store-password=<your-key-store-password> \
  --key-store-alias=<your-alias-name> \
  --key-store-alias-password=<your-alias-password> \
  --copyTo=./dist/build.aab
```

**iOS:** (Mac only)

```sh
npx nx build <app-name> iod --c=prod
```

As mentioned, you can pass any additional NativeScript CLI options as flags on the end of your nx build command:

* example of building IPA for upload to iOS TestFlight:

```
npx nx build <app-name> ios --c=prod \
  --provision <provisioning-profile-name> \
  --copy-to ./dist/build.ipa
```

#### Clean

It can be helpful to clean the app at times. This will clear out old dependencies plus iOS/Android platform files to give your app a nice reset.

```sh
npx nx clean <app-name>
```

## Create NativeScript library

You can create a library of NativeScript components or plugins or whatever you'd like.

```sh
npx nx g @nativescript/nx:library buttons
```

This will generate a `nativescript-buttons` library where you could build out an entire suite of button behaviors and styles for your NativeScript apps.

```ts
import { PrimaryButton } from '@myorg/nativescript-buttons';
```

The NativeScript Nx plugin will prefix libraries by default to help distinguish them against other apps and libraries in your workspace for clarity. 

### `--groupByName`

If you prefer you can also provide a flag to suffix instead:

```sh
npx nx g @nativescript/nx:library buttons --groupByName
```

Which would generate a `buttons-nativescript` library.

```ts
import { PrimaryButton } from '@myorg/buttons-nativescript';
```


## Using NativeScript plugins

NativeScript plugins can be used in Nx workspaces in one of the two following methods:

### Installing NativeScript plugins at app-level

If the plugin is needed by one app only, and not others, you can install it for the specific app:

```sh
cd apps/<app-name>
npm install <plugin-name>
```

### Installing NativeScript plugins at workspace-level

Alternatively, you can install the plugins at the workspace (root), so it is accesible to all your workspace apps:
```sh
npm install --save <plugin-name>
```

### Known issues
If a plugin contains platforms folder with native includes, the plugin must be added to app package.json at moment. https://github.com/NativeScript/nx/issues/17#issuecomment-841680719
