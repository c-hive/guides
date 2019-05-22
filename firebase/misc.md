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

#### Define RESTful API routes - [*Designing Web APIs using Google Firebase Functions: Achieving True Routing*](https://medium.com/@atbe/firebase-functions-true-routing-2cb17a5cd288)

GOOD

```js
const app = require("express")();

// ...

app.get("/users/:uid", function(req, res) => {
    // ...
});

exports.api = functions.https.onRequest(app);

// => /users/npWxk05ZCKMcYb0OaDSJffYQZZq1
```

BAD

```js
exports.getUser = functions.https.onRequest((req, res) => {
    const uid = req.query.uid;

    // ...
});

// => /getUser?uid=npWxk05ZCKMcYb0OaDSJffYQZZq1
```