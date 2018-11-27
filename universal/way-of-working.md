# Way of Working

Deploying to GH from GL:
GH: deploy keys
GL: https://docs.gitlab.com/ee/ci/ssh_keys/

## Tools

They might differ on project-to-project basis but all should be present in some form.

### Development platform

#### Open source

[GitHub](https://github.com/) has the best reach and is the go-to platform for open source projects.

#### Private

Integrating private projects with various third party services (e.g CI, code quality tools) poses additional security risk. Self-hosting them requires additional effort for each tool and they might not even be avaialble as self-hosted. [GitLab](https://gitlab.com/) on the other hand has a good selection of tightly integrated services. By default they all work within GitLab reducing security point of failures and they are all integrated into the same runtime environment (GitLab Runner) which makes it easier to set them up as self hosted. Arguably it also has the best price/value.

#### Interoperability between GitHub and GitLab

Migrating to GitLab is well supported. PRs, issues, tags will be kept. It also supports [bidirectional mirroring](https://docs.gitlab.com/ee/workflow/repository_mirroring.html).

Migrating to GitHub is restricted to importing the repository.

One use case to consider is to have private development on GitLab, but deploy to GitHub Pages and handle external issues also on GitHub to reach more people. This can be achieved by [using a GitHub deploy key in GitLab CI](https://docs.gitlab.com/ee/ci/ssh_keys/).

### Issue tracking
- Dev platform's issue tracker
- Keep decisions and (excerpts of) technical discussion in the issues

### Time tracking
- Record time spent on the issue before closing, dev platform might also offer features for it
- Record total time spent (also referencing topic) in a shared space, optionally [Clockify](https://clockify.me/)

### Communication:
- Slack
- Secondary, mainly for voice: discord

### CI
- CircleCI for GitHub
- built-in CI for GitLab

### Code quality
- Linter
- Coverage

### Deployment platform
- Heroku
- GitHub pages

## CI steps

- test
- report coverage
- lint
- build
- deploy

## Development workflow

- Work on feature branches
- Prefix branch with GitHub issue
- Everything is reviewed by at least 1 person (ideally right before merge but that's subject to dev availability)
- Use "squash and merge"
- Remove merged branches to keep the repo thin

### Pre-commit hooks

It's a good practice to do "clean" commits where the CI is always expected to pass. It's useful for e.g.
- to be able to revert to any state of master (depending on the merge strategy this isn't always possible with failing commits)
- assuring you and others that the work can be continued without having to wait for the CI

One way to force checks before committing is via pre-commit hooks. Nowadays there're libraries that support storing this configuration in the repo.

Run at least `test` and `lint` steps.

### Special branches

- master (production or staging/preview)
- release or tags on master (if master is used for staging/preview)

## Design

### Types

- technical design
- for FE
  - wireframe design
  - functional / interaction design
  - visual design

### Tools

- draw.io - diagrams, wireframes
- balsamiq.cloud - wireframes
- figma - visual
