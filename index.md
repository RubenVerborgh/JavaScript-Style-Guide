# Developing sustainable projects in JavaScript – a style guide

An opinionated list by [Ruben Verborgh](https://ruben.verborgh.org/)


## 1. Code structure

### 1.1. Use object orientation by default
JavaScript is a multi-paradigm programming language.
Without making any judgement about any paradigm, we choose object orientation as the default approach.

Within this paradigm, we make the following decisions.

- Organize your code as classes.
  - Specifically, use ES6 classes.
  - Since your class represent an instantiatable thing, name it with a noun.
    E.g., `Command` and `CommandExecutor`, _not_ `executeCommand`.
  - Typically there is one main class per file, which is the default export.
  - Give your file the name of the class, plus the applicable file extension.
  - Exceptions for specialized parts of code exist, where for instance functional programming is more appropriate.

- A class has a single responsibility.
  - Usually, it should be possible to describe in one sentence what a class does.

- A class encapsulates state.
  - The class is responsible for keeping (only) its own state consistent.
  - Consumers should not know and not depend on how an object is implemented;
    much less how its dependencies are implemented.
  - If a method is consistently invoked with the same parameter,
    the consumer might have too much knowledge about the class's state.
    Consider making it a constructor parameter instead.

- Classes have limited knowledge about how other classes work.
    - Follow the [Law of Demeter](http://misko.hevery.com/2008/07/18/breaking-the-law-of-demeter-is-like-looking-for-a-needle-in-the-haystack/),
      and expect specific dependencies to be passed in
      rather than traversing object trees to find them.

- Every class has a corresponding test file with unit tests.
  - Write classes in a unit-testable way (which depends on architecture).

- Write any exported executables as minimal wrappers around a class.
  - Any command-line script instantiates a class with the right arguments.
  - That class can be independently unit-tested (the script much less so).


### 1.2. Design for substitutability
- There exist three kinds of objects:
  objects that _are_ things,
  objects that _do_ things,
  and objects that _make_ things.
  - The first group are data structures
    that represent a logical unit of information.
    They typically have few dependencies (mostly other data structures).
  - The second group are classes that process data
    and/or interact with the environment.
    They can depend on other objects for behavior,
    but they should usually not instantiate those.
  - The third group are factories,
    whose sole job is to instantiate other objects.
  - A regular object should not know how to instantiate its dependencies;
    rather, it takes its dependencies as constructor arguments.
    This allows for unit testing and changing behavior.
    Only factories know how to construct object trees.

- Prefer composition over inheritance for reuse of functionality.
  - Inheritance is useful for polymorphism and substitutability;
    so do inherit for interface reasons.
  - However, rather than relying too much on inherited functionality,
    extract that functionality into reusable classes.
  - This simplifies testing and changing behavior,
    and avoids the need to retest inherited behavior.


### 1.3. Organize code conceptually in packages
- Use a folder hierarchy to represent packages of related code.

- Dependencies are usually tree-shaped.
  - Other graph shapes might indicate an architectural problem.

- One index file with non-default ES6 exports exposes the public API.



## 2. Coding style

### 2.1. Consistency
- Above all, strive for consistency.
  - A linter can help you achieve that.


### 2.2. Defensive programming
- Validate your assumptions (at the moment you make them).
  - For typing, this is possible through TypeScript.
  - Methods throw an error when called with invalid arguments.


### 2.3. Asynchronicity
- Prefer `async`/`await` over explicit `Promise`s, and prefer those over callbacks.
  - `Promise` wrappers for native Node functions exist.

- Use callbacks if needed for performance reasons.
  - Especially for streams.



## 3. Testing
- Ensure all code is tested with unit tests.
  - Classes have corresponding unit tests.
  - Replace dependencies by mocks with validated assumptions.
  - High coverage is important, but not a goal in itself.
  - Not only ensure that code is used, but that its effects are tested.

- Write integration tests for groups of classes.
  - Since one class has one specific responsibility,
    integration tests check how those responsibilities interact.
