# Testing

## Unit testing

- [Better specs](http://www.betterspecs.org/) (although created for RSpec and Ruby) contains many good general principles
- [Rubocop's guide on test naming](https://github.com/rubocop-hq/rspec-style-guide#naming) (although created for RSpec and Ruby) contains many good general principles
- Test coverage aim: 100%. This doesn't mean that the build should fail if it's 99, but specifying a lower aim makes no sense as the goal is to have everything covered
- Tests are written to test, not to increase coverage
- Their scope is very limited (to not bleed into other code, both functionality and coverage wise) and it is clear what they test

### Naming

- Name of testfile is the same as name of tested file. Additional suffex might be added as per framework conventions. E.g.
  - JS with Jest: `getProxyURL.js` => `getProxyURL.test.js`
  - Ruby with RSpec: `get_proxy_url.js` => `get_proxy_url_spec.js`
- Test are described in 3 parts: `describe`, `context`, `it`
  - `describe` simply states the name of the component / function
  - `context` elaborates on additional state / setup
    - only if needed
    - always starts with `when`, `with` or `without`
    - forms a sentence with proper grammar when composed with `it` block descriptions
    - uses `context` function (or `describe` if the former is not available)
    - does *not* describe action, only state (e.g. correct: when button is inactive. incorrect: when button is clicked)
    - usually has a counter-example with the opposite state
  - `it` states what the tested piece of logic (referenced by describe) does in the given context

Practical example:
```js
`describe('getProxyURL', () => {
  describe('when CORS_PROXY_ENABLED env var is true', () => {
    it('returns the given URL with a CORS proxy', () => {
      # test
    });
  });
});
```

This read nicely as: `describe getProxyURL: when CORS_PROXY_ENABLED env var is true, it returns the given URL with a CORS proxy`

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

### Independence of tests

Tests are always independent of each other.

Best practices include
- using per test case context setups (i.e. before each, not before all)
- resetting common storages between tests (e.g. DB, locale storage)
- resetting mocks between tests

It is enforced by running tests isolated or in a randomized order.

### Mocking / stubbing / spying

Should be used
- to skip unreliable / slow parts of the code (e.g. network requests)

Could be used
- to avoid testing code _written for the project_ which is not the subject of the test
- to avoid indeterminate functionality

GOOD

```
it "creates user with a unique key" do
  key = 'dummy_unique'
  allow(UniqueGenrator).to receive(:hex).and_return(key)
  
  expect(User.create.key).to eq(key)
end
```

Should _not_ be used
- unless there's a reason to use them (a.k.a. this is not the default approach to testing)
- to skip 3rd party code (e.g. DB, framework)
- as a replacement for verifying return values

GOOD

```
get :index
expect(response).to be_successful
```

BAD

```
expect(response).to receive(:send_status).with(200)
get :index
```

- as a replacement for verifying side effects

GOOD

```
deleteUser(id)
expect(User.find(id)).not_to be
```

BAD

```
expect(User).to receive(:delete).with(id)
deleteUser(id)
```
