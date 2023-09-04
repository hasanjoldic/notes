# Javascript testing best practices

## Design for lean testing

Testing code is not production-code - Design it to be short, dead-simple, flat, and delightful to work with. One should look at a test and get the intent instantly.

## Categorize tests under at least 2 levels

Apply some structure to your test suite so an occasional visitor could easily understand the requirements (tests are the best documentation) and the various scenarios that are being tested. A common method for this is by placing at least 2 `describe` blocks above your tests:

1. the 1st is for the name of the unit under test
2. 2nd is for an additional level of categorization like the scenario or custom categories.

```js
// Unit under test
describe("Transfer service", () => {
  //Scenario
  describe("When no credit", () => {
    //Expectation
    test("Then the response status should decline", () => {});

    //Expectation
    test("Then it should send email to admin", () => {});
  });
});
```

## AAA pattern

Structure your tests with 3 well-separated sections Arrange, Act & Assert (AAA). Following this structure guarantees that the reader spends no brain-CPU on understanding the test plan:

1. Arrange: All the setup code to bring the system to the scenario the test aims to simulate.

2. Act: Execute the unit under test. Usually 1 line of code

3. Assert: Ensure that the received value satisfies the expectation. Usually 1 line of code

```js
describe("Customer classifier", () => {
  test("When customer spent more than 500$, should be classified as premium", () => {
    //Arrange
    const customerToClassify = { spent: 505, joined: new Date(), id: 1 };
    const DBStub = sinon.stub(dataAccess, "getCustomer").reply({ id: 1, classification: "regular" });

    //Act
    const receivedClassification = customerClassifier.classifyCustomer(customerToClassify);

    //Assert
    expect(receivedClassification).toMatch("premium");
  });
});
```

## Describe expectations in a product language: use BDD-style assertions

Coding your tests in a declarative-style allows the reader to get the grab instantly without spending even a single brain-CPU cycle. When you write imperative code that is packed with conditional logic, the reader is forced to exert more brain-CPU cycles.

Wrong:

```js
test("When asking for an admin, ensure only ordered admins in results", () => {
  //assuming we've added here two admins "admin1", "admin2" and "user1"
  const allAdmins = getUsers({ adminOnly: true });

  let admin1Found,
    adming2Found = false;

  allAdmins.forEach(aSingleUser => {
    if (aSingleUser === "user1") {
      assert.notEqual(aSingleUser, "user1", "A user was found and not admin");
    }
    if (aSingleUser === "admin1") {
      admin1Found = true;
    }
    if (aSingleUser === "admin2") {
      admin2Found = true;
    }
  });

  if (!admin1Found || !admin2Found) {
    throw new Error("Not all admins were returned");
  }
});
```

Right:

```js
it("When asking for an admin, ensure only ordered admins in results", () => {
  //assuming we've added here two admins
  const allAdmins = getUsers({ adminOnly: true });

  expect(allAdmins)
    .to.include.ordered.members(["admin1", "admin2"])
    .but.not.include.ordered.members(["user1"]);
});
```

## Stick to black-box testing: Test only public methods

## Choose the right test doubles

Before using test doubles, ask a very simple question: Do I use it to test functionality that appears, or could appear, in the requirements document? If not, it’s a white-box testing smell.

## Don’t “foo”, use realistic input data

## Test many input combinations using Property-based testing

[fast-check](https://www.npmjs.com/package/fast-check)

## Don’t catch errors, expect them

```js
it("When no product name, it throws error 400", async () => {
  await expect(addNewProduct({}))
    .to.eventually.throw(AppError)
    .with.property("code", "InvalidInput");
});
```

## Tag your tests

Different tests must run on different scenarios: quick smoke, IO-less, tests should run when a developer saves or commits a file, full end-to-end tests usually run when a new pull request is submitted, etc. This can be achieved by tagging tests with keywords like `#cold` `#api` `#sanity` so you can grep with your testing harness and invoke the desired subset. For example, this is how you would invoke only the sanity test group with Mocha: `mocha — grep "sanity"`.

```js
//this test is fast (no DB) and we're tagging it correspondigly
//now the user/CI can run it frequently
describe("Order service", function() {
  describe("Add new order #cold-test #sanity", function() {
    test("Scenario - no currency was supplied. Expectation - Use the default currency #sanity", function() {
      //code logic here
    });
  });
});
```

## Ensure new releases don’t break the API using contract tests

[PACT](https://docs.pact.io/)

## Test your middlewares in isolation

```js
const httpMocks = require("node-mocks-http");

const unitUnderTest = require("./middleware");

test("A request without authentication header, should return http status 403", () => {
  const request = httpMocks.createRequest({
    method: "GET",
    url: "/user/42",
    headers: {
      authentication: ""
    }
  });
  const response = httpMocks.createResponse();
  unitUnderTest(request, response);
  expect(response.statusCode).toBe(403);
});
```

## Test for crash recoveries

## Avoid global test fixtures and seeds, add data per-test

## Isolate the component from the world using HTTP interceptor

```js
// Intercept requests for 3rd party APIs and return a predefined response 
beforeEach(() => {
  nock('http://localhost/user/').get(`/1`).reply(200, {
    id: 1,
    name: 'John',
  });
});
```

## Separate UI from functionality

## Whenever possible, test with a realistic and fully rendered component

## Don't sleep, use frameworks built-in support for async events. Also try to speed things up

## Have very few end-to-end tests that spans the whole system

## Speed-up E2E tests by reusing login credentials

## Have one E2E smoke test that just travels across the site map

```js
it("When doing smoke testing over all page, should load them all successfully", () => {
  // exemplified using Cypress but can be implemented easily
  // using any E2E suite
  cy.visit("https://mysite.com/home");
  cy.contains("Home");
  cy.visit("https://mysite.com/Login");
  cy.contains("Login");
  cy.visit("https://mysite.com/About");
  cy.contains("About");
});
```

## Expose the tests as a live collaborative document

[cucumber](https://www.npmjs.com/package/@cucumber/cucumber)
[storybook](https://www.npmjs.com/package/storybook)

## Enrich your linters and abort builds that have linting issues

## Shorten the feedback loop with local developer-CI

## Perform e2e testing over a true production-mirror

## Fail early, run your fastest tests first
