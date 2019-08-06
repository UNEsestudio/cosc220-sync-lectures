class: center, middle

# Code coverage & mutation testing

---

## The trouble with tests

* Poor code can pass poor tests. Not what we want.

* Any code that compiles can pass all the tests, if the tests never call it. Not what we want.

--

* So how do we know our tests are any good?

---

## Code coverage

* **Statement coverage**

  What proportion of statements in my code are exercised by my tests?

--
  
* **Branch coverage**

  What proportion of branches are covered in my tests? 
  
  An `if` statement has two branches; my tests might not try both branches

--
  
* **Path coverage**

  What proportion of possible paths through my code are covered in my tests?
  
  Two `if` statements in a method have 4 paths  
  Three `if` statements in a method have 8 paths.  
  etc.

---

## Example

(Worked in class)

---

## That sounds like work...

* May be an exam question on it...

* But generally, why do it manually when we can get a machine to do it for us?

---

## JaCoCo

* **Ja**va **Co**de **Co**verage

    ```gradle
    apply plugin: "jacoco"
    ```
    
    ```bash
    gradle jacocoTestReport
    ```

* https://docs.gradle.org/current/userguide/jacoco_plugin.html

---

## The trouble with test coverage

```java
public double calculateThrust(double x, double y) {
  double determinant = Complex.flugenFactor(x, y, FLUGEN_CONSTANT);
  double spoodle = Mindboggling.spoodle(x, y, NUM_ELEPHANTS, getWeatherInOslo());
  
  return Math.max(determinant, determinant / Math.log(spoodle));
}
```

```java
@Test
public void testCalculateThrust() {
  double x = 50d;
  double y = 103d;
  double result = calculateThrust(x, y);
  
  assertEquals("1 wasn't 1", 1 == 1);
}
```


---

## What if we test the tests?

* A test should fail if there's a bug in our code

* A random change to our code is highly likely to introduce a bug

* What if we had our test suite randomly change our code, and measured whether  the changes were caught by the tests?

* "Mutation testing"

---

## Recent findings (2014)

* http://www.linozemtseva.com/research/2014/icse/coverage/coverage_paper.pdf

* Used mutation testing to compare effectiveness of test suites with similar *number of test cases* but different *code coverage*

>  Our results suggest that
coverage, while **useful for identifying under-tested parts of a
program**, should not be used as a quality target because it is
not a good indicator of test suite effectiveness.



