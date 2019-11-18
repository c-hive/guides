# Universal / Versioning and releases

- Use [Semantic versioning](http://semver.org/)

## Releases
- Tag releases with version, e.g. `v1.3.1`

## Artifacts
- Name pattern: `<app-name>-<revision>-<type?>.<extension>`, e.g.
  - `my-app-v0.0.1.exe`
  - `my-app-v1.3.1-setup.exe`
  - `my-app-v2.0.0-5-standalone.exe`
  - `my-app-v5.3.0-88-gcc7b71f-standalone.exe`

The following command returns version and revision from git:

```bash
git describe --always --tags --dirty
```

It returns
- commit hash as revision when no tagging is used (e.g. `gcc7b71f`)
- tag name as version when on a tag (e.g. `v2.1.0`, used for releases)
- tag name, revision number since last tag and commit hash when after a tag (e.g. `v5.3.0-88-gcc7b71f`)
- same as above plus a "dirty" tag if the working tree has local modifications (e.g. `v5.3.0-88-gcc7b71f-dirty`)

See also: https://www.git-scm.com/docs/git-describe#Documentation/git-describe.txt
