# Universal / Error handling

An error can be categorized by many criteria, whether
- it's **critical software** or it's **non-critical software**
- it's **user facing** or **developer facing**
- there **is a fallback** or **there is no fallback**
- it's on the **frontend** or **backend**
- the code is **synchronous** or **asynchronous**
- a certain language or library is used (specific guides: [Javascript](../js/error-handling.md))

These are general rules aimed to help making a decision about how to handle certain errors. For errors texts refer to the [wording guideline](https://github.com/c-hive/guides/issues/2). Also check the [Javascript error handling](../js/error-handling.md) guide.

## Critical software

There's always a fallback strategy. Errors are always reported. Possible bugs are considered for graceful error handling.

- Example: bank system

## Non-critical software

Bugs are not considered for graceful error handling. There isn't always a fallback strategy but errors should be reported.

## User facing error

Often happens on the frontend.

#### By default, the user must be notified and a fallback strategy must be implemented.

- Example: a form validation failed on post. The validation error is shown, the field is higlighted and the previous value may or may not be removed.

- Example: worst case, some unexpected error happens. A generic error is shown: "Something unexpected happened, save your changes offline and [reload the page](error-handling.md)."

#### In certain (specifically identified) cases the user may not be notified.

- Example: an async fethcing keeps the website up to date with other changes but it fails. This may fail silently in the bakckground (but must be handled explicitly). Fallback strategy could be retrying a few times and, if it keeps failing, showing a generic error to the user: "The page is running with limited functionality, [reloading the page](error-handling.md) might help resolve it."

#### Expose the minimal necessary information.

- Example: GOOD: "Something unexpected happened", BAD: "JSON.parseError"
- Example: GOOD: "The page is running with limited functionality", BAD: "XYZ feature is not available"

#### Consecutive errors must be distinguishable.

- Example: BAD: An AJAX form validation failed on post, the field is highlighted. The user edits the field and resends the form. Throughout this, the error highlight is always visible. The user cannot tell if there's another error or if it's just "stuck".

- Example: GOOD: An AJAX form validation failed on post, the field is highlighted. As the user changes the value the highlight is removed. Upon resubmission, if the value is still incorrect the highlight appears again.

- Example: GOOD: Input validation failed on post, there's an error bubble. The bubble has a fade-in animation, it can be dismissed and it is automatically dismissed upon a new submission. If the value is still incorrect the bubble shows up again using the same fade-in animation.

## Developer facing error

Often happens on the backend. In certain cases a generic fallback strategy should be implemented (e.g. retry).

#### Expose as much information as possbile.

## Best practices and anti-patterns

#### Fail hard unless there's a fallback strategy

GOOD

```js
thisMightThrow();
// continue...
```

BAD

```js
try {
  thisMightThrow();
}
catch (err) {
  console.error(err);
}
// continue...
```

#### Fail early

GOOD

```js
function notifyNewUser(user) {
  if (!user) throw "User doesn't exist";
}
```

BAD

```js
function notifyUser(user) {
  if (!user) return null;
}
```

#### Make sure the right error is being handled

And not every possible error.

GOOD

```js
updateUser(err => {
  if (!err) return;

  if (err.name === "UserNotFound") {
    console.error("Cannot update user", err);
    handleUserNotFound();
  } else {
    console.error("Description of the error", err);
  };
};
```

BAD

```js
updateUser(err => {
  if (!err) return;

  console.error("Cannot update user", err);
  handleUserNotFound();
};
```

#### Make sure no information is lost from the original error

GOOD

```js
createUser(err => {
  if (err) console.error("User couldn't be created", err);
};
```

GOOD

```js
class CustomError extends Error {
  constructor(error) {
    // a proper copy constructor
  }
}
// ...
createUser(err => {
  if (err) throw new CustomError(err);
};
```

BAD

```js
createUser(err => {
  if (err) console.error("Cannot create user");
};
```

BAD

```js
createUser(err => {
  if (err) throw new CannotCreateUserError();
};
```

#### Define generic purpose error classes, provide details as error message

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

#### Use proper error codes

GOOD

```js
if (err) {
  if (err typeof RecordNotFound) {
    res.status(404);
    throw err;
  } else {
    throw err;
  }
}
```

BAD

```js
if (err) {
  return res.status(400);
}
```
