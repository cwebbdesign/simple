# Radical Simplicity

Pragmatism: A practical, matter-of-fact way of approaching or assessing situations or of solving problems. - (Wordnik)[pragmatism]

We are motivated by making things that people love to use. We want to
ship, ideally as early as possible, so we can get user feedback. This
requires being pragmatic instead of having architecture and tools for
their own sake. A focus on radical simplicity flows naturally out of our
pragmatism. Radical simplicity makes our programs easy to reason about,
which speeds up rolling out new features and leads to more maintainable code.

## Defining Simple

Simple is a potentially misleading term, so we begin by defining what [simple][simple] is.

To define simple we start with the opposite word: complex. The Terminology dictionary
defines complex as: "complicated in structure; consisting of interconnected parts".
Simple is about the lack of complexity. However, it is not simplistic nor is it another
word for easy.

Simplistic would imply naievety in our solutions. Easy on the other hand indicates that
something is not hard.  Ironically it can be quite hard to make things simple without
being simplistic.

Simplicity is about reducing incidental complexity as much as possible in order to be able
to focus on the complexity that is inherent to the problems we solve. The notion of "accidental
complexity" as opposed to "essential complexity" is discussed in Fred Brooks' famous 1986 [paper][no-silver-bullet]: No Silver Bullet. Accidental complexity is
the complexity that we as programmers introduce while essential complexity is caused by the problem we are solving.

The degree of easey is a subjective measurement based on familiarity and skillset. Simplicity is
an objective quality: the lack of complexity. This is a critical difference.
We attempt to make our decisions &mdash; whether about the tools we use, the code we write,
or the validity of the code we are reviewing &mdash; based on the objective
measurement instead of the subjective measurement. In other words, simplicity becomes our measuring stick giving us way to make decisions pragmatically. So simplicity flows out of pragmatism and pragmatic solutions are furthed by simplicity.


## Imperative vs Declarative programming

Traditional programs are written like instructions to the CPU or
runtime; each instruction changes the state of the program.
They use mutable state which is tied to to the
memory or filesystem of the computer. The imperative mindset focuses
on describing how the program should operate. Unfortunately, this approach
introduces a lot of accidental complexity. It is a good fit with how
computers work at a low level. It does not, however, make things
simple. Alternatively, we prefer a more declarative style.
Formulating what a program needs to do, instead of how it should do it,
is a much closer fit with the business language of the problem domain.

## The problem with mutable state

Mutable state introduces side effects. Whether it is a pointer to a
place in memory, a file on the harddisk, a database record or a node
in the DOM of a web browser, overwriting values and having different
behaviour depending on their current state becomes hard to reason
about when programs grow beyond a few hundred lines of code. It is
already hard in the context of a single threaded application, but in a
multi-threaded environment or a network architecture there is the added
problem of keeping things consistent when different components might
overwrite values independently. Mutable state forces
you to reason about anything that touches it, which is potentially
anything, including things that are external to the program.

## The value of values

Immutable data structures are generic and don't encapsulate
[values][values] with a custom interface like objects do.
They expose values. This makes it possible to write simple, abstract functions
that return new values instead of mutating existing ones. You can also
reuse the same abstractions over and over again, which enables
programmers to be more productive. The opposite of value oriented
programming is place (object) oriented programming, where you have a pointer to
a value that keeps mutating. This is hard to test and even harder to
reason about in the context of a complex system. Working with values
is an extremely powerful way to avoid this complexity. It also makes
testing a breeze, because you just have to test an input and an output
value.

## Functional programming

Functional programming doesn't necessarily mean that you're not
allowed to use objects. It also doesn't conflict with having a
powerful type system, if you look at languages like Haskell, Scala,
and ([typed][typed-clojure]) Clojure that all have powerful,
but very different, type systems in a functional environment. It is
also not a new phenomenon, because the principles have been in play
since at least the invention of double-entry accounting and more
recently lambda calculus. In fact, experienced programmers in the
imperative world tend to gravitate towards principles like avoiding
mutable state without actually calling it functional programming.

So what is it, really? It is the concept that functions should be
idempotent, which means that they always provide the same results when
given the same arguments. Idempotent functions don't have side effects
that introduce accidental complexity. Functions become building blocks
for algorithms and can be used in a declarative, rather than
imperative style. With concepts like higher order functions, recursion
and currying you get things done simply and with less code.

## References

- [No silver bullet][no-silver-bullet] (accidental complexity vs essential complexity)
- [Simple made easy][simple]
- [The value of values][values]
- [Functional programming (Wikipedia)][fp-wikipedia]
- [Imperative programming (Wikipedia)][imperative-wikipedia]
- [Declarative programming (Wikipedia)][declarative-wikipedia]

[no-silver-bullet]: https://en.wikipedia.org/wiki/No_Silver_Bullet
[simple]: http://www.infoq.com/presentations/Simple-Made-Easy
[values]: http://www.infoq.com/presentations/Value-Values
[typed-clojure]: http://typedclojure.org/
[fp-wikipedia]: https://en.wikipedia.org/wiki/Functional_programming
[imperative-wikipedia]: https://en.wikipedia.org/wiki/Imperative_programming
[declarative-wikipedia]: https://en.wikipedia.org/wiki/Declarative_programming
[pragmatism]: https://www.wordnik.com/words/pragmatism