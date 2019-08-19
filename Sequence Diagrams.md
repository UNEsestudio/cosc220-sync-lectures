class: center, middle

# Sequence Diagrams

COSC220

---

### Sequence Diagrams

<img style="float: right;" src="/out/uml/seqDiagram/knockknock.svg" alt="Knock Knock Joke" />


* *Class diagrams* showed the structure of our entities (static relationships)

* *Sequence diagrams* can show the dynamic interactions between them

* Each participant has a *lifeline*. Time runs down the page.

* The box shows when that participant is *active*

* Solid lines represent messages being sent. Dotted lines are responses being returned synchronously.


<div style="clear: both;"></div>

---

### Sequence Diagrams in PlantUML

```plantuml
@startuml knockknock
hide footbox

actor Dad
actor Rebecca

activate Dad
Dad -> Rebecca : Knock Knock
activate Rebecca
return Who's there?

Dad -> Rebecca : Owls go
activate Rebecca
return Owls go who?

Dad -> Rebecca : Yes, they do
activate Rebecca
return groan
@enduml
```

---

### Creating and destroying

<img style="float: right;" src="/out/uml/seqDiagram/message_create.svg" alt="Create and destroy" />


* Creating participants is shown by putting their box further down the lifeline.

* Destroying participants is shown with an X

<div style="clear: both;"></div>

---

### Loops and Alts

<img style="float: right;" src="/out/uml/seqDiagram/marking.svg" alt="Alt and Loop" />

* *Alternatives* and *loops* are shown as fragments

* The condition is placed in [ square brackets ]

* *Optional* sections are also shown as a fragment, labelled "opt"

<div style="clear: both;"></div>


---

### Actors

<img style="float: right;" src="/out/uml/seqDiagram/atm.svg" alt="Alt and Loop" />

* So far, I've been showing people using the `Actor` logo (stick figure)

* Anything outside the "system boundary" (the part you are designing) is usually shown as an actor.

  e.g., if you're designing the ATM, that doesn't mean you're designing the bank


<div style="clear: both;"></div>

---

class: bottom


Written by Will Billingsley while at NICTA, UQ, and UNE

<small>
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.</small>