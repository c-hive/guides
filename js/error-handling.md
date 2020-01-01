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

#### Pass strings to Error constructors

Objects passed are converted to strings and can be lost.

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
  throw new Error({
    message: 'Uh-oh!'
  });
}).catch(function(error) {
  console.error(error);
});

  // => Error: [object Object]
  //      at <anonymous>:58:50
  //      at new Promise (<anonymous>)
  //      ...
```

```js
new Promise(function(resolve, reject) {
  throw new Error({
    message: 'Uh-oh!'
  });
}).catch(function(error) {
  console.error(JSON.stringify(error.message));
});

  // => "[object Object]"
```

#### Do not concatenate error logs with other objects

GOOD

```js
new Promise(function(resolve, reject) {
  throw new Error('Uh-oh!');
}).catch(function(error) {
  console.error('Error during something:');
  console.error(error);
});

// => Error during something:
//    Error: Uh-oh!
//      at <anonymous>:2:44
//      at new Promise (<anonymous>)
//      at <anonymous>:2:1
//      ...
```

#### Log error objects as-is

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


```js
new Promise(function(resolve, reject) {
  throw { message: 'Uh-oh!' };
}).catch(function(error) {
  console.error(error);
});

// => Object { message: "Uh-oh!" }
```

GOOD

```js
try {
  throw new Error('Uh-oh!');
} catch(error) {
  console.error(error);
}

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

#### Rejected Promises must always be caught and at least logged

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