# Testing

## Unit testing

### Naming

- Name of testfile should be the same as name of tested file. e.g. `getProxyURL.js` => `getProxyURL.test.js`
- Test are described in 3 parts: `describe`, `context`, `it`
  - `describe` simply states the name of the component / function
  - `context` elaborates on additional state / setup
    - only if needed
    - always starts with `when`, `with` or `without`
    - forms a sentence with proper grammar when composed with `it` block descriptions
    - uses `describe` function
    - does *not* describe action, only state (e.g. correct: when button is inactive. incorrect: when button is clicked)
    - usually has a counter-example with the opposite state
  - `it` states what the tested piece of logic (referenced by describe) does in the given context

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

### What to test

- Conditionals (both sides)
- State and its changes
- What should be rendered (e.g. `expect(wrapper.find(MyComponent)).toHaveLength(1);`)
