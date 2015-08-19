## Coding Guidelines

We aim to reduce incidental complexity as much as possible in the programs we build.
This allows us to focus on the complexities inherent in the problems we solve.
Incidental complexity can be imposed by our decisions as well as by the language itself.
This styleguide is intended to promote the reduction of imposing unneeded complexity by:
  - encouraging a functional programming paradigm,
  - codifying and unifying code style (i.e. whitespace, etc...), and
  - avoiding the 'bad' parts of the language (i.e. use `===` instead of relying on type coercion).

As a result of the choice to rely on data-based architecture (programming with immutable values) instead of
an architecture based on class hierarchies, we have chosen to use Mori.js in our architecture.
If you aren't using the data structures provided by Mori, you can swap those sample with JavaScripts' primitive
types. (i.e. arrays, objects).


### Style

#### Line-length and newlines
Try to keep lines at 80 characters or less, but up to 120 characters
is allowed. Not only is this scientifically more readable,
many of us write code on a vertical split-screen which makes longer line lengths
unreadable.

Use a combination of newlines and parenthesis to make it
easier to visually parse blocks, e.g:

```
_.hash_map(
  "type", "week",
  "week", week,
  "year", year,
  "practitionerId", practitionerId
);
```

Note that the closing parenthesis is on a new line to visually close the block.

#### Whitespace
- Never use tabs or trailing spaces.
- The indent size is two spaces.
- Use a single space between e.g. function arguments and key/value
  pairs in objects (e.g. `function (foo, bar) {}`, `{"foo": "bar"}`).
- Use a single space after the `if` keyword (i.e. `if (true) {}`).

#### Defining functions
Because we work in a modular JavaScript environment, we can safely use function declaration
without polluting the global namespace:

```
function doSomething() {}
```
When we are defining a function within a function, we use function expression:
```
function doOneThing () {
  var getValue = function () {};
}
```

#### JavaScript peculiarities
- Use `===` instead of `==` to test equality on singular values and `_.equals` from Mori to compare data structures.
- One `var` assignment per line except in the rare case of needing to initialize an empty variable
  ```
  var a = 1;
  var b = 2;
  var c, d;
  ```

#### Dates
All dates need to be stored internally as strings, formatted
as `YYYY-MM-DD HH:mm` if they include a time or `YYYY-MM-DD` if they do not. Normally, timezones are not relevant in the context of the Salonbooster, but when they are format according to [RFC3339][rfc3339]. These date strings are sortable using simple string comparison in Javascript (i.e. `end > start` will always be true if the end is after the start). For more complex date and time operations, as well as generating human-readable dates and times to display in the browser, we use [Moment.js][moment].


### Functional programming
> Functional programming... is also a way of thinking about how to build programs to minimize the complexities inherent in the creation of software. - Michael Fogus

We aim to build our software and it's smaller pieces of functionality in as functional a manner as possible.
Even when a particular problem truly calls for class- or object-oriented thinking, we look to apply functional concepts.

- Use `const` as often as possible instead of `let` or `var`. Be aware that `const` is block scoped. (see [ECMAScript 6 scoping][es6-scoping]).
This approach has two benefits:
    - forces avoiding mutation
    - allows `var` to provide a clear clue when mutation is being used
