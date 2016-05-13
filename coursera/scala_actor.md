## Concurrency using Actor
An actor in scala provides an event-based lightweight thread. It's scala's primary concurrency constructs. 
Actors communicate by exchanging messages. Actors can also be seen as a form of active objects where invoking
a method conrresponds to sending a message.

The scala actors library provides both asynchronous and synchronous message sends (The latter are imiplemented
by exchanging several asynchronous messages). Moreover, actors may communicate using futures where requests are
handled asynchronously, but return a representation (the future) that allows to await the reply.

* To create an actor, simply use the method named `actor()` in the `scala.actors.Actor` companion object. It
  accepts a function value/closure as a parameter and starts running as soon as you create it.
* To send a message to an actor, use the `!()` method.
* To receive a message from an actor, use the `receive()` method. The `receive()` method accepts a closuer as well.
* Typically, you'd use pattern matching to process the received message.
* 

## [Actor-Based Concurrency](http://cs.nyu.edu/~lerner/spring12/Preso01-Actors.pdf)

Actor has:
* a thread pool, each thread in pool will start reading from mailbox
* a blockingqueue (mailbox)
* a load factor buffer

An Executor is monitoring and manage all registered actors, and use the mailbox's load factor buffer to balance the load
(by assign more or less threads to each actor)

### [A Java impl of Actor](https://github.com/edescourtis/actor)

Basically using a thread as an actor, and use blockqueue as mailbox, the thread run() method will loop the mailbox, do a
put() and take() to process the message based on the recieve() logic passed in to an actor.

## [Thread partitioning](http://www.blackpepper.co.uk/thread_partitioning_to_handle_actor_based_concurrency_in_java_/)
