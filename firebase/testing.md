# Firebase / Testing

### Packages

#### `firebase-server`

Setup and close the server gracefully with the help of global test hooks.

```js
// Mocha
// => global-hooks.js

const FirebaseServer = require("firebase-server");

let firebaseServer;

before(() => {
    firebaseServer = new FirebaseServer(5000, "localhost");
});

after(async () => {
    await firebaseServer.close();
});
```

#### `firebase-functions-test`

Further investigations are required.

### Best practices

#### Empty the database among the tests

```js
// mocha-global-hooks.js

beforeEach(() => {
    database.ref.remove();
});
```