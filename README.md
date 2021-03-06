## Installation

1. `npm install`

2. Install VSCode extensions: [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint), [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

## Usage

1. `npm run start`: start running dev-server

2. `npm run build`: build bundled JS file from src folder into dist folder

3. `npm run watch`: start typescript watch mode

4. `npm run lint`: use ESLint for manually checking files inside src folder

5. Fix format error automatically on save

## Project Setting Details:

1. Install dev dependencies

    `$ npm install --save-dev webpack webpack-cli webpack-dev-server typescript ts-loader`

2. Install dependencies

    `$ npm install react react-dom --save` 

3. Install eslint and relavant eslint-plugins

4. Install dev dependencies for TS type

    `$ npm i -D @types/react @types/react-dom`

5. Modify `tsconfig.json` as follows

  - Customize the `"outDir"`, delete `"rootDir"` because webpack will determine where the root file is and take over there.

  - Set `"jsx"` and `"module"` as [React 17 suggestion](https://zh-hant.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html).

```js
{
    "compilerOptions": {
      "target": "es5",
      "module": "esnext",
      "lib": ["ES2017", "ES7", "ES6", "DOM"],
      "strict": true,
      "noImplicitAny": false,
      "removeComments": true,
      "preserveConstEnums": true,
      "baseUrl": "./",
      "outDir": "./dist",
      "typeRoots": ["node_modules/@types"],
      "esModuleInterop": true,
      "sourceMap": false,     // not to generate index.js.map file
      "declaration": false,   // not to generate index.d.ts file
      "noEmitOnError": true,  // stop producing bundled file on compilation error
      "jsx": "react-jsx"
    },
    "include": ["src/*.ts","src/*.tsx"],
    "exclude": ["node_modules", "dist"]
}
```

6. Create `webpack.config.js` as follows

    `resolve: { extensions: ['.ts','.tsx','.js','.jsx'] }` is for the import syntax like `import App from './app';` without `'./app.tsx'` suffix

```js
const path = require('path'); 

module.exports = {
    mode: 'development',
    entry: './src/index.tsx',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    devServer: {
        contentBase: "./dist",
    },
    module: {
        rules: [
            {
                test: /\.ts$|\.tsx$/,
                use: 'ts-loader',
                exclude: /node_modules/
            }
        ]
    },
    resolve: {
        extensions: ['.ts','.tsx','.js','.jsx']
    }
};
```

7. Split `webpack.config.js` into `webpack.common-config.js`, `webpack.dev-config.js`, `webpack.prod-config.js` using `webpack-merge` package

8. Install package for supporting path alias

    `$ npm i -D tsconfig-paths-webpack-plugin`

9. Install packages for removing existing files under `dist` and generate `index.html` with hashed `app.[contentHash].js` after `$ npm run build`

    `$ npm i -D html-webpack-plugin clean-webpack-plugin`

10. Install package for removing `bundle.js.LICENSE.txt` after `$ npm run build`

    `$ npm i -D terser-webpack-plugin`

11. Modify the script in `package.json`

```js
  "scripts": {
    "start": "webpack serve --config webpack.config.dev.js",
    "build": "webpack --config webpack.config.prod.js",
    "watch": "tsc -w",
    "lint": "eslint ./src --ext .js,.jsx,.ts,.tsx"
  },
```

12. Create and modify `eslintrc.js`
 
13. Add VSCode setting file `setting.json` in `.vscode` folder:
    [Set the TypeScript version of local VSCode workspace](https://stackoverflow.com/questions/39668731/what-typescript-version-is-visual-studio-code-using-how-to-update-it) if needed 
```js
{
  "editor.formatOnSave": false,
  "editor.codeActionsOnSave": {
    "source.fixAll": true
  }
}
```
