# Javascript / Best practices

#### Avoid inpure exports

If imports are not pure (not without side effects) [tree-shaking](https://webpack.js.org/guides/tree-shaking/) and faster access to imports are lost. See also:
- https://geedew.com/es6-module-gotchas/
- https://medium.com/@rauschma/note-that-default-exporting-objects-is-usually-an-anti-pattern-if-you-want-to-export-the-cf674423ac38

BAD

```js
export default new Store();
```

This would technically be a Singleton because [ES6 modules are only evaluated once](https://stackoverflow.com/questions/36564901/in-the-import-syntax-of-es6-how-is-a-module-evaluated-exactly). Instead, prefer default exporting a class that implements the Singleton pattern.

BAD

```js
export default { a, b, c };
```

Instead, prefer breaking up the code into separate default exported modules or use named exports. See also:
- https://geedew.com/es6-module-gotchas/
- https://esdiscuss.org/topic/moduleimport#content-0
- https://github.com/airbnb/javascript/issues/710

GOOD

The right way of Singleton export.

```js
export default class Store { /* insert singleton pattern */ };
```

GOOD

```js
export default () => { /*...*/ };
```

GOOD

```js

const func = () => {
  // ...
};

export default func;
```

#### Comment dependencies in the `package.json`

As described [here](https://stackoverflow.com/a/14221781/2771889) and [here](https://stackoverflow.com/questions/14221579/how-do-i-add-comments-to-package-json-for-npm-install/14221781#comment50530934_14221781). E.g.
```json
{
  "//dependencies": {
    "crypto-exchange": "Unified exchange API"
  },
  "dependencies": {
    "crypto-exchange": "^2.3.3"
  },
  "//devDependencies": {
    "chai": "Assertions",
    "mocha": "Unit testing framwork",
    "sinon": "Spies, Stubs, Mocks",
    "supertest": "Test requests"
  },
  "devDependencies": {
    "chai": "^4.1.2",
    "mocha": "^4.0.1",
    "sinon": "^4.1.3",
    "supertest": "^3.0.0"
  }
}
```

#### Specify `npm` version requirements to avoid `package-lock` ping-pong

See: https://stackoverflow.com/a/55373854/2771889

`package.json`
```json
{
  "engines": {
    "npm": ">=6.6"
  }
}
```

`.npmrc`
```
engine-strict=true
```

#### Serialize objects when concatenating to strings

GOOD

```js
const obj = {
  // ...
};

console.log("Uh! " + JSON.stringify(obj);

// => Uh! { ... }
```

BAD

```js
const obj = {
  // ...
};

console.log("Uh! " + obj);

// => Uh! [object Object]
```
