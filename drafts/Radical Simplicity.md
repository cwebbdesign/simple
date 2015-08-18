# Radical Simplicity

We are motivated by making things that people love to use. We want to
ship, ideally as early as possible, so we can get user feedback. This
requires being pragmatic, instead having architecture and tools for
their own sake. We focus on radical simplicity, because it follows
naturally from being pragmatic about everything else, too. It makes
things easy to reason about, which speeds up rolling out new features
and leads to more maintainable code. So what is simple? Simple isn't
necessarily easy. Easy is subjective and about familiarity. Being
[simple][simple] is about a lack of complexity, which is an objective
quality. It can be hard and counterintuitive to make things radically
simple.

## Imperative programming

Traditional programs are written like instructions to the CPU or
runtime. Additionally, they use mutable state which is tied to to the
memory or filesystem of the computer. This imperative mindset
introduces a lot of [accidental complexicity][no-silver-bullet], which
is complexity we create ourselves as programmers, as opposed to the
essential complexity of hard problems. It is a good fit with how
computers work at a low level. It does not, however, make things
simple. We prefer a more declarative style. Formulating what a program
needs to do, instead of how it should do it, is a much closer better
fit with the business language of the problem domain.

## The problem with mutable state

Mutable state introduces side effects. Whether it is a pointer to a
place in memory, a file on the harddisk, a database record or a node
in the DOM of a web browser, overwriting values and having different
behaviour depending on their current state becomes hard to reason
about when programs grow beyond a few hundred lines of code. It is
already hard in the context of a single threaded application, but in a
multi-treaded environment or a network architecture there is also the
additional problem of keeping things consistent when different
components might overwrite values independently. Mutable state forces
you to reason about anything that touches it, which is potentially
anything, including things that are external to the program.

## The value of values

Immutable data structures are generic and don't encapsulate
[values][values] with a custom interface like objects do. They expose
values. This makes it possible to write simple, abstract functions
that return new values instead of mutating existing ones. You can also
reuse the same abstractions over and over again, which enables
programmers to be more productive. The opposite of value oriented
programming is place oriented programming, where you have a pointer to
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
mutable state without actually calling it functional programming. So
what is it, really? It is the concept that functions should be
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