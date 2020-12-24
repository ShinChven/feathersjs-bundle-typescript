# feathersjs-bundle-typescript

> A demo project to demonstrate how to bundle a TypeScript FeathersJS project to one single JavaScript file using webpack.

## Try this demo

- Clone the project
- Install node dependencies
- Bundle the project to one single JavaScript file: `npm run bundle`
- Run JavaScript distribution file: `npm run serve`

## How to config

### Add webpack dependencies

```bash
npm i -D webpack webpack-cli webpack-node-externals ts-loader
```

### Modify tsconfig.json

```json tsconfig.json
{
  "compilerOptions": {
    "target": "es2018",
    "module": "commonjs",
    "outDir": "./lib",
    "rootDir": "./src",
    "strict": true,
+    "esModuleInterop": true,
+    "noImplicitAny": true,
+    "strictNullChecks": true,
  },
+  "include": [
+    "src/**/*.ts"
+  ],
  "exclude": [
+    "node_modules",
    "test"
  ],
}
```

### Add webpack.config.js

```typescript
const nodeExternals = require('webpack-node-externals');

module.exports = {
  entry: './src/index.ts',
  output: {
    filename: 'index.js', // <-- Important
    libraryTarget: 'this' // <-- Important
  },
  mode: 'production',
  target: 'node', // <-- Important
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: 'ts-loader',
        options: {
          transpileOnly: true
        }
      }
    ]
  },
  resolve: {
    extensions: [ '.ts', '.tsx', '.js' ]
  },
  externals: [nodeExternals()] // <-- Important
};
```

### Bundle and run

```bash
webpack
node dist/index.js
```

## References

- [[Stackoverflow] How do I compile my TypeScript code for Node.js to one file?](https://stackoverflow.com/a/56220367/2108080)
