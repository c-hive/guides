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

#### Define meaningful error classes

GOOD

```js
class RecordAlreadyExistsError extends Error {
  // ...
}
```

BAD

```js
class UserError extends Error {
  // ...
}
```

#### Pass additional, useful information along with the error message

GOOD

```js
class RecordAlreadyExistsError extends Error {
  // ...
}

if (userExists(user)) {
  throw new RecordAlreadyExistsError(user + " already exists.");
}
```

BAD

```js
class RecordAlreadyExistsError extends Error {
  // ...
}

if (userExists(user)) {
  throw new RecordAlreadyExistsError("Already exists.");
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
