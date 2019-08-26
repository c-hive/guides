# Firebase / Best practices

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

See also: [Designing Web APIs using Google Firebase Functions: Achieving True Routing](https://medium.com/@atbe/firebase-functions-true-routing-2cb17a5cd288)

GOOD

```js
const app = require("express")();

// ...

app.get("/users/:uid", function(req, res) => {
    // ...
});

exports.api = functions.https.onRequest(app);

// => /api/users/npWxk05ZCKMcYb0OaDSJffYQZZq1
```

BAD

```js
exports.getUser = functions.https.onRequest((req, res) => {
    const uid = req.query.uid;

    // ...
});

// => /getUser?uid=npWxk05ZCKMcYb0OaDSJffYQZZq1
```

#### Let promises get rejected when handling errors unless there is a fallback strategy

GOOD

```js
exports.signup = functions.auth.user().onCreate(user =>
  Model.create(user)
    .then(() => Service.createCustomer(user))
    .catch(err => {
      console.error(JSON.stringify(err));

      // Re-throw the error to fail the Promise explicitly.
      throw err;
    })
);

// => Function execution took 60 ms, finished with status: 'error'
```

BAD

```js
exports.signup = functions.auth.user().onCreate(user =>
  Model.create(user)
    .then(() => Service.createCustomer(user))
    .catch(err => {
      console.error(JSON.stringify(err));
    })
);

// => Function execution took 60 ms, finished with status: 'ok'
```
