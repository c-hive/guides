# Testing

The naming and the unit testing are the same as the ones used in the [frontend](../react/testing.md).

##### What to test?
- Conditionals (both sides),
- API requests,
- Functions,
- Exports.

#### Examples
##### API Request
```
describe('GET /', () => {
	it('responds with the basic text', () => {
		request(app)
			.get('/')
			.expect(200)
			.end((req, res) => { ... });
	});
});
```

##### Export
```
it('exports an example function', () => {
	expect(ExampleFunction).to.be.a('function');
});
```