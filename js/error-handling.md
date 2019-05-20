# Javascript / Error handling

## Best practices and anti-patterns

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
