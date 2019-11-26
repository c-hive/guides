# GitHub Actions / Best practices

#### Specify timeout

The [default timeout is 360 minutes](https://help.github.com/en/articles/workflow-syntax-for-github-actions#jobsjob_idtimeout-minutes) which is most likely more than needed. Each job must have an explicit time constraint.

GOOD

```yml
name: Verify

on: [push]

jobs:
  test:
    runs-on: ubuntu-18.04
    timeout-minutes: 10
    steps:
    - uses: actions/checkout@v1
```

#### Add support for skipping CI

https://github.com/actions/bin/issues/39#issuecomment-531562107

#### Print current ref

At the moment, when using `on: [push]` [it's not possible to distingish an on-tag-push build from an on-commit-push build](https://github.community/t5/GitHub-Actions/Differentiate-between-tag-and-non-tag-builds/m-p/39540). There can be additional actions on a tag build, such as releasing. In this case, to make the difference clear in the logs, add a step to print the current ref.

GOOD

```yml
name: Verify

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - name: Print ref
      run: echo ${{ github.ref }}
```
