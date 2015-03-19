- Start Date: 2015-03-17
- RFC PR: (leave this empty)
- Github Issue: (leave this empty)

# Summary

Babelscript (final name TBD) is a coffeescript-inspired language that compiles
to ESNext (specifically babel). It is designed to be a minimal layer on top of
babel that allows some features that are coffeescript-like whilst also leveraging
the built in features of newer versions of javascript.

# Motivation

ES6 / Babel are massive improvements over ES5, however there are still features
that can make coffeescript a lot more rewarding to code in. However, development
of the two main implementations of coffeescript are precluded from adopting some ES6 functionality, making it more and more difficult to write modern projects in coffeescript.

A similar language, that compiles to babel, would allow continuing to use a more natural syntax whilst taking advantage of more modern javascript features.

# Action Plan

These are the minimal steps I believe are required to launch a version that would be vaguely usable. If something is in this list, it's either because it's a core feature or because I believe that it has to be resolved before people start writing code in the language because otherwise things will definitely break in the future.

- [ ] Decide on name and file extension
- [ ] Fork a CS Compiler
- [ ] Module syntax
- [ ] Compile CS classes to ES6 classes
- [ ] Change splats to spread operator (or potentially permanently special case cs behaviour)
- [ ] Use spread operator for destructuring, only support what babel does in terms of ordering (or potentially permanently special case cs behaviour)
- [ ] Resolve lexical scoping and variable safety issues
- [ ] Compile to babel loops and comprehensions (if the behaviour is any different at all, needs checking)
- [ ] Resolve `for of` issue
- [ ] Remove anything that is definitely going to be removed, so that people don't rely on it in their code.
- [ ] Review reserved word list and update as necessary

# Detailed design

Below I've taken the headings from the coffeescript docs, and written how I think Babelscript should behave.

## Language

In general, the syntax of babelscript will be the same as coffeescript, except where exceptions are mentioned below. The benefits of this are twofold.

1. It means that existing tooling will work with babelscript (e.g. syntax highlighters etc), and not be much of a burden to learn.
2. It means that to start off we can use a fork of a coffeescript compiler, reducing the initial amount of work required to implement the language

## Modules

We should support ES6 module syntax, perhaps with a more liberal version (i.e. make curlies optional where it makes sense)

## Functions

Functions will look much the same as the do now in CS. Single arrow functions will compile to normal JS functions as they do now, whereas fat arrow functions will compile to babel fat arrow functions.

It would be good to (a) compile default arguments to babel default arguments and (b) replace splats with the spread operator. However splats are allowed at any argument position in CS but only as the final argument in babel.

One thing to bear in mind is that babel arrows do not require braces around the argument if there is only a single one:

```es6
var odds = evens.map(v => v + 1);
```

Wheras in CS this would compile to (roughly):

```js
var odds = evens.map(v(function() { return v + 1 }));
```

It's probably worth keeping the CS behaviour here and enforcing the parens around single function arguments, as it's more consistent and will be less work, along with being more consistent.

## Objects and Arrays

Objects and arrays would function similarly to the way they do now, with 2 deviations:

### Object literal shorthand

```coffee
foo = {a} # compiles to {a: a}
```

We should compile this now to `{a}` which babel supports

### Dynamic property names

Ideally we should support dynamic property names

```coffee
obj =
  foo: 1
  ["b" + "ar"]: 2
```

## Lexical Scoping and Variable Safety

We need to review variable hoisting in CS and let behaviour - preferable to adopt `let` instead of `var` and use ES6 scoping semantics

> If we changed default variable declarations to let then we're going to need extra syntax for var unless we specifically are trying to discourage it's use. (are we?)

## If, Else, Unless, and Conditional Assignment

No major changes here.

## Splats...

It would be nice to replace splats with the ES6 prefixed notation, and definitely compile to this.

## Loops and Comprehensions

We should compile to babel loops and comprehensions. `for of` needs resolving as this is (a) confusing anyway and (b) is a [completely different feature](https://babeljs.io/docs/learn-es6/#iterators-for-of) in babel

## Array Slicing and Splicing with Ranges

No changes for now

## Everything is an Expression (at least, as much as possible)

No changes for now

## Operators and Aliases

No changes for now, although it probably makes sense to deprecate some aliases at some point (i.e. maybe `and`, `or`, `not`, `is`, `isnt`, `yes`, `no`, `on`, `off`)

## The Existential Operator

No changes for now

## Classes, Inheritance, and Super

We should compile to ES6 Classes, which will result in slightly different class semantics.

## Destructuring Assignment

We can mostly compile to ES syntax which is mostly the same. The exception is the splat/spread operator, which is only allowed as the last position in ES6 but in any position in CS.

## Generator Functions

CS already produces ES6 generator functions, so we can pass this straight through.

## Embedded JavaScript

It might be worth changing the javascript embed syntax from backticks, as these are the ES6 template string syntax, but it doesn't really matter as CS already supports better template string syntax than ES6.

## Switch/When/Else

No changes for now

## Try/Catch/Finally

No changes for now

## Chained Comparisons

No changes for now

## String Interpolation, Block Strings, and Block Comments

It might make sense to change the interpolation syntax to `"${foo}"` instead of `"#{foo}` to mirror ES syntax. We could certainly output template strings instead of string concatenation.

## Block Regular Expressions

No changes for now

## Cake, and Cakefiles

Probably not necessary although would be required if we fork a CS compiler (as it's used for the build systems AFAIK)

## Source Maps

We need to produce source maps, and input them into babel so the mapping is maintained after babel transpilation. This [may not be possible right now](https://github.com/babel/broccoli-babel-transpiler/issues/3)

## text/coffeescript Script Tags

We probably don't want to support this this


# Features from ES6 that we want to add support for

* `const`
* Iterators
* `u` RegExp mode
* Modules
* Octal literals (binary literals already supported)
* Tail calls
    - This would be hard because it seems to require named functions

# Drawbacks

Why should we *not* do this?

* Could lead to language fragmentation
* Some people just plain hate coffeescript and will be angry at us

# Alternatives

What other designs have been considered? What is the impact of not doing this?

1. Wait for (one of) the coffeescript implementations to support more ESNext stuff
   The issue with this is that some stuff in ESNext is not compatible with CS.
   E.g. ES6 classes
2. Some horrible hacks to make coffeescript work with newer features, like
  [ember-cli-coffees6](https://github.com/alexspeller/ember-cli-coffees6)

# Unresolved questions

What parts of the design are still TBD?

Pretty much all of them at this point :)
