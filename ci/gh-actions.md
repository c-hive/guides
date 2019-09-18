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
