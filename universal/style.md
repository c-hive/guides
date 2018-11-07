# Universal style guide

## Code style

In most general terms: follow Clean Code. This phrase is often thrown around but here it refers to the book and video series of Robert C. Martin (a.k.a. uncle Bob). This is a useful read / binge if you haven't already.

- Code style in projects is unified, using a common linter and style guide.
- Comments in code are avioded and used only in exceptional cases.

## Commit messages

- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/) by Chris Beams

## Branches

- use snake case
- are prefixed with issue number and a dash, e.g. `issue#4-create_comething_fancy`

## Unit tests

- [Better specs](http://www.betterspecs.org/) (although created for RSpec and Ruby) contains many good general principles
- [Rubocop's guide on test naming](https://github.com/rubocop-hq/rspec-style-guide#naming) (although created for RSpec and Ruby) contains many good general principles

- Test coverage aim: 100%. This doesn't mean that the build should fail if it's 99, but specifying a lower aim makes no sense as the goal is to have everything covered
- Tests should be written to test, not to increase coverage
- Their scope should be very limited (to not bleed into other stuff, both functionality and coverage wise) and it should be clear what they test

## File structure and naming

- Names should be simple, clear and intuitive
- Follow language/framework specific guides

### Special files

- Repos contain a README file and a COPYRIGHT or LICENSE file.


## Documentation

- The README gets newcommers up to speed and helps them get the app up & running
