# 🏗️ Project setup

## 📦 NPM package
To setup your project as an NPM package and generate a `package.json` run `npm init` and 
provide the requested values.  

As `license` put `MIT` and add a `LICENSE` file in your root folder with the license text
as found on https://opensource.org/licenses/MIT.

Also add a `README.md` file in your root folder.

In your `package.json`, add a `files` array with the files that should be packaged.

To ensure the quality of your published packages add the following script to your `package.json`:
```javascript
"scripts": {
  "prepublishOnly": "yarn test && yarn build"
}
```
> We will add the `test` and `build` scripts later in this checklist.

## 🦖 Typescript
To add typescript to your project run `yarn add --dev typescript` and create a `tsconfig.json`
file.
```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "target": "es2015",
    "module": "commonjs",
    "moduleResolution": "node",
    "sourceMap": true,
    "strict": true
  },
  "include": ["src/**/*.ts"]
}
```

Add a `src` folder to hold your typescript files.

Add a `build` script in your `package.json` with value `tsc` to compile the typescript
sources in your `src` folder to javascript files in your `dist` folder.
```json
"scripts": {
  "build": "tsc"
},
```

Your `main` and `files` fields in your `package.json` should only include javascript 
files from the dist folder.

## 🎀 eslint
Add eslint to your project with `yarn add --dev eslint`, then run `eslint --init` to set
up the configuration.
> How to use eslint > `To check syntax, find problems, and enforce code style`  
> Type of modules ? > `JavaScript modules`  
> Which framework ? > `None of these`  
> TypeScript? > select the appropriate answer  
> Where does your code run? > select the appropriate answer  
> Define a style? > `Use a popular style guide`  
> Style guide? > `Airbnb`  
> Config file format? > `JavaScript`

If you are using typescript, add the following `rules` and `settings` in the generated `eslintrc.js`.
```javascript
"rules": {
  "import/extensions": [
    "error",
    "ignorePackages",
    {
      "js": "never",
      "ts": "never",
    }
  ]
},
"settings": {
  "import/resolver": {
    "node": {
      "extensions": [".js", ".ts"]
    }
  }
},
```

Run eslint before compiling your code by adding the eslint command to your `build` script in
your `package.json`.
```json
"scripts": {
  "build": "eslint src/**/*.ts && tsc"
},
```

In your IDE set up a file watcher to run eslint on file save.

For IntelliJ / WebStorm:  
Install the `File Watcher` plugin if you haven't already  

In Settings > Tools > File Watchers, add a new watcher with the `<custom>` template.  
> File type: `Typescript`  
> Scope: `Current File`  
> Program: select `eslint` from the `.bin` folder in your `node_modules`  
> Arguments: `--fix $FilePath$`  
> Output paths to refresh: `$FileDir$`  
> Under advanced options uncheck `Auto-save` and `external changes`  

## 🃏 Jest
Run `yarn add --dev jest ts-jest @types/jest` to add Jest to your project.

Create a file `jest.config.js` to your project's root folder with the following content:
```javascript
  'use strict';
  
  module.exports = {
    preset: 'ts-jest',
    testEnvironment: 'node',
    coverageReporters: ['cobertura', 'lcov'],
    coverageDirectory: './build/code-coverage',
    reporters: ['default'],
  };
```

Add a `test` script to your `package.json`:
```json
"scripts": {
  "test": "jest --coverage"
},
```

## 🖥 Dev server
If your typescript project includes an express server, run `yarn add --dev ts-node ts-node-dev` to
add a typescript dev server.

In your `package.json` add a `start` script:
```json
"scripts": {
  "start": "ts-node-dev --respawn src/server.ts"
},
```
