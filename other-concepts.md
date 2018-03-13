# Other RuleScript concepts

## `true` and `false`

`true` and `false` are sometimes called "Boolean values". They are simple to grasp for quantitative questions ("is X greater than Y?", for example), but there are some points worth discussing.

First, RuleScript’s `true` and `false` are the same as JSON’s `true` and `false`. So, you can use a rule to simply ask whether a JSON field is `true`. For example, in the input:

```json
{
   "is-valid": true
}
```

The following rule is ok:

```clojure
(rule
  is-valid?
  (in input find is-valid))
```

Second, RuleScript contains a number of synonyms for `true` and `false` to make reading rules more natural. *These synonyms are an experimental language feature and may be removed in future versions*.

RuleScript is designed so that rules must evaluate to a `true` or `false` value. Due to the way Clojure represents truthiness, the values `nil` (equivalent to JSON’s `null`) and `false` are equivalent to `false` in RuleScript, and *any other value* is equivalent to `true`. This is a major gotcha for new RuleScript writers.

Taking the above two paragraphs into account, here are all of the synonyms for `true` and `false`:

| Same as `true` | 	Same as `false` |
| ---- | ---- |
| `yes` | `no`|
| `ok` | `not-ok` |
| any other value 	| `nil`|


## Rule results

Applying a rule will cause one of four results to be recorded:

- `pass`
- `fail`
- `error`
- `warning`

`pass` and `fail` correspond to `true` and `false` respectively. They say, in effect, that the rule’s criteria was or was not met.

An `error` result indicates that something went wrong with the compiler when trying to apply your rule. You should check the output for a hint about how to fix the problem.


## `warning` and `warn-when`

A `warning` result is given when you have specifically indicated that a `pass` or `fail` should instead be reported as a `warning`. This is a convenience for special cases where a human reviewing your results needs to be aware of something meaningful to your policy, but the outcome of the rule is not critical to your overall verification process.

You can cause a warning to be emitted by using the `warn-when` expression. It looks like this:

```clojure
(warn-when
  sentinel
  (rule
    rule-name
    (single-rule-expression)))
```

`sentinel` can be `true`, `false`, or any of their synonyms. Note that to make a `warn-when`, you simply "wrap" it around a standard rule definition. Nothing about the rule expression changes when using `warn-when`; the rule is just the second argument to `warn-when`.


## complete? and complete-seq?

Two very useful expressions for validating data. `complete?` takes a JSON object and checks whether all of its fields/keys/properties have an associated value. For example, the object `{ "name": "" }` is not complete; the `name` value is empty. Similarly, `{ "age": null }` is empty; the `age` value is null. `complete?` works on very large objects--use it everywhere! `complete-seq?` works like `complete?`, but on sequences of things.


