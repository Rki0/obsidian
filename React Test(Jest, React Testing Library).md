# React Test Library vs Jest
- React Test Library(RTL)
	- provides virtual DOM for tests
- Jest
	- Test runner that
		- Finds tests
		- Runs tests
		- Determines whether tests pass or fail

# syntax
- `render`
	- Create virtual DOM for argument JSX
	- Access virtual DOM via `screen` global
- `screen.getByText()`
	- Find element by display text
- `expect().toBeInTheDocument()`
	- assertion, causes test to succeed or fail

# Assertions
- `expect`
	- Jest global, starts the assertion
	- argument
		- subject of the assertion
- matcher
	- type of assertion
	- this matcher comes from Jest-DOM
	- matcher argument
		- refines matcher

```js
expect(expect argument).matcher()

ex)
expect(likeElement).toBeInTheDocument()
expect(element.textContent).toBe("hello");
expect(elementArray).toHaveLength(7);
```

## jest-dom
- comes with create-react-app
- `src/setupTests.js` imports it before each test, makes matchers available
- DOM-based matchers

# Jest
- RTL helps with
	- rendering components into virtual DOM
	- searching virtual DOM
	- Interacting with virtual DOM
- Needs a test runner
	- Find tests, run them, make assertions
- Jest
	- is recommended by Testing Library
	- comes with create-react-app
## Watch Mode
- Watch for changes in files since last commit
- Only run tests related to these files
- No changes? No tests.
## How does Jest work?
- global `test` method has two arguments : 
	- string description
	- test function
- Test fails if error is thrown when running function
	- assertions throw errors when expectation fails
- No error -> tests pass
	- Empty test passes!
