# Javascript / Error handling

## Best practices and anti-patterns

#### A practical example of writing custom error classes

GOOD

```js
class ExampleError extends Error {
  constructor(...args) {
    super(...args);

    // Ensures that the error constructor isn't included in the stack trace.
    Error.captureStackTrace(this, ExampleError);

    this.name = this.constructor.name;
  }
}
```

BAD

```js
class ExampleError extends Error {}
```

#### Throw Error objects

Otherwise debugging information is lost.

GOOD

```js
new Promise(function(resolve, reject) {
  throw new Error('Uh-oh!');
}).catch(function(error) {
  console.error(error);
});

// => Error: Uh-oh!
//     at <anonymous>:2:9
//     at new Promise (<anonymous>)
//     at <anonymous>:1:16
```

BAD

```js
new Promise(function(resolve, reject) {
  throw 'Uh-oh!';
}).catch(function(error) {
  console.error(error);
});

// => Error: Uh-oh!
```

#### Do not serialize error classes

GOOD

```js
new Promise(function(resolve, reject) {
  throw new Error('Uh-oh!');
}).catch(function(error) {
  console.error(error);
});

// => Error: Uh-oh!
//      at <anonymous>:2:44
//      at new Promise (<anonymous>)
//      at <anonymous>:2:1
//      ...
```

BAD

```js
new Promise(function(resolve, reject) {
  throw new Error('Uh-oh!');
}).catch(function(error) {
  console.error(JSON.stringify(error));
});

// => {}
```

BAD

```js
new Promise(function(resolve, reject) {
  throw new ExampleError('Uh-oh!');
}).catch(function(error) {
  console.error(JSON.stringify(error));
});

// => {"name":"ExampleError"}
```

#### Serialize objects when wrapping into error classes

Otherwise the message cannot be retreived.

GOOD

```js
new Promise(function(resolve, reject) {
  throw new Error(JSON.stringify({
    message: 'Uh-oh!'
  }));
}).catch(function(error) {
  console.error(error);

  // => Error: {"message":"Uh-oh!"}
  //      at <anonymous>:68:50
  //      at new Promise (<anonymous>)
  //      ...

  console.error("Uh! " + error);

  // => Uh! Error: {"message":"Uh-oh!"}
});
```

BAD

```js
new Promise(function(resolve, reject) {
  throw new Error({
    message: 'Uh-oh!'
  });
}).catch(function(error) {
  console.error(error);

  // => Error: [object Object]
  //      at <anonymous>:58:50
  //      at new Promise (<anonymous>)
  //      ...

  console.error("Uh! " + error);

  // => Uh! Error: [object Object]
});
```

#### Define reusable error classes, provide details as error message

GOOD

```js
class RecordAlreadyExistsError extends Error {
  // ...
}

if (userExists(user)) {
  throw new RecordAlreadyExistsError(JSON.stringify(user) + " already exists.");
}
```

BAD

```js
class UserError extends Error {
  // ...
}

if (userExists(user)) {
  throw new UserError("Already exists.");
}
```

#### Rejected Promises must always be catched and at least logged

Otherwise information might be lost.

GOOD

```js
new Promise(function(resolve, reject) {
  throw new Error('Uh-oh!');
}).catch(function(error) {
  console.error(error);
});

// => Error: Uh-oh!
//     at <anonymous>:2:9
//     at new Promise (<anonymous>)
//     at <anonymous>:1:16
```

BAD

```js
new Promise(function(resolve, reject) {
  throw new Error('Uh-oh!');
});

// => (node:12977) UnhandledPromiseRejectionWarning: Unhandled promise rejection (rejection id: 1): Error: Uh-oh!
```