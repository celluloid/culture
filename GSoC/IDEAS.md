#### Leader Election/Services for DCell

The zk gem implements a (semi-)robust leader election protocol that works on top of Zookeeper. This can be leveraged to allow you to run the same DCell service on multiple nodes and have those services elect a leader that can be used for any behavior that requires coordination

#### Gossip/Paxos for DCell

We tried this before, but it didn't work out. I would love for someone to pick it up again! This would be an in-process replacement for Zookeeper in the above context like we had before, that could be used as an alternative in environments where Zookeeper is seen as an onerous dependency

#### Typesafe Console Clone

A dashboard of your entire Celluloid system, ala http://typesafe.com/products/console

#### Circuit Breaker implementation

An implementation of the [circuit breaker pattern](http://en.wikipedia.org/wiki/Circuit_breaker_design_pattern) for Celluloid and DCell, inspired by Netflix's [Hystrix](https://github.com/Netflix/Hystrix) library. This could potentially integrate with a Typesafe-style console.

#### Actor Supervision ([@digitalextremist](https://github.com/digitalextremist))

Take the combined ideas of Erlang and Akka towards a supervision model for Celluloid. 
Several large issues have been raised with the flexibility of supervision and pooling. 
The goal would be to have a solid solution for system and user level actors within Celluloid. 

#### Executing actors in a thread pool ([Ben Langfeld](http://langfeld.me))

One thread per actor often results in a *lot* of threads. If your actors aren't very busy, you've got a *lot* of threads not doing anything. If those actors were to be assigned to a dynamically sized thread pool, they could perhaps be more efficient about their thread usage if they're not so busy. This would hopefully reduce the amount of time the kernel spends juggling those threads across cores.

#### Deadlock detection ([@digitalextremist](https://github.com/digitalextremist))

In systems with complex interaction between Actors, it is possible for deadlocks to occur. This is particularly true when using the `exclusive` feature to force in-order processing of messages. Finding and debugging these deadlocks is very difficult. Creating a "deadlock detector" thread or providing more robust logging would be very useful for solving these issues when they arise.

#### Asynchronous integration testkit

Current state of the art is to send messages, loop to check and timeout or sleep and then check stuff. Would be great to provide asynchronous expectations kit, ala `expectMsg` in Akka:
http://doc.akka.io/docs/akka/2.1.0/scala/testing.html

#### Celluloid "Turbo Mode" for JRuby

[Celluloid](http://celluloid.io) is an actor-based concurrent object framework (somewhat similar to Akka) written in pure Ruby. It presently uses Ruby Mutexes and ConditionVariables for synchronization. However, the JVM has many, many other options which could provide better performance.

Celluloid provides an [ActorSystem](https://github.com/celluloid/celluloid/blob/master/lib/celluloid/actor_system.rb) abstraction for supporting multiple different platform-specific backends, and we'd love to have one specific to JRuby.

The goal of this project would be to implement a Celluloid `ActorSystem` which is a better fit with JRuby. Some options to consider:

* ***[concurrent-ruby](https://github.com/ruby-concurrency/concurrent-ruby)***: a cross-Ruby VM library containing many high-performance concurrency primitives. Leveraging this existing work would mean Celluloid would not need a JRuby-specific backend, but rather could use faster concurrency primitives on JRuby when available, and fall back to slower (single-threaded) ones on MRI.
* ***[LMAX Disruptor](http://lmax-exchange.github.io/disruptor/)***: Disruptor is a library which supports a number of different patterns for multithreaded execution. [Some work has already been done to implement Celluloid Mailboxes in terms of Disruptor](https://github.com/celluloid/celluloid/issues/342)
* ***[ArrayBlockingQueue](http://docs.oracle.com/javase/6/docs/api/java/util/concurrent/ArrayBlockingQueue.html)***: These are fast, fixed-sized data structures built atop arrays.
* ***[LinkedTransferQueue](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedTransferQueue.html)***: Introduced in Java 7, LinkedTransferQueues are one of the most adaptable concurrency primitives available on the JVM today.
* ***[Fork/Join](http://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html)***: a framework introduced in Java 7 for abstracting multicore execution on the JVM

#### IMAP::Server ( & ::Client ) and/or SMTP::Server ( & ::Client )

Similarly to how Celluloid::DNS provides a bare-bones DNS Server, write a bare-bones IMAP or SMTP Server using Celluloid::IO, in pure Ruby. There are many existing servers offering IMAP, SMTP, etc -- but these tend to be extremely large applications which are relatively slow, use a tremendous amount of resources, and which fail to progress as the internet evolves.

A Celluloid::IO based IMAP & SMTP servers could become the cornerstone of an increased trend of light-weight and yet highly robust and modifiable internet protocol servers, like Reel is for the Web.
