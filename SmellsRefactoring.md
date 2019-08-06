class: center, middle

# Smells & Refactoring

---

## Just because it works doesn't mean it's good code

> Any fool can write code a computer can understand.
> Good programmers write code that humans can understand.
-- Martin Fowler


---

## Design patterns had a pattern...

* Name
* Problem
* Design fragment

--

*But what if the problem is "my code stinks"?*

---

## What do we want code to do...

Not just *work now* but *be maintainable in the future*

Advice by Will: Half the art of software design is making life easy for yourself

---

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr">Developer progression: instead of junior to senior<br>1. Simple and wrong<br>2. Complicated and wrong<br>3. Complicated and right<br>4. Simple and right</p>&mdash; Tim VanFosson (@tvanfosson) <a href="https://twitter.com/tvanfosson/status/765913312894877700">August 17, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

---

## Code smell

A heuristic or sign that your code might be badly designed

[https://sourcemaking.com/refactoring/smells](https://sourcemaking.com/refactoring/smells)

---

### Duplicated code

Very similar code exists in more than one location

--

**Don Robert's "rule of three"**:

The first time, there's no duplication. 
The second time, sign and put up with it.
The third time, refactor

---

### Long method

* Long methods get complicated (many paths).
* That means they can be hard to reason about.
* That means if there isn't a bug yet, there probably will be soon

*Maintainability is not about the bugs you have now, but about how many your code will attract as it ages*

---

### Method takes too many parameters

* Perhaps it doesn't have a clear purpose?

---

### "Speculative generality"

Complex machinery for things we *might need*, but don't.

> Ok, so someone might be a student in one class and a tutor in another...
>
>... but we probably really don't need to deal with the case where they are a student and a lecturer in the same class.

---

class: center, middle

> Hmm... "duplicated code" sounds like something we could
> detect automatically...

---

### Static Analysis

* Programs that *analyse your code* and find common problems

* Classic example is [FindBugs](http://findbugs.sourceforge.net/)

---

### Integrating Static Analysis in the buildchain

* Testing is part of our toolchain... Static analysis can be too

* Fairly common to run *multiple* programs across the project, and gather *code quality metrics*

* Takes time, so it's usually a special build or a post-build action (re-reruns the build with several extra plugins)

* Put the results in a database and run a dashboard so the team can see how code quality is changing over time... (SonarQube)[https://sonarqube.com/]

---

### Ok, so now we knw our code stinks. Now what?

* Just as design patterns had suggested solutions, there's suggested solutions to code smells too..

---

## Refactoring (n)

A change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behaviour

---

## Refactor (v.t.)

to refactor software by applying a sequence of refactorings without changing its observable behaviour

---

class: center, middle

### how do we know it hasn't changed behaviour?

---

### Refactoring

* Strictly, a refactoring is a *transformation* applied to some code. 

* Most IDEs parse code into an Abstract Syntax Tree. Trees are data ... we can manipulate trees...

* Many IDEs have a *Refactoring* menu that can alter your code in known ways

---

### Extract Method

You have a code fragment that can be grouped together
Turn the fragment into a method whose name explains the purpose of the method

---

### Inline Method

A method’s body is just as clear as its name
Put the method’s body into the body of its callers and remove the method.

---

### There are catalogs of known refactorings

[http://refactoring.com/catalog](http://refactoring.com/catalog)

and yes, they relate to patterns. For example:

"Replace Type code with state/strategy"

---

### How does it fit into development

> ...first refactor the program to make it easy to add the feature, then add the feature...
-- Martin Fowler

---

### Refactoring example

Tony Weston wrote up the first example from Martin Fowler's book and put it up as a GitHub repository.

This is a fork:

[https://github.com/UNEsestudio/Refactoring-Chapter-1](https://github.com/UNEsestudio/Refactoring-Chapter-1)

