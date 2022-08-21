# Unit Testing in Angular

## Learning Goals

- Learning Goal 1

## Introduction

As we learned in other sections of this course, testing can take multiple forms,
with unit testing and end-to-end testing being 2 very specific types of tests:

1. With unit testing, we are focused on testing the functionality of a specific
   component
2. With end-to-end testing, we are focused on testing how the application works
   from the perspective of a user

Angular provides tools for unit testing out of the box, and has even created a
few test files for us while we were creating our components.

Jasmine is a testing framework that lets us define test suites and create
specific tests within those test suites. It also gives us tools for setting up
Angular-specific ways to test components, services, etc.. without having to run
our full application in a browser.

Karma is a test running that lets us run multiple tests and create reports in a
browser-readable format to make it easier to interpret the test results.

## Cleaning up default tests

Let's start by cleaning up the tests that the Angular CLI created automatically
for us when we created all our application components. We will then be adding
some of these tests back as we learn about unit testing.

Each component has a `<component-name>.component.spec.ts` test file associated
with it, so to get rid of all the tests that were created by default, remove all
the `spec.ts` files from your project:

1. `app.component.spec.ts`
2. `application-component.component.spec.ts`
3. `contact-list-component.component.spec.ts`
4. `conversation-control-component.component.spec.ts`
5. `conversation-history-component.component.spec.ts`
6. `conversation-thread-component.component.spec.ts`
7. `sender-message-component.component.spec.ts`
8. `send-message-component.component.spec.ts`
9. `header-component.component.spec.ts`

Now let's create a test file for a component we actually want to test. Let's
start with the header component.

## Simple test

The functionality Jasmine (the testing framework that comes with Angular)
provides is made available through specific functions. The main functions are:

1. `describe()`: this function denotes an entire test suite and has the
   following structure:
   1. The first argument is the description of the test suite in plain words
   2. The second argument is a function that will get called when the test suite
      is run
   3. Here is an example test suite:

```typescript
describe("'describe()' is a Jasmine function that lets us define a test suite", () =>  {
    // test suite code here
  });
});
```

2. `it()`: this function denotes a specific test case in a test suite, and has
   the following structure:
   1. The first argument is the description of the test case in plain words
   2. The second argument is a function that will get called when the test is
      run, which is every time the parent test suite is run
   3. Here is an example test case within our test suite defined above:

```typescript
describe("'describe()' is a Jasmine function that lets us define a test suite", () => {
  it("'it()' is a Jasmine function that lets us define a test case", () => {
    expect(true).toBe(true);
  });
});
```

Let's save this code in `header-component.component.spect.ts` in the
`header-component` folder of our header component.

Now you can run this test with the following command from the root directory of
your project:

`ng test`

This commands use Karma to run your Jasmine tests. We'll explain more about
Karma and why it's needed below. For now, just know that `ng test` runs your
Jasmine tests using Karma and Karma will by default open a browser window to
show you the results of running your tests, so the command above will result in
a window like this being shown to you:

![Karma simple success](https://curriculum-content.s3.amazonaws.com/java-mod-8/ng-messaging-karma-simple.png)

As you can see, our test suite was run, and within our test suite, our test was
run and successful.

Let's now create a new suite within the same suite and make it fail to see what
that output looks like. This is what the new test suite looks like once you've
added the new test:

```typescript
describe("'describe()' is a Jasmine function that lets us define a test suite", () => {
  it("'it()' is a Jasmine function that lets us define a test case", () => {
    expect(true).toBe(true);
  });

  it("should fail in order to demonstrate what a test failure looks like in Karma", () => {
    expect(false).toBe(true);
  });
});
```

And now the Karma output shows the failure:

![Karma simple failure](https://curriculum-content.s3.amazonaws.com/java-mod-8/ng-messaging-karma-simple-failure.png)

A couple of things to note:

1. The second test has a more standard description that starts with "should" and
   describe the expected result of that specific test
2. When you run `ng test`, it starts in "watch mode", which means that your test
   will run automatically as soon as they are updated. If you keep the browser
   window that it first opens opened, then that will also refresh automatically
   when you updated any of your test files.
3. You can now see that the Karma report shows you 1 passing test and 1 failing
   test and shows details of the failing test

## Karma vs Jasmine

We will not be getting into the details of Karma for this course, but we do want
to give an overview of why Angular uses it and how Karma is different from
Jasmine:

1. Jasmine is the testing framework and gives us the tools we need for actually
   writing our unit tests, including tools for specifying a test suite, tests
   within a suite and assertions within a test. It also provides us tools for
   integrating with the Angular framework (as we will see in the section below).
2. Karma is a tool for running our tests in specific browsers and emulate
   specific hardware. The tests we write in Jasmine should be the same across
   all platforms and devices, but the results may not the same in all the
   combinations of those environments.

As we saw in the previous section, Karma also has convenient reporting and watch
functionality that makes it easier to get a tight feedback loop between code
updates and test results.

## `beforeEach`

Before we start extending our test such that it can test our header component,
we need to get familiar with a new Jasmine function: `beforeEach()`.
`beforeEach()` takes a function as an argument and that code is run for every
test within the test suite (i.e. the `describe()`) where the `beforeEach()` is
defined. This allows us to have some common setup code for all the tests, rather
than having to duplicate that code in all tests.

Let's add a `beforeEach()` to our existing suite:

```typescript
describe("'describe()' is a Jasmine function that lets us define a test suite", () => {
  let testRunCount: number = 0;

  beforeEach(() => {
    testRunCount++;
    console.log("'beforeEach()' has run " + testRunCount + " time(s)");
  });

  it("'it()' is a Jasmine function that lets us define a test case", () => {
    expect(true).toBe(true);
  });

  it("should fail in order to demonstrate what a test failure looks like in Karma", () => {
    expect(false).toBe(true);
  });
});
```

Let's highlight a few things about this updated code:

1. The `beforeEach()` function is at the same level inside the `describe()`
   function as each one of the tests
2. We created a `testRunCount` variable that is global to the test suite
3. We increase `testRunCount` by 1 every time `beforeEach()` is run so that we
   can track the number of times it's called by Jasmine
4. In `beforeEach()` we `console.log()` the number of times it's been run every
   time it runs
5. You can view the output of `console.log()` in the console of the browser
   window Karma opens

You should now see an output like this from Karma and on the console:

![Karma console log](https://curriculum-content.s3.amazonaws.com/java-mod-8/ng-messaging-karma-console-log.png)
