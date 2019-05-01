# Package development

- [README template](package-readme-template.md)

## Keeping files out of the package

The package should only include production code and a few mandatory files (included by default). The proper way to handle it is via `package.json`'s `files` key which should look something like this:
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

Sources:
- https://docs.npmjs.com/misc/developers#keeping-files-out-of-your-package
- https://medium.com/@jdxcode/for-the-love-of-god-dont-use-npmignore-f93c08909d8d
