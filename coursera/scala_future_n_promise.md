## [Future](http://docs.scala-lang.org/overviews/core/futures.html)
Futures are defined as a type of read-only placehoder object created for a result which does not yet exist. Future is excuted
thread pool. (See `ExecutionContex`, it is backed by a work-stealing thread pool, or [ForkJoinPool](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html)).

You can create future with `Future` method. You can register callback to a future `f` by `f onComplete` :
```scala
import scala.util.{Success, Failure}
val f: Future[List[String]] = Future {
  session.getRecentPosts
}
f onComplete {
  case Success(posts) => for (post <- posts) println(post)
  case Failure(t) => println("An error has occured: " + t.getMessage)
}
```

## Promise
Promise can be thought of as a writable, single-assignment container, which completes a (associated) future on assignment. 
The value can be assigned by calling methods such as `success(value)`, `failure(throwable)` and so on. 

Each Promise `p` is associated with a future `f`, which can be retrieved by calling `f=p.future`. Once the value is assigned
(either by calling `p.success(value)` or `p.failure(throwable)`, the associated future `f` is completed. If you are listen
to the future `f`, then you will be notified. The following is an example:

```scala
import scala.concurrent.{ Future, Promise }
import scala.concurrent.ExecutionContext.Implicits.global

val p = Promise[T]()
val f = p.future
val producer = Future {
  val r = produceSomething()
  p success r
  continueDoingSomethingUnrelated()
}
val consumer = Future {
  startDoingSomething()
  f onSuccess {
    case r => doSomethingWithResult()
  }
}
```

In above example, the consumer can obtain the result before the `producer` task is finied executing the `continueDOdingSomethingUnrelated()` method.

