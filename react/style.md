# Style guide

In most general terms: follow Clean Code. This phrase is often thrown around but I refer explicitly to the book and video series of Robert C. Martin (a.k.a. uncle Bob). This is a useful read / binge if you haven't already.

Any useful rule is fine as long as it's understood by everyone in the team.

Use your best judgement and make exceptions when needed.


## Code style

- Code style is unified, using a common linter and style guide.
- Comments in code are avioded and used only in exceptional cases.


## Commit messages

- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/) by Chris Beams


## Unit testing

- Unit testing coventions (for Ruby but many apply in general): [Better specs](http://www.betterspecs.org/), [Rubocop guide on naming](https://github.com/rubocop-hq/rspec-style-guide#naming)
- Test coverage aim: 100%. This doesn't mean that the build should fail if it's 99, but specifying lower as a goal makes no sense IMO as the goal is to have everything covered.
- Tests should be written to test, not to increase coverage. :) Their scope should be very limited (to not bleed into other stuff, both functionality and coverage wise) and it should be clear what they test.

### Naming conventions

- Name of testfile should be the same as name of tested file. e.g. `getProxyURL.js` => `getProxyURL.test.js`
- Test are described in 3 parts: `describe`, `context`, `it`
  - `describe` simply states the name of the component / function
  - `context` elaborates on additional state / setup
    - only if needed
    - always starts with `when` or `with`
    - use `describe` function
    - does *not* describe action, only state (e.g. correct: when button is inactive. incorrect: when button is clicked)
    - usually has a counter-example with the opposite state
  - `it` states what the tested piece of logic (referenced by describe) does in the given context
- They should read well as a sentence

Practical example:
```js
`describe('getProxyURL', () => {
  describe('when CORS_PROXY_ENABLED env var is true', () => {
    it('returns the given URL with a CORS proxy', () => {
      # bla
    });
  });
});
```

This read as: `describe getProxyURL: when CORS_PROXY_ENABLED env var is true, it returns the given URL with a CORS proxy`

Always double check whether the test actually does what the description says. E.g.
```
describe ('delete button, () => {
  it ('removes the item`, () => {
    originalLength = items.length;
    wrapper.find('deleteButton').simulate('click');
    expect(items).toHaveLength(originalLength - 1);
  });
});
```
This test desciption is *not* correct. It checks that when the delete button is clicked on an item, it removes *an* item, not necessarily *the* item. Tests descriptions are there so that we don't have to look at the tests themselves, but that's only the case if they're accurate. It might be fine to have a test like the above, but the description should read e.g. `it descreases the number of items by 1`.

### What to test

- Conditionals (both sides)
- State and its changes
- What should be rendered (e.g. `expect(wrapper.find(MyComponent)).toHaveLength(1);`)


## File structure and naming conventions

- Names should be simple, clear and intuitive

### Casing

- Category folders use **snake_case**
  - e.g. `utils`, `components`
- Component folders, files and class names use **UpperCamelCase**
	- e.g. `LeagueSelector`, `UrlBuilder`
- Variables, functions use **lowerCamelCase**
	- e.g. `fetchLeagues`, `fetchItems`

### Special folders

- `src/components`
- `src/utils`
  - Non-UI / non-component / utility features not tightly linked to the specific project (e.g. `Api`, `CorsProxy`)
- `src/resources`
  - Machine readable, hardcoded data (e.g. `ItemTypes`, json data files)
- `src/common_styles`
  - css files applied to the whole project

### Special files

- Repos contain a COPYRIGHT or LICENSE and a README file.

### Grouping

- Files are grouped by functionality (components) in folders
- A component folder contains all related code (e.g. js, css, tests). See also: [Jest conventions](https://jestjs.io/docs/en/configuration.html#testregex-string)
- Snapshots currently need to be in a `__snapshots__` folder next to the tests, but [changes to that are already on master](https://github.com/facebook/jest/issues/1650)
- Files can be logically groupped into more folders (e.g. `selectors/LeagueSelector` and `selectors/ItemTypeSelector`)

### Example project structure

```
.
├── COPYRIGHT.txt
├── package.json
├── public
│   ├── assets
│   │   └── image.jpg
│   ├── favicon.ico
│   └── index.html
├── README.md
├── src
│   ├── common_styles
│   │   └── colors.css
│   ├── components
│   │   ├── App
│   │   │   ├── App.css
│   │   │   ├── App.js
│   │   │   ├── App.test.js
│   │   │   └── __snapshots__
│   │   │       └── App.test.js.snap
│   │   └── Header
│   │       ├── Header.css
│   │       ├── Header.js
│   │       ├── Header.test.js
│   │       └── __snapshots__
│   │           └── Header.spec.js.snap
│   └── utils
│       ├── testing
│       │   └── TestHelpers.js
│       └── proxy
│           └── GetProxyURL.js
└── webpack.config.js
```

Examples in the wild:
- https://github.com/btholt/photo-gallery


## Documentation

- The readme gets newcommers up to speed and helps them get the app up & running.
