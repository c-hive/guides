# GitHub Actions / Issues and limitations

#### Cannot specify condition "tag-only" for a job

If you could use [job conditions](https://help.github.com/en/articles/contexts-and-expression-syntax-for-github-actions) to run a certain step only on a tag you could use the `needs` keyword to specify dependencies. Without this, these steps need to be ran again in the tag-only workflow.

```
jobs:
  publish:
  if: github.tag # <- this doesn't work
  needs: [test, build]
```

Workaround: create a separate workflow.

#### Cannot match regex in job conditions

For example if you want to match a versioning format on tags you can do:
```
on:
  push:
    tags:
    - 'v*'
```

But you cannot do:
```
on:
  push:
    tags:
    - '/^v[0-9]\.[0-9]\.[0-9]$/'
```

#### Cannot specify dependencies between workflows

This would allow for organizing workflows. For example the workflow `publish` would depend on the workflow `verify`, both of which have their set of jobs. You're left with either a single workflow (uncomfortably named something generic like `CI`) within which jobs can have the proper dependencies, or multiple workflows where certain steps are duplicated (e.g. you always want to run tests before publishing).

See also: https://github.community/t5/GitHub-Actions/Github-Actions-trigger-workflow-on-Succes-Failure-of-other/td-p/17944
