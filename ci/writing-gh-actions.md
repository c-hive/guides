# GitHub Actions / Best practices for writing actions

#### Stringify objects when using `core.*` utilities to output logs

Objects are [expected to be supported](https://github.com/actions/toolkit/issues/386) sooner or later.

GOOD

```js
try {
  await run();
} catch (err) {
  core.setFailed(err.toString());
}

// => ::error::HttpError: Not Found
```

BAD

```js
try {
  await run();
} catch (err) {
  core.setFailed(err);
}

// => UnhandledPromiseRejectionWarning: TypeError: (s || "").replace is not a function
```

#### Use `actions-tagger` to automatically update major version tags

GitHub [recommends](https://docs.github.com/en/actions/creating-actions/about-actions#using-tags-for-release-management) to use semantic versioning and update the major version tag to the latest release. This action automates that:
- article: https://michaelheap.com/semantic-versioning-for-github-actions/
- action: https://github.com/marketplace/actions/actions-tagger
