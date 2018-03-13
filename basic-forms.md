# Core RuleScript expressions

RuleScript expects you to structure your specs around a small number of basic expressions, described below.

## `validate-document`

All RuleScript files start with the validate-document command, which specifies the name to be used for the input file and the rules to be checked. The signature is:

```clojure
(validate-document
  (input-name)
  ...rules
)
```

Note that input-name is enclosed within parentheses. The name `input-name` is just an example; you can (and should!) use a name that illustrates the thing you are validating. Just to be clear, what you are doing in the input-name expression is giving a name to the input document being combined with this spec, so that you can refer to it in subsequent parts of the spec.

## `rule`

Rules are the heart of a RuleScript spec. Each one is a specific yes/no question you are asking about the input data.

Rule are defined in the following way:

```clojure
(rule
  rule-name
  (single-rule-expression ...))
```

The `rule` expression must be followed by the rule’s name. Make this name descriptive; it’s how you’re going to find the rule later. In accordance with RuleScript syntax rules, the rule’s name must only be one symbol (i.e. *NOT* a between quotation marks). If you want a multiword rule, separate the "words" with a dash character. So, for a carnival ride, "Applicant Is Tall Enough" would become `applicant-is-tall-enough`. Later, your results will be printed with proper spaces between the words and each word capitalized in the rule name.

Note: `single-rule-expression` means that each rule must have exactly one (1) expression following the rule name, which ultimately evaluates to `true` or `false`. Of course, your rule expression is likely to contain sub-expressions; this is fine, as long as there is only one top-level expression. For example, the following is fine:

```clojure
(rule
  not-an-error
  (and
    (check-first-condition ...)
    (check-second-condition ...)))
```

... but a rule expression containing more than one expression, like the following, is not:

```clojure
(rule
  compiler-is-unhappy
  (do-first-thing ...)
  (do-second-thing ...))
```

## `in`

`in` allows you to link input data to the logic in your spec. Except for `rule`, which is required to structure specs, `in` will be your most frequently-used RuleScript expression. `in` searches for data within a source, using a special verb to tell the computer how to look for the terms you provide. It follows this template:

```
(in <subject> <verb> <object(s)>)
```

- `subject` is the thing you want to look inside. This is usually just the `input` to your spec, but it can also be the result of an `in` expression (more on this later).
- `verb` is one of `find`, `find-each`, or `extract`. They are explained below.
- `objects` are the JSON fields/keys/properties to search for within `subject`. You must provide at least one `object` to `in`.

If `in` can't find the data you specify, it will return `nil`.

This is a long section with lots of intimidating notation. If that's too much for you right now, just stick with basic useage of `in`. It will cover 90% of your use-cases.

Let's look at the different forms of `in` using a simple example. Consider the following JSON object representing a home for sale:

```json
{
    "realtor": "Dinn Blynn",
    "address": "403 Cook St",
    "city": "Victoria",
    "still-available?": true,
    "features": {
        "second-suite": true,
        "above-ground-pool": false,
    }
}
```

### Verb: `find`

How do we access specific parts of this object in RuleScript? Simple: use `in`. To retrieve the `city` field from the home object (defined in our spec as `home`), we would say:

```clojure
(in home find city) ;; evaluates to "Victoria"
```

`find` looks for the named field/key/property in its `subject`. It also supports multiple fields, "walking" downward through the data structure:

```clojure
(in home find features above-ground-pool) ;; evaluates to false :(
```

*Clojure pros*: the expression `(in data find foo bar)` is equivalent to `(get-in data [:foo :bar])`.

### Verb: `find-each`

While calling `find` with multiple `objects` will drill down on your data, `find-each` treats multiple `objects` as things that *should all be returned*. You will get a sequence: useful if you want to check that they all exist, call `(in _ extract _ _)` on them, use them in a `map` call, etc.

Using our `home` example:

```clojure
(in home find-each realtor address) ;; returns '("Dinn Blynn", "403 Cook St")
``` 

`object`s in `find-each` can themselves be sequences, allowing you to be more specific about what should be in your sequence:

```clojure
(in home find-each (features second-suite) still-available) ;; returns `(true, true)
```

*Clojure pros*: the expression `(in data find-each (foo bar) baz)` is equivalent to `(map #(get-in data %) '([:foo :bar] [:baz]))`.

### Verb: `extract`

`extract` is the mirror of `find-each`: Instead of making a sequence by searching for multiple `object`s in the same `subject`, you make a sequence by searching for the same `object` to multiple `subject`s. This means the `subject` part of `in` must be a sequence.

It's actually pretty simple. Let's call the following data `academic-record' (note that it's a JSON array, which is a sequence of things):

```json
[
    { 
        "course-name": "Math 201",
        "professor": "Alonzo Church",
        "grades": {
            "final": 92,
            "midterm": 87
        }
    }, 
    {
        "course-name": "Public Speaking",
        "professor": "Simon Peyton Jones",
        "grades": {
            "final": 80,
            "midterm": 79 
        }
    },
    {
        "course-name": "Software Design",
        "professor": "Richard Hickey",
        "grades": {
            "final": 96,
            "midterm": 78,
            "hammock lab": 99
        }
    },
]
```

... And to get our final grades, we would call:

```clojure
(in academic-record extract grades final) ;; evaluates to '(92, 80, 96)
```

"But wait," you say. "The data in my `input` is a JSON object, not a JSON array!" 

Correct (although an array would also be a valid `input`). But consider the possibility of using `extract` alongside sequences *within* your input returned by `find`, or created with `find-each`. Suddenly `extract` is a nice, compact notation for talking about that data.

*Clojure pros*: the expression `(in data extract foo bar)` is equivalent to `(map #(get-in % [:foo :bar]) data)`.

### Nesting `in` expressions

`in` merely expects its `subject` to be something that can be searched. This can be your `input` data, or *a JSON object returned from another `in` call*. So, returning to our `house` example, the following is totally valid:

```clojure
(in (in house find features)
    find second-suite)          ;; evaluates to true
```

"Don't do this" goes without saying. Only nest `in` expressions when it will actually increase clarity. For example, instead of:

```clojure
(in home find-each (features second-suite)
                   (features above-ground-pool))    ;; evaluates to '(true, false)
```

Try:

```clojure
(in (in home find features)
    find-each 
        second-suite 
        above-ground-pool) ;; evaluates to '(true, false)
```

### finally...

A hint for RuleScript beginners: don't get fancy with `in`. You can do lots of tricks to nest `in` expressions together in different forms, but increased complexity means increased difficulty for human readers. Use good taste: break your logic into different rules if you notice your `in`s getting out of hand.

