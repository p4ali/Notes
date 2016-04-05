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