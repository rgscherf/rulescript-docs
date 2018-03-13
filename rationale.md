# Rationale: Why RuleScript?

It may be difficult to wrap your head around why RuleScript is useful until you’ve seen it in action. That said, here are some possible benefits to get you interested:

- Obviously, the most important reason to automate document validations is that *it will save you time*. In most offices, saving at least a couple person-hours per week is easily within reach; that adds up to several days of staff time per year. As a manager, imagine having a "bonus" week of staff time to throw at your most pressing issues. As staff, imagine liberating a week per year for more interesting tasks. RuleScript’s benefits scale linearly with the volume of tasks you can automate; certain departments could save serious time with this tool.

- RuleScript’s focus on being easy to read has some interesting effects. First, it means that policies can be written in RuleScript alone, *so that your policies are the computer code which enforces the policies*. Second, it means that computer code is no longer a black box for managers; even if your manager can't write RuleScript, he/she can understand what’s going on and offer feedback. Third, it means that RuleScript written by others is easy to understand, increasing the chance that you can share code and build off each others' efforts.

- RuleScript looks like a programming language, but it’s really just a high-level set of commands given to a programming language. *RuleScript shields you from the immense complexity and frustration of actually writing computer code*. You just focus on the logic of your policies.

- Being fast to write, RuleScript is great for prototyping. Policy development takes on a whole new meaning when you can create two different specifications of a policy and instantly calculate their differences against a single set of test data. This kind of policy development is ripe for experimentation.

- RuleScript delivers control into the hands of analysts, rather than the IT department. Consider that many public policies make use of computer programs that collect information, validate whether the information meets some criteria, and then gives the results to analysts. What happens when your validation rules change? Or, what happens if those rules were miscommunicated in the first place? Usually, the answer is "fixing it is expensive". RuleScript exists to avoid these problems by putting the validation step entirely into analysts' hands. Your validation criteria are best expressed by the people who handle your policy every day, not by the IT department.

- RuleScript is built on top of the [Clojure programming language](htt://clojure.org), which leverages the industrial-strength Java Virtual Machine. Although you’d do fine to use only the built-in RuleScript functionality, all of Clojure is actually available to you in your RuleScript files. The language can become dramatically more powerful when you need it.

- RuleScript is available everywhere. You can use the RuleScript compiler on any desktop computer. You can access the compiler online. You can even use RuleScript as a software library from Clojure, Java, Scala, Kotlin, or any other Java Virtual Machine language.
