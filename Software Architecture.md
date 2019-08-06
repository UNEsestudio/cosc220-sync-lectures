class: center, middle

# Software Architecture

COSC220

---

## Software Architecture

* As well as implementing our functionality, we usually want our software to meet *non-functional requirements*.
--
  E.g.:

    * Handling enough concurrent requests
    * Performing well enough

--

* We also usually want it to have certain *quality attributes*.
--
  E.g.:

    * Maintainability
    * Extensibility
--

It's when you're trying to implement the needed *functionality*, meeting the *non-functional requirements*, with the required *quality attributes* that your team needs to consider the achitecture of the code.

--

**The architecture of your program helps you reason about your program**

---

## Simple code quality attributes

Generally, we want code to have *high cohesion* and *low coupling*.

![](http://www.plantuml.com/plantuml/svg/SoWkIImgAStDuU8gI4pEJanFLL1oL5Aevb9G0ABapABa7A18X18oBr89YHEb154QeQ2h40cXTHMYW8n828Eh5iba9v39I9e3KCmDH3Ot26fk0D3SG0Yjgn04P0I26M1pmLO4S74vfEQb0Bq00000)

**Cohesion** - how much an item within a module relates to other things within the module

**Coupling** - how much a module depends on the internal details of another module

---

## Simple code quality attributes

So, for example, this doesn't look as clean:

![](http://www.plantuml.com/plantuml/svg/SoWkIImgAStDuU8gI4pEJanFLL1oL5Aevb9G0ABapABa7A18X18oBr89YHEb154QeQ2h40cXTHMYkBXgaGnq0Xc8f2S0nRX0PEE2eCO508qBXD2w2a5Wuo91p00ki4WKX8hW2e9GN0wfUIb0Nm00)


--

That's easy to say. The harder part is *doing it*.

---

## Conways Law 

> Organizations which design systems ... are constrained to
> produce designs which are copies of the communication
> structures of these organizations

--

So it's a good idea if teams *also* have high cohesion and low coupling

![](http://www.plantuml.com/plantuml/svg/NSvD2i8m4CNn_PpYaNtkqDQF4tY18GurE2sIf0iHxousaKxT_kOD_BjSE9LbKg87XpkF0dSHdj0xS0RZHGI4c88ANAzZ56TWa5JsSf0GzVaL0jOzFEfi0uyw8zIJ9Nh_fmnhoh1FEV9D8piZ9qwpv6Bfd7WEabjDVO57MnhoQ5F2xsfmRTf2wnDH9_DrcVG3)

---

## Architecture and Patterns

Software engineers learn from the solutions they have seen before. We tend to call these solutions *patterns* -- we'll meet them more formally in a future week.

For the moment, let's look through a few examples of how architectures let us achieve different *non-functional requirements*.

---

## Layered Architectures

Each layer in a layered architecture deals with a different level of abstraction

![](https://developer.ibm.com/developer/tutorials/l-virtual-filesystem-switch/images/figure2.gif)

image: https://developer.ibm.com/tutorials/l-virtual-filesystem-switch/

--

Typically, for each layer, there is more than one way the layer below could be implemented.

---

## Layered Architectures

![](https://upload.wikimedia.org/wikipedia/commons/3/3b/UDP_encapsulation.svg)

image: Colin Burnett

---

## TCP over ...?

```
Network Working Group                                        D. Waitzman
Request for Comments: 1149                                       BBN STC
                                                            1 April 1990

   A Standard for the Transmission of IP Datagrams on Avian Carriers

Status of this Memo

   This memo describes an experimental method for the encapsulation of
   IP datagrams in avian carriers.  This specification is primarily
   useful in Metropolitan Area Networks.  This is an experimental, not
   recommended standard.  Distribution of this memo is unlimited.

Overview and Rational

   Avian carriers can provide high delay, low throughput, and low
   altitude service.  The connection topology is limited to a single
   point-to-point path for each carrier, used with standard carriers,
   but many carriers can be used without significant interference with
   each other, outside of early spring.  This is because of the 3D ether
   space available to the carriers, in contrast to the 1D ether used by
   IEEE802.3.  The carriers have an intrinsic collision avoidance
   system, which increases availability.  Unlike some network
   technologies, such as packet radio, communication is not limited to
   line-of-sight distance.  Connection oriented service is available in
   some cities, usually based upon a central hub topology.

Frame Format

   The IP datagram is printed, on a small scroll of paper, in
   hexadecimal, with each octet separated by whitestuff and blackstuff.
   The scroll of paper is wrapped around one leg of the avian carrier.
   A band of duct tape is used to secure the datagram's edges.  The
   bandwidth is limited to the leg length.  The MTU is variable, and
   paradoxically, generally increases with increased carrier age.  A
   typical MTU is 256 milligrams.  Some datagram padding may be needed.

   Upon receipt, the duct tape is removed and the paper copy of the
   datagram is optically scanned into a electronically transmittable
   form.

Discussion

   Multiple types of service can be provided with a prioritized pecking
   order.  An additional property is built-in worm detection and
   eradication.  Because IP only guarantees best effort delivery, loss
   of a carrier can be tolerated.  With time, the carriers are self-

```

---

## Layered Architecture

In our code, there are potentially three layers 

![Three layers](http://www.plantuml.com/plantuml/svg/LOun2W9134NxdEAp_TvXnUaiB2p58cQ3oAohJ4P1nBkRYGhQXVyUZmnMkTJhQIAwi6G-ABhbTDIvTdWG0TjLkPyXCUt0XYn4pnzxe-McvS-scDws-Tf0uifW4JKBa1Rhw8o-xzayb3vNIpcWBEZx5d2rNLzEDEWy-iil)

--

While we might not think of replacing the server, we might think of replacing the database.

*"Use Apache Derby in development, PostgreSQL in production"*

---

## Pipe and Filter 

Pipe and filter architectures are useful whenever there can be successive stages of processing.

![Pipe and filter](http://www.plantuml.com/plantuml/svg/SoWkIImgAStDuU8gIaqkISnBpqbLK4fEB55II2nM0DB8mkb5gGLWyNH3T645tJA8Z16oJ4vgSJ5O6CJWuW8Qfw1h1zIjm0N489OHa6K4P44LEAJcfG3T0W00)

```sh
cat out.log | grep ERROR | grep -v network | less
```


---

## Related - streams and filters

It is quite common to have a stream of events reaching a bus. You might, however, want to be able to listen to a filtered stream

* *"Just give me the messages from Australian senders"*

--

or to transform them in some way

* *"Now translate those messages into French for me"*

---

## Enterprise service bus

![Service bus](http://www.plantuml.com/plantuml/svg/SoWkIImgAStDuU9AIIn9J4eiJbLGyaqjBavCJrKeB4qjJLLII2nMACejLAZcgkNYYlRDJodDILLmZ5MmqRK3YIF4d41Yw8BEo8900gmD9kaIgy35vP2Qbm9q0000)

* Different processes within a company listen to a shared message queue. 

* Each process might listen to a filtered version of the stream. e.g., the provisioning service only cares about "user created" events

---

## Pipe-and-Filter in our code

In our code, we have several places where streams of events might occur:

* Input events in the client

* Messages received over the network in the server

* Messages received over the network in the client

Any of these could be piped and filtered to interested listeners.

```java
addListener(ListenerBuilder.forClass(Disconnect.class).onReceive((conn, disconnect) -> {
    logger.info("Disconnect notification from {}", conn.getID());
}));
```

---

## Broker architectures

A *broker* receives messages and passes them on to services that are available. e.g., maitre d at a restaurant finding you an available table.

![](http://www.plantuml.com/plantuml/svg/SoWkIImgAStDuU9ISix9JCqhKUBYoijFILLmAihFJYs2SiBpYu0SXSHYXKHqWIHqWMGkBeX92ZQwkdOmSo0KH2WHXPU4p0FfTaZDIm6w2000)

---

## Broker architectures

Common uses in tech - 

* Load-balancing requests between many servers

* Automated fail-over if a server becomes unavailable

* Task-runners. e.g., if a continuous integration server has many projects to build, it might have several worker machines that it can set to work doing the building.

---

## Broker architecture in our code

Inside the server, there is a `WorkerManager` for handling long-running tasks. This keeps some (5) Worker actors that can handle requests.

![](http://www.plantuml.com/plantuml/svg/RP2n2W8n343tV4Mu_Vv0vDHHd1mSn27N1f4x6qch8EA_6uaEXKxblTUKq24NqdA_pd2ZCD6PiTkpFbWxV04Sj_eKT752ohYPBvmAG99eDm-Y4-kEaitPWFMrYfbVzxmAHVJRg6d7fWrD6vkM0NmjNFJzyh-27wweeh6YK56io5v-my0fslzy0000)

---

## Working with background tasks

To send a message to the workers

```java
workerManager.schedule(ping, reply -> {
    logger.info("I sent my ping to a background worker, and all I got in return was this lousy {}", reply);
});
```

* The first parameter is the message to send

* The second is a function describing what to do with the reply

---

## Defining what the workers do

The workers sit waiting for messages to come in

```java
@Override
public Receive createReceive() {
    return receiveBuilder()
        .match(Ping.class, 
        // This matches Ping messages, to show an example of how the worker can do things in response to
        // receiving a ping.
        ping -> {
            ActorRef sender = sender();
            logger.info("Worker received a ping from {}", sender);
            sender.tell("String saying I got your ping", self());
        }
        )
        .matchAny(
            o -> logger.info("Worker received a message {}", o)
        )
        .build();
}
```




