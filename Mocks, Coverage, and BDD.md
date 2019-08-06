class: center, middle

# Mocks, More test categories, and BDD

---

### "Test doubles" (stand-in objects used in tests)

* **Dummy objects**: passed around but never actually used. 

* **Fake objects**: have working implementations, but not suitable for production. 

    * eg, in-memory database.

* **Stubs**: provide canned answers to the calls made during the test.* **Spies**:stubs that record some information from how they were called. 

    * eg, count how many times called

---

* **Mocks**: stubs that can verify what calls were made, and throw an exception if an unexpected call is made

Hence *Mockito*...


---

### Mockito

* Popular Java Mocking library. [mockito.org](http://mockito.org/)

```java
import static org.mockito.Mockito.*;

// mock creation
List mockedList = mock(List.class);

// using mock object - it does not throw any "unexpected interaction" exception
mockedList.add("one");
mockedList.clear();

// selective, explicit, highly readable verification
verify(mockedList).add("one");
verify(mockedList).clear();
```

---

### Mockito when...

```java
// you can mock concrete classes, not only interfaces
LinkedList mockedList = mock(LinkedList.class);

// stubbing appears before the actual execution
when(mockedList.get(0)).thenReturn("first");

// the following prints "first"
System.out.println(mockedList.get(0));

// the following prints "null" because get(999) was not stubbed
System.out.println(mockedList.get(999));
```

---

### Here's a tutorial / challenge exercise I made earlier

[The Key](https://github.com/wbillingsley/tutorial-thekey-mock-public)

---

### Awkward wrinkle with Akka

* You can't mock an `ActorRef`

* To mock a class, Mockito has to be able to extend it

* But (details omitted), something inside `ActorRef` is marked `final` and cannot be extended.

* If you need to test Akka actors: [Akka's page on testing](http://doc.akka.io/docs/akka/snapshot/java/testing.html)

    * (or let me know, and I'll try to help you through it)


---

### Unit v Integration testing

Suppose module A and module B are supposed to work together

* We can *unit test* module A by testing it with a *mock* Module B

* We can *unit test* module B by testing it with a *mock* Module A

* We can *integration test* modules A and B by writing a test that wires them together and checks they work together

(We sometimes also use "integration testing" to mean testing our system with the external systems it integrates with)


---

## What kinds of tests do we need?


.    | support programming | critique project 
---- |--------             |---------
business facing | functional acceptance tests | showcases <br/> usability testing <br/> exploratory testing 
tech facing | unit tests <br/> integration tests <br/> system tests | nonfunctonal acceptance tests <br/>(capacity, performance, security...)

*diagram based on Brian Marick*

**Usability testing**: how usable is the system by real users? (has to be manual, as we need to try it with users)


---

## Functional acceptance tests

* Does the system do what your *client* wants?

* At some point, system requirements are specified

    * Maybe along the way (agile methodologies)
    * Maybe up-front (plan-and-document methodologies)

* Once a feature works, we'd like 

* **Acceptance tests** are traditionally run by the *client* to see if the system does what they're paying for (whether to accept it and write the cheque...)

---

### Behaviour driven development

* What if we wrote our specification so clearly it was executable?

* *Automated* functional acceptance tests

* Typically, domain-specific language for writing tests

    * eg, Cucumber, Specs2

---

### Cucumber

```cucumber
Feature: Refund item

  Scenario: Jeff returns a faulty microwave
    Given Jeff has bought a microwave for $100
    And he has a receipt
    When he returns the microwave
    Then Jeff should be refunded $100
```

---

### Specs2

```scala
import org.specs2._

class HelloWorldUnitSpec extends mutable.Specification {
  "The 'Hello world' string" should {
    "contain 11 characters" in {
      "Hello world" must haveSize(11)
    }
    "start with 'Hello'" in {
      "Hello world" must startWith("Hello")
    }
    "end with 'world'" in {
      "Hello world" must endWith("world")
    }
  }
}
```

---

### Plain ol' JUnit

* Do non-technical customers *really* try to read our test code, even in a DSL? (open question)

* Developers need to learn the DSL (additional overhead to starting development)    
    
* Possibly main benefit is getting *test-writers* (ie, developers) to think in terms of business behaviour

1. **Given**...

2. **When**...

3. **Then**...


