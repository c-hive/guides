# Javascript / Best practices

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
