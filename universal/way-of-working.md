# Way of Working

## Requirements

- [RFC memo about key words used to Indicate Requirement Levels](https://tools.ietf.org/html/rfc2119)

## Tools

They might differ on project-to-project basis but all should be present in some form.

### Personal
Grammarly or something similar for correct spelling and grammar.

### Development platform

#### Open source

[GitHub](https://github.com/) has the best reach and is the go-to platform for open source projects.

#### Closed source

[GitHub](https://github.com/) has their [own CI](https://github.blog/changelog/2019-11-11-github-actions-is-generally-available/) which is also available for self-hosting. This mitigates the possibility of leaking code through various third party services and puts them on par with [GitLab](https://gitlab.com/). They have various security features ([1](https://github.blog/2019-11-14-announcing-github-security-lab-securing-the-worlds-code-together/), [2](https://github.blog/changelog/2019-11-14-automated-updates/), [3](https://github.blog/changelog/2019-11-14-security-advisories-generally-available-can-request-cves/)) and we found thier service to be much more stable than GitLab.

### Issue tracking
- Dev platform's issue tracker
- Keep decisions and (excerpts of) technical discussion in the issues

### Time tracking
- Record time spent (referencing the topic) in a shared space, e.g. [Clockify](https://clockify.me/)

### Communication:
- [Slack](https://slack.com/)

### CI
- GitHub Actions

### Code quality
- Linter
- Coverage
  - For coverage-critical projects the threshold must be set at 100% and specific parts may be exlcuded from coverage with an explanation

### Deployment platforms
- Heroku
- GitHub pages

## CI steps

- lint
- test
- report coverage
- build
- deploy

## Development workflow

- Work on feature branches
- Include issue reference in branch name
- Review PRs before merge
- Remove merged branches to keep the repo thin

### Pre-commit hooks

It's a good practice to do "clean" commits where the CI is always expected to pass. It's useful to e.g.
- be able to revert to any state of master (depending on the merge strategy this isn't always possible with failing commits)
- ensure that the work can be continued without having to wait for the CI

One way to force checks before committing is via pre-commit hooks. Nowadays there're libraries that support storing this configuration in the repo.

Run at least `test` and `lint` steps.

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
