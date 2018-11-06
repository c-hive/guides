# Way of Working

## Tools

Currently in the GitHub universe, however GitLab seems very promising with built-in CI, time tracking, code quality tools.

VC platform:
- GitHub

Issue tracking:
- GitHub issues with labels
- keep technical discussion on GitHub

Time tracking:
- record time spent on the issue before closing
- record total time spent in a shared space, optionally [Clockify](https://clockify.me/)

Communication:
- Slack
- Secondary: facebook, discord

CI:
- CircleCI
- steps: test, report coverage, lint, deploy if on master

Code quality:
- ESLint for linting
- Prettier for formatting
- Codecov for coverage

PaaS:
- GitHub pages
- Heroku

## Development workflow

- Work on feature branches
- Prefix branch with GitHub issue
- Everything is reviewed by at least 1 person (ideally before merge but that's subject to dev availability)
- Use "squash and merge"
- Remove branch on GitHub after merge to keep the repo thin

## Pre-commit hooks

It's a good practice to do "clean" commits where the CI is always expected to pass. It's useful for e.g.
- to be able to revert to any state of master (depending on the merge strategy this isn't always possible with failing commits)
- assuring you and others that the work can be continued without having to wait for the CI

One way to force checks before committing is via pre-commit hooks. They [cannot easily be bound to the repo via git](https://stackoverflow.com/questions/427207/can-git-hook-scripts-be-managed-along-with-the-repository) therefore we use this package: https://github.com/observing/pre-commit

Currently `test` and `lint` steps. They can be bypassed if needed via `--no-verify`.

## Branches

- master (production or staging, preview)
- feature/working branches
- release (if needed)

## Design

- draw.io - great for diagrams, ok for wireframes
- balsamiq.cloud - great for wireframes
