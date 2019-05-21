# Firebase / Miscellaneous

## Best practices

#### Functions should return `Promise` when permorming asynchronous tasks

GOOD

```js
exports.getUser = functions.https.onRequest((req, res) => {
    // return admin.database() ...
});
```

BAD

```js
exports.getUser = functions.https.onRequest((req, res) => {
    // => Function returned undefined, expected Promise or value.
    // admin.database() ...
});
```

#### Define restful, explicit API routes with Express

```js
// GOOD => /users/:uid
// BAD => /getUser?uid={UID}

const app = require("express")();

app.get("/users/:uid", function(req, res) => {
    // ...
});

exports.api = functions.https.onRequest(app);
```