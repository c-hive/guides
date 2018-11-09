# Way of Working

## Tools

These are configured for each project. They might differ on project-to-project basis.

GitHub is better for open source projects because of reach. GitLab seems better for private projects because of the bigger selection of tightly integrated services (therefore less configuration, security point of failures) and pricing.

We're currently still in the GitHub universe with every project.

#### Development platform
- GitHub
- GitLab

#### Issue tracking
- Dev platform's issue tracker
- Keep decisions and (excerpts of) technical discussion in the issues

#### Time tracking
- Record time spent on the issue before closing, dev platform might also offer features for it
- Record total time spent (also referencing issues) in a shared space, optionally [Clockify](https://clockify.me/)

#### Communication:
- Slack
- Secondary, mainly for voice: discord, facebook

#### CI
- CircleCI for GitHub
- built-in CI for GitLab

#### Code quality
- Linter
- Coverage

#### Deployment platform
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
- release (if master is used for staging/preview)

## Design

### Types

- technical design
- for FE
  - wireframe design
  - functional / interaction design
  - visual design

### Tools

- draw.io - great for diagrams, ok for wireframes
- balsamiq.cloud - great for wireframes