- Rely on small (idempotent)[#idempotent] functions that do a single things.
- Use the small functions to compose more complex things.
- Avoid use of state. Use stateless idempotent functions instead. This has a number of implications:
  - functions should be stateless
  - only use methods (functions defined on a object) when absolutely needed, like for the life cycle methods of React components, e.g. `shouldComponentUpdate` or `render`.
- Embrace classless programming.
  - focus on data instead of hierarchies of types. Data modeling is based on the primitive types provided by JS (or Mori.js).
We use [Mori][mori] to provide the data structures and convenient helping functions that are building blocks for pretty much anything.
  - Avoid using constructor functions, JavaScript class syntax, `super` and `this` whenever possible.

#### Immutability
Avoid mutating values, but return a new value instead. Consider this example:

```
var numbers = [1,2,3];
console.log(numbers.push(4).toString());
console.log(numbers.toString());
```

Versus this example using [Mori.js][mori]:

```
const numbers = _.vector(1, 2, 3);
console.log(_.conj(numbers, 4).toString());
console.log(numbers.toString());
```

The first example might trigger all sorts of unexpected behaviour
when accessing or reusing the `numbers` value elsewhere in the code.
Immutable values are easier to reason about precisely because they
don't change. In our example using Mori, we know that `numbers` is
always [1, 2, 3]. This allows us to do any number of activities based
on the starting value of `numbers`.

We have chosen tools such as Mori, React, an Fluxy which encourage immutability.
However they are not truly pure in this regard. Not to mention that JavaScript
itself was not designed with immutability in mind. While there are cases in which
mutable code leads to better performance due to the design of the Javascript engine,
in practice this is mostly a non-issue because it is rarely a bottleneck.

Regardless, by following a few simple guidelines, we can take the concept of mutability
quite far in our application:
- avoid using mutable data structures and use Mori datastructures
  instead,
- with variables, always assign a new value instead of updating an existing one,
- with functions, always return a new value instead of mutating an argument or
a variable that is defined outside of the function scope.

#### Purity and Idempotency
Idempotency means that a function always returns the same result when given the same
arguments, regardless of when and how many times it is executed. Idemopotence is closely
related to the notion of purity.

A pure function must calculate its result from the arguments it is provided. Nothing
outside of the function scope can have an effect on the result of the computation
inside of it. Additionally, the function may not modify the state of something
external to itself.

Impure functions generally can't be idempotic. For example,
a function that performs I/O or makes a network call
can never be idempotent, because it depends on the state of an external system.
Likewise, a function cannot be regarded as idempotic if it relies on a mutable
variable defined outside of the function scope because the function will return a
different result when that variable is mutated by other code.

Object-oriented architectures tend to rely on these type ofmutations. For exampke,
they may rely on a particular external state to influence the result of a
method call. Unfortunately, this tends to add incedental complexity.

There are a number of benefits introduced by purity and idempotency. They
make code a lot easier to reason about. Diving into a new codebase
becomes easier when the only requirement for understanding a function is to
actually read the code inside of it, without being conerned with externalities.
Additionally, testability becomes far more trivial when the only consideration
is the input and the output of a specific function.

For the sake of simplicity, we generally use the word idempotic
when referring to purity and idempotency in the rest of this document.

#### Functional composition
A goal in the architecture is to define small, reusable functions that can
be combined and sculpted. In JavaScript there are a number of books and libraries
which demonstrate this approach. (i.e. Functional JavaScript, JavaScript allonge, and Mori.js)

We use the simple functions provided by Mori, along with our
own functions as building blocks.

E.g:

```
// taken from https://github.com/swannodette/mori:
const second = mori.comp(mori.first, mori.rest);
second(mori.vector(1,2,3));
```

The following example shows a number of functional elements. Rather than modifying
a value through a chain of actions performed on it, it flows a value through a series
of operations each of which returns a new value which is passed into the following
operation. This leaves the starting value unchanged, but returns a new value based
on the result of the operations.  In other words, it returns a new immutable data
structure via threading another immutable data structure through a set of idempotic
functions. This utilizes:
  - data flow (pipeline)
  - immutable data structures (hash maps), and
  - currying.

```
const updatedData = _.pipeline(
  _.hash_map(
    "utilized", _.hash_map("minutes", 0),
    "available", _.hash_map("minutes, 100)
  ),
  _.curry(_.update_in, ["utilized", "minutes"], _.curry(_.sum, 15)),
  _.curry(_.update_in, ["available", "minutes"], _.curry(_.min, 20))
);
```
#### Partial application and currying
A curried function returns a new function for each argument that it takes.
A partially applied function returns a function that accepts and is waiting for the remainder of its arguments.

E.g.
```
  function add(a, b, c) {
    return a + b + c;
  }

  // Partial application.
  const addOne = partial(add, 1);
  addOne(2, 3); // 1 + 2 + 3 = 6
  addOne(2);    // 1 + 2 + undefined = NaN

  // Currying.
  const addOne = curry(add)(1);
  addOne(2)(3); // 1 + 2 + 3 = 6
  addOne(2);    // returns the equivalent of curry(add)(1)(2)
```

This can be used for great power in functional programming.


#### Recursion
A recursive function is a function that calls itself. It solves
a problem by performing an operation on a subset of the problem and using those solutions to solve the problem.
Writing a recursive function requires that you know:
  - what problem you need to solve,
  - the edge cases (aka when to stop),
  - what the smallest unit of the problem is (aka the general case)

#### Map and Reduce and filter

Map, reduce and filter are 3 functions that take a function and a collection and call the function once for each element of the collection.
  - Map returns the same number of values as it was given. Map takes a collection and runs each element of the collection through a function and returns a collection of the results..
  - Reduce returns one value from all the values it is given. Reduce accumulates a collection into a single value by calling the passed function each time with an accumulator and the next item from the collection.
  - Filter returns a subset of the collection of values it was given. Filter calls a function that returns true or false and collects each value that returned true.

# TODO

#### Laziness



[mori]: http://swannodette.github.io/mori/
[moment]: http://momentjs.com/
[rfc3339]: http://tools.ietf.org/html/rfc3339#page-10
[fluxy]: https://github.com/jmreidy/fluxy
[react]: http://facebook.github.io/react/
[es6-scoping]: http://www.2ality.com/2015/02/es6-scoping.html