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

```js
test("renders learn react link", () => {
	throw new Error("test failed");
});
```

# TDD(Test-Driven Development)
- Write tests before writing code
	- then write code according to "spec" set by tests
- "red-green" testing
	- Tests fail before code is written

1. Write "shell" function(내용은 없고 존재만 하는 함수, 컴포넌트를 의미)
2. Write tests
3. Tests fail(red)
4. Write code
5. Tests pass!(green)

- Make a huge difference in how it feels to write tests
	- part of the coding process, not a "chore" to do at the end
- More efficient
	- Re-run tests "for free"(자동으로) after changes

# React Test Library
- Creates virtual DOM for testing
	- and utilities for interacting with DOM
- Allows testing without a browser

# Types of Tests
- Unit tests
	- Test one unit of code in isolation
- Integration tests
	- How multiple units work together
- Functional tests
	- Tests a particular function of software
	- Unit tests랑 다른게 뭐지? 특정 코드 함수가 아닌 소프트웨어의 일반적인 동작을 테스트한다!
	- 가령, 데이터를 폼에 입력하고 제출을 클릭하면 소프트웨어가 특정 데이터 셋으로 바르게 동작하는 기능을 확인해야한다.
	- 그럼 Unit tests가 모인 거랑 같네요? 그렇긴하다 코드가 아닌 동작을 테스트하는 것에 초점이 맞춰져 있다고 보면 이해가 더 쉽다.
- Acceptance / End-to-End(E2E) tests
	- Use actual browser and server(Cypress, Selenium)

# Functional Testing
- Unit Testing
	- lsolated : mock dependencies, test internals
	- Very easy to pinpoint failures
	- Further from how users interact with software
	- More likely to break with refactoring

- Functional Testing
	- Include all relevant units, test behavior
	- Close to how users interact with software
	- Robust tets
	- More difficult to debug failing tests

# TDD vs BDD(Behavior-Driven Development)
- BDD is very explicitly defined
	- involves collaboration between lots of roles
		- developers, QA, business partners, etc
	- defines process for different groups to interact