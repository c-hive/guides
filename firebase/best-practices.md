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

#### Let the `Promise` fail to construct meaningful logs in Firebase unless there is a fallback strategy

GOOD

```js
exports.signup = functions.auth.user().onCreate(user => 
  Model.create(user)
    .then(() => Service.createCustomer(user))
);

// ERROR => Function execution took 60 ms, finished with status: 'error'
```

GOOD

```js
exports.signup = functions.auth.user().onCreate(user =>
  Model.create(user)
    .then(() => Service.createCustomer(user))
    .catch(err => {
      console.error(JSON.stringify(err));

      // Re-throw the error to fail the Promise explicitly.
      throw new ExampleError(err);
    })
);

// ERROR => Function execution took 60 ms, finished with status: 'error'
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

// ERROR => Function execution took 60 ms, finished with status: 'ok'
```
