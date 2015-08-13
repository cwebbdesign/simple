## Coding Guidelines

### Style

#### Line-length and newlines
Try to keep lines at 80 characters or less, but up to 120 characters
is allowed. Use a combination of newlines and parenthesis to make it
easier to visually parse blocks, e.g:

```
_.hash_map(
  "type", "week",
  "week", week,
  "year", year,
  "practitionerId", practitionerId
);
```

Note that the closing parenthesis is on a new line to visually close
the block.

#### Whitespace
- Never use tabs or trailing spaces.
- The indent size is two spaces.
- Use a single space between e.g. function arguments and key/value
  pairs in objects (e.g. `function (foo, bar) {}`, `{"foo": "bar"}`).
- Use a single space after the `if` keyword (i.e. `if (true) {}`).

#### Defining functions
Because we work in a modular JavaScript environment, we can safely use function declaration:
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
- Use `===` instead of `==` to test equality on singular values and
  `_.equals` from Mori to compare data structures.
- Use single blocks for `var` assignments (i.e. `var foo = 1, bar = 2;`,
  but with a newline between assignments, instead of `var foo = 1; var bar = 2;`).

#### Functional programming
- Use `const` as often as possible instead of `let` or `var` to force yourself to avoid mutation. Be aware that `const` is block scoped. (see [ECMAScript 6 scoping][es6-scoping]).
- Write small, idempotent functions that do a single thing and combine them to do more complicated things.
- Avoid using constructor functions, JavaScript class syntax, `super` and `this` whenever possible. (Sometimes delegating to methods is better for managing memory.) 
- Only use methods (functions defined on a object) when absolutely needed, like for the life cycle methods of React components, e.g. `shouldComponentUpdate` or `render`. Instead we aim to use stateless idempotent functions.

#### Dates
All dates need to be stored internally as strings, formatted
as `YYYY-MM-DD HH:mm` if they include a time or `YYYY-MM-DD` if they do not. Normally, timezones are not relevant in the context of the Salonbooster, but when they are format according to [RFC3339][rfc3339]. These date strings are sortable using
simple string comparison in Javascript (i.e. `end > start` will always
be true if the end
is after the start). For more complex date and time operations, as
well as generating human-readable dates and times to display in the
browser, we use [Moment.js][moment].

### Functional programming
We use [Mori][mori] to provide the data structures and convenient
helping functions that are building blocks for pretty much anything.

#### Immutability
Avoid mutating values, but return a new value instead. Consider this
example:

```
const numbers = [1,2,3];
console.log(numbers.push(4).toString());
console.log(numbers.toString());
```

Versus this example using [Mori.js][mori]:

```
const numbers = _.vector(1, 2, 3);
console.log(_.conj(numbers, 4).toString());
console.log(numbers.toString());
```

The first example might trigger all sorts of unexpected behaviour when
reusing the `numbers` value in other, potentially unrelated. The
immutable example produces the same result, but without mutating the
underlying value. Immutable values are much easier to reason about
too, because you don't have to take changes to a value into account.

Mori.js provides some immutable data structures, but the Javascript
language itself wasn't designed with immutability in mind. The
[Fluxy][fluxy] framework and React encourage immutability, but are by
no means pure in this regard. In some cases mutable code leads to
better performance due to the design of the Javascript engine, but in
practice this is mostly a non-issue because it is rarely a bottleneck.

By following a few simple guidelines, however, it is possible to take
the concept of immutability quite far in our application:

- avoid using mutable data structures and use Mori datastructures
  instead,
- with variables, always asign a new value instead of updating an
  existing one,
- with functions, always return a new value instead of mutating an
  argument or a variable that is defined outside of the function scope.

#### Idempotency

Idempotency means that a function always returns the same result when
given the same arguments, regardless of when and how many times it is
executed. This requires that nothing outside of the function scope has
an effect on the result of the computation inside of it. That means
that a function that performs I/O or makes a network call can never be
idempotent, because it depends on the state of an external system.
Another example would be using a mutable variable that is defined
outside of the function scope, because the function will return a
different result when that variable is mutated by other code. This
pattern is very common in object-oriented architectures, where an
attribute of an object influences the result of a method call.

Idempotency makes code a lot easier to reason about. It is also faster
to wrap your head around a new codebase if you can assume that the
only thing you have to do to understand a function is to actually read
the code inside of it, without having to worry about externalities.
Another obvious benefit is testability. Sadly, the Salon Booster
codebase is far from pure in this regard, mostly because we followed
the imperative, object-oriented style of [React][react] and
[Fluxy][fluxy] pragmatically. That being said, idempotency, as well as
immutability, remains an important design goal.

#### Functional composition and partial application

A goal in the architecture is to define small, reusable functions that
can be combined and sculpted. The simple functions provided by Mori,
with our own functions alongside and on top of those, are building
blocks.

E.g:

```
// taken from https://github.com/swannodette/mori:
var second = mori.comp(mori.first, mori.rest);
second(mori.vector(1,2,3));
```

```
_.pipeline(
  _.hash_map(
    "utilized", _.hash_map("minutes", 0),
    "available", _.hash_map("minutes, 100)
  ),
  _.curry(_.update_in, ["utilized", "minutes"], _.curry(_.sum, 15)),
  _.curry(_.update_in, ["available", "minutes"], _.curry(_.min, 20))
);
```

#### Map and Reduce

#### Laziness



[mori]: http://swannodette.github.io/mori/
[moment]: http://momentjs.com/
[rfc3339]: http://tools.ietf.org/html/rfc3339#page-10
[fluxy]: https://github.com/jmreidy/fluxy
[react]: http://facebook.github.io/react/
[es6-scoping]: http://www.2ality.com/2015/02/es6-scoping.html