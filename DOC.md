# Chrome Extension With NPM Webpack CLI

## Installation

In the package root directory, run the `npm init` command.

```bash
npm init
```

## Create Required Folder Structure

```
├─ src
   ├─ icons
   ├─ content // content scripts
   ├─ locales
   |  └─ en
   |     └─ messages.json
   ├─ options
   |  ├─ options.html
   |  ├─ options.css
   |  └─ options.js
   ├─ popup
   |  ├─ popup.html
   |  ├─ popup.css
   |  └─ popup.js
   |
   ├─ # Add More Pages
   |
   ├─ background.js
   └─ manifest.json
```

## Modify `package.json` File

```json
{
  "name": "extension-npm-cli",
  "version": "0.0.1",
  "description": "extension with npm",
  "scripts": {
    
  },
  "author": "ctechhindi",
  "license": "MIT",
  "devDependencies": {
    "webpack": "^4.44.1",
    "webpack-cli": "^3.3.12"
  }
}

```

## Install NPM Package

### **webpack**

* `webpack` is a module bundler.
* Install [Webpack](https://www.npmjs.com/package/webpack) with npm:
    ```bash
    npm install --save-dev webpack
    npm install --save-dev webpack-cli
    ```
* Read Webpack [Guides](https://webpack.js.org/guides/development/)

#### Create `webpack.config.js` File in the Project Root Directory

```js
const config = {
    mode: 'production', // production, development

    context: __dirname + '/src',

    // Where to start bundling
    entry: {
        'background': './background.js',
        // ... Insert Others Files
    },

    // Where to output
    output: {
        path: __dirname + '/dist',
        filename: '[name].js',
    }
};

module.exports = config;
```

Insert Script in `package.json` file [webpack development](https://webpack.js.org/guides/development/)

```json
scripts: {
    "watch": "webpack --watch",
    "watch:dev": "webpack --watch --mode development",
    "build": "webpack"
}
```

Webpack [Command Line Interface](https://webpack.js.org/api/cli/)

```bash
webpack --help
webpack --json
```

---

### **copy-webpack-plugin**

* Copies individual files or entire directories, which already exist, to the build directory.
* Install [CopyWebpackPlugin](https://webpack.js.org/plugins/copy-webpack-plugin/) with npm:
    ```bash
    npm install copy-webpack-plugin --save-dev
    ```

#### Use `CopyWebpackPlugin` in webpack config file.

Then add the plugin to your webpack config. For example:

```js
// webpack.config.js
const CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = {
  plugins: [
    new CopyWebpackPlugin({
      patterns: [
        { from: 'icons', to: 'icons' },
        { from: 'locales', to: '_locales' }
      ],
    }),
  ],
};
```

---

### mini-css-extract-plugin

* This plugin extracts CSS into separate files.
* Install [MiniCssExtractPlugin](https://webpack.js.org/plugins/mini-css-extract-plugin/) with npm:
    ```bash
    npm install --save-dev mini-css-extract-plugin
    npm install --save-dev style-loader css-loader
    ```

#### Use `MiniCssExtractPlugin` in webpack config file.

Then add the loader and the plugin to your webpack config. For example:

```css
/* style.css */
body {
  background: green;
}
```

```js
// component.js
import './style.css';
```

```js
// webpack.config.js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  plugins: [new MiniCssExtractPlugin()],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, 'css-loader'], // https://webpack.js.org/loaders/css-loader/
      },
    ],
  },
};
```

---

### **webpack-extension-reloader**

* This plugin extracts CSS into separate files.
* Install [webpack-extension-reloader](https://github.com/rubenspgcavalcante/webpack-extension-reloader) with npm:
    ```bash
    npm install webpack-extension-reloader --save-dev
    ```

#### Use `Webpack Extension Reloader` in webpack config file.

```js
// webpack.config.js
const ExtensionReloader  = require('webpack-extension-reloader');

plugins: [
  new ExtensionReloader(),
  new CopyWebpackPlugin([
        { from: "./src/manifest.json" },
        { from: "./src/popup.html" },
    ]),
]
```

---

### **cross-env**

* Run scripts that set and use environment variables across platforms.
* Install [cross-env](https://www.npmjs.com/package/cross-env) with npm:
    ```bash
    npm install cross-env --save-dev
    ```

#### Use `cross-env` in webpack config file.

I use this in my npm scripts: `cross-env NODE_ENV=production`

```json
// package.json
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack"
  }
}
```

```js
// webpack.config.js
const config = {
    mode: process.env.NODE_ENV,
}
```

## Asset Management

### [Loading CSS](https://webpack.js.org/guides/asset-management/#loading-css)

In order to import a CSS file from within a JavaScript module, you need to install and add the style-loader and css-loader to your module configuration.

```bash
npm install --save-dev style-loader css-loader
```

### [Loading Images](https://webpack.js.org/guides/asset-management/#loading-images)

```bash
npm install --save-dev file-loader
```

### [sass-loader](https://webpack.js.org/loaders/sass-loader/#root)

```bash
npm install sass-loader sass webpack --save-dev
```

### [babel-loader](https://www.npmjs.com/package/babel-loader/v/8.0.0-beta.1)

```bash
npm install babel-loader babel-minify --save-dev
```

### Package for Build Zip  

```bash
npm install archiver --save
```

## Project Command

```bash
npm run watch
npm run watch:dev
npm run build
npm run build-zip
```

## Pending Problems

* Options.css file not minify