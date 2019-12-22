# Javascript / Package development

- [README template](package-readme-template.md)

## What to include

Packages [only include](https://docs.npmjs.com/misc/developers#keeping-files-out-of-your-package) production source code and a few mandatory files (included by default). The [safe way](https://medium.com/@jdxcode/for-the-love-of-god-dont-use-npmignore-f93c08909d8d) to handle it is via `package.json`'s `files` key which looks something like this:
```json
{
  "files": [
    "/lib/**/*.js",
    "!/lib/**/*.test.js",
    "/lib/**/*.json",
    "/lib/**/*.svg"
  ],
}
```

## ESM?

Update 2019-10-22: [this 2ality post](https://2ality.com/2019/10/hybrid-npm-packages.html) explains the possibilities of publishing hybrid NPM packages. Library will have to be tested.

TLDR; no ESM.

There are [many ways](https://github.com/transitive-bullshit/npm-es-modules) to interpret ES Modules, e.g. [esm](https://github.com/standard-things/esm), Node's native experimental support ([1](http://2ality.com/2017/09/native-esm-node.html), [2](http://2ality.com/2018/12/nodejs-esm-phases.html), [3](http://2ality.com/2019/04/nodejs-esm-impl.html)) or webpack ([1](https://medium.com/webpack/webpack-4-released-today-6cdb994702d4), [2](https://webpack.js.org/concepts/modules)). The common convention for declaring them is to use the `.mjs` extension for ESM files and `.js` (or `.cjs`) for CommonJS files.

Shipping backward compatible ESM packages could happen as:
- [shipping both `.mjs` and `.js` files](https://stackoverflow.com/a/52509974/2771889) and letting the interpreters chose
- shipping only `.mjs` files and forcing the interpreters to transpile when needed

The question "Is it safe to release ESM packages?" boils down to:
- Can interpreters handle ESM files?
- Can interpreters fall back to CommonJS files when needed?

TLDR; sadly no and no.

#### Can interpreters handle ESM files?

- [Node.js plans to officially support ESM in October 2019](http://2ality.com/2019/04/nodejs-esm-impl.html#using-es-modules-on-nodejs)
  - developing support in many libraries will only start after Node's proposal is no longer subject to change
- [esm](https://github.com/standard-things/esm) package's support doesn't align with Node's which causes issues
  - "Builtin require cannot sideload .mjs files." https://github.com/standard-things/esm#loading, https://github.com/standard-things/esm/issues/498#issuecomment-403496745
  - "The .mjs file extension should not be the thing developers reach for if they want interop or ease of use. It's available since it's in --experimental-modules but since it's not fully baked I can't commit to any enhancements to it." https://github.com/standard-things/esm/issues/498#issuecomment-403655466
- [mocha doesn't have native support for `.mjs` files](https://github.com/mochajs/mocha/issues/3006)
- Many high-profile projects had issues with `.mjs` files:
  - [create-react-app](https://github.com/facebook/create-react-app/pull/4085)
  - [react-apollo](https://github.com/apollographql/react-apollo/issues/1737)
  - [graphql-js](https://github.com/graphql/graphql-js/issues/1272)
  - [inferno](https://github.com/infernojs/inferno/issues/1296)

#### Can interpreters fall back to CommonJS files when needed?

- Node's proposed way of automatically selecting the right files when the extension is omitted is [broken with the release of v12](http://2ality.com/2019/04/nodejs-esm-impl.html#es-modules-on-npm). They went as far as to request: "Please do not publish any ES module packages intended for use by Node.js until this is resolved."
- Transpiling dependencies is painful and unreliable
  - https://github.com/webpack/webpack/issues/2031
  - https://github.com/babel/babel-loader/issues/171

#### Further reading

- https://medium.com/passpill-project/files-with-mjs-extension-for-javascript-modules-ced195d7c84a
