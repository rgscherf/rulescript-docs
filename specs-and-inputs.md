# Specs and Inputs

RuleScript’s software needs two pieces of information to produce a result:

- an *input document*, in the form of a JSON object. JSON is an easy-to-understand data format that is the lingua franca of information transmission. If you are validating information from a Web-based source, your data is probably already in JSON!
- a *policy specification* (hereafter “spec”), in the form of a RuleScript file. In the desktop version of RuleScript, you can pass this spec either as a string or an EDN file.

Your spec checks specific information in the input document to verify that each rule in the spec is satisfied in a pass/fail manner. Importantly, your spec must be aware of the specific location of the data in your input document. This means that RuleScript is only appropriate when you know the exact shape of the data being applied to your spec.

## The Spec file

RuleScript is a superset of Clojure, so the standard Lisp syntax rules apply. These are succinctly explained in the [interactive tutorial](https://rulescript.org/try).

RuleScript differs from full-fledged Clojure in that the compiler expects your entire spec to exist as a single expression starting with `(validate-document ...)`. Of course, comments (beginning with `;;`) can exist outside this single expression.