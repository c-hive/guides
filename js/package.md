# Javascript / Package development

- [README template](package-readme-template.md)

## What to include

Packages [only include](https://docs.npmjs.com/misc/developers#keeping-files-out-of-your-package) production source code and a few mandatory files (included by default). The [safe way](https://medium.com/@jdxcode/for-the-love-of-god-dont-use-npmignore-f93c08909d8d) to handle it is via `package.json`'s `files` key which looks something like this:
```json
{
  "files": [
    "/lib/**/*.js",
    "!/lib/**/*.test.js",
    "/lib/**/*.json",
    "/lib/**/*.svg"
  ],
}
```

The source code is included as-is, it is [not transpiled or minified](https://github.com/flexdinesh/npm-module-boilerplate/issues/5).
