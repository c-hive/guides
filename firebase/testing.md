# Firebase / Testing

### Testing with `firebase-server`

Testing frameworks usually provide a way to setup test hooks globally.

#### An example with Mocha

```js
// mocha-global-hooks.js

const FirebaseServer = require("firebase-server");

let firebaseServer;

before(() => {
    firebaseServer = new FirebaseServer(5000, "localhost");
});

after(async () => {
    await firebaseServer.close();
});
```

### Testing with `firebase-functions-test`

Further investigations are required.

### Empty the database among the tests

```js
// mocha-global-hooks.js

beforeEach(() => {
    database.ref.remove();
});
```