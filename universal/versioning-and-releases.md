# Universal / Versioning and releases

- Use [Semantic versioning](http://semver.org/)

## Branches
- Use separate branches for development and releases/production

## Releases
- Tag version, e.g. `v1.3.1`

## Artifacts
- Name pattern: `<app-name>-<version>-<revision?>-<type?>.<extension>`, e.g.
  - `my-app-v0.0.1.exe`
  - `my-app-v1.3.1-setup.exe`
  - `my-app-v2.0.0-5-standalone.exe`
  - `my-app-v5.3.0-88-gcc7b71f-standalone.exe`

The following command returns version and revision from git:

```bash
git describe --tags --exact-match 2> /dev/null || git describe --always
```

It returns
- commit hash as revision when no tagging is used
- tag name as version when on a tag
- tag name, revision number since last tag and commit hash when after a tag
