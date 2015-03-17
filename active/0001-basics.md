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

# Detailed design

This section needs a lot of work, however here are some key issues:

* Should support ES6 module syntax
* Indentation-based (maybe optional curlies a la Stylus)
* Statements are always expressions when possible (e.g. if/else, functions return their last value etc.)
* Support for `@`
* Existential operator (`?`)
* I would really like to support some way of making using computed properties in
  ember more easily. Not an ember-specific feature, but some language featuer that
  would enable that. Examples: annotations, macros, decorators. This could be useful for lots of other use cases too. I don't believe annotations or decorators are part of ES6 at the moment, however I think they have been discussed.

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
