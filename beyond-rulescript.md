# Beyond this documentation

"Real" RuleScript specifications, even the ones in the [online tutorial](https://rulescript.org/try), use lots of epressions not explained in this documentation.

These expressions are from the [Clojure programming language](https://clojure.org), which RuleScript sits on top of. Almost any Clojure expression can be used from within RuleScript (see below for a list of disallowed symbols). This allows RuleScript writers to be incredibly expressive, although there are a lot of new expressions to learn. Some especially usefuly ones are below.

The [official Clojure cheatsheet](https://clojure.org/api/cheatsheet) is a great tool for learning new expressions and refreshing your memory about old ones.

## Getting started

- `+`, `-`, `*` (multiplication), and `/` (division) work like you'd expect.
- `min` and `max` take the minimum and maximum, respectively, of the provided arguments.
- `and` takes any number of values or expressions, and returns either the first `false`/`nil` value (called, amusingly, "falsy"), or else the final value. Answers the question, "Are all of these things true?"
- `or` is similar; given values, return the first non-falsy (called "truthy") value, or else the final value. Anwsers the question, "Are ANY of these things true?"
- `if` takes the form `(if test then else)`, where `test` is a boolean expression/value, `then` is the thing that will be returned if `test` is true, and `else` is the thing that will be returned if `test` is false.
- `map` takes an expression and a sequence, and applies that expression to each element of the sequence. It returns a sequence of those results. For example, consider `(map complete? some-elements)`.
- `filter`, like `map`, takes an expression and a sequence, and applies the expression to each element of the sequence. However: the expression *must* return a boolean value, and `filter` returns the *original values for which the expression is `true`*.

## Others?

There are hundreds! In addition to the Clojure cheatsheet, you may wish to check out [Clojure's full API documentation](https://clojure.org/api/api). Pro tip: function names that end with `?` are boolean expressions.
