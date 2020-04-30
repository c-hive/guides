# Javascript / Best practices

#### Avoid exporting an object for the purpose of exporting its properties

[Axel Rauschmayer](https://medium.com/@rauschma/note-that-default-exporting-objects-is-usually-an-anti-pattern-if-you-want-to-export-the-cf674423ac38):
> Note that default-exporting objects is usually an anti-pattern (if you want to export the properties). You lose some ES6 module benefits ([tree-shaking](https://webpack.js.org/guides/tree-shaking/) and [faster access to imports](https://2ality.com/2014/09/es6-modules-final.html#benefit-1%3A-faster-lookup)).

Instead, prefer breaking up the code into separate default exported modules or use named exports. See also:
- https://geedew.com/es6-module-gotchas/
- https://github.com/airbnb/javascript/issues/710#issuecomment-297840604
- https://2ality.com/2014/09/es6-modules-final.html#default-exports-are-favored

BAD

```js
export default { a, b, c };
```

GOOD

```js
export a;
export b;
export c;
```

GOOD

```js
// a.js
export default a;

// b.js
export default b;

// c.js
export default c;
```

#### Avoid exporting mutable objects and mutating imported objects

https://geedew.com/es6-module-gotchas/
> ES6 modules were designed with a static module structure preferred. This allows the code to be able to discover the import/export values at compile time rather than runtime. Exporting an object from a module allows for unexpected situations and removes the static compilation benefits.

See also:
- https://2ality.com/2014/09/es6-modules-final.html#static-module-structure
- http://calculist.org/blog/2012/06/29/static-module-resolution/
- [All exports are static](https://stackoverflow.com/questions/35035304/what-qualifies-as-being-a-dynamic-export-in-es6)
- Note that [functions are technically mutable Objects](https://stackoverflow.com/a/2136691/2771889) but exporting them is obviously fine

#### Avoid exporting an evaluation

In this case the module exports the result which is only evaluated once. This is rarely the desired outcome, but if it is, a Singleton pattern should be used instead. [Some ES6 module benefits](#avoid-exporting-an-object-for-the-purpose-of-exporting-its-properties) are lost, it makes imports slower and makes them possible to cause side-effects which should rather happen upon invocation.

Instead, export a function or class.

See also:
- [All exports are static](https://stackoverflow.com/questions/35035304/what-qualifies-as-being-a-dynamic-export-in-es6)
- [ES6 modules are only evaluated once](https://stackoverflow.com/questions/36564901/in-the-import-syntax-of-es6-how-is-a-module-evaluated-exactly)

BAD

```js
export default someModule();
```

GOOD

```js
export default someModule;
```

```js
export default () => { someModule() };
```

#### Pass objects to `console.log` as separate parameters

GOOD

```js
user = { name: "Albert" };
console.log("User:", user);
// => User: Object { name: "Albert" }
```

BAD

```js
user = { name: "Albert" };
console.log(`User: ${user}`);
// => User: [object Object]

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

#### Always give functions names when exporting, otherwise debugging information is lost

Enable [`import/no-anonymous-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-anonymous-default-export.mds) to report if a default export is unnamed. 

```js
const foo = () => {
  console.trace();

  // ...
};

export default foo;

// foo @ foo.js:2
// ...
```

GOOD

```js
export default function foo() {
  console.trace();

  // ...
}


// foo @ foo.js:2
// ...
```



BAD

```js
export default () => {
  console.trace();

  // ...
};

// (anonymous) @ foo.js:2
// ...
```