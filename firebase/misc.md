# Firebase / Miscellaneous

## Best practices

#### Deployed functions should return `Promise` when performing asynchronous tasks

GOOD

```js
exports.signup = functions.https.onRequest((req, res) => {
    return admin.database()
        .ref("/users/" + req.query.id)
        .once("value")
        .then(() => {
            // ...
        });
});
```

BAD

```js
exports.signup = functions.https.onRequest((req, res) => {
    admin.database()
        .ref("/users/" + req.query.id)
        .once("value")
        .then(() => {
            // ...
        });

    // Firebase console => Function returned undefined, expected Promise or value.
});
```

#### Define RESTful API routes

GOOD

```js
const app = require("express")();

app.get("/users/:uid", function(req, res) => {
    // ...
});

exports.api = functions.https.onRequest(app);

// => /users/:uid
```

BAD

```js
exports.getUser = functions.https.onRequest((req, res) => {
    const uid = req.query.uid;

    // ...
});

// => /getUser?uid={UID}
```