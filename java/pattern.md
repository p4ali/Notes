# Creation pattern

## [Initialization-on-demand holder] (https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom) 
lazy loaded singleton. A static holder class owns the caller's singleton statically, caller use its static method to get holder class's singleton.
holder class is not initialized when caller is initialized. It is only initialized when caller's static method is called.
```
public class Something {
    private Something() {}

    private static class LazyHolder {
        private static final Something INSTANCE = new Something();
    }

    public static Something getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```

# Behavioral pattern

## Command and Active Object
Command encapsulate all the information to excute an action or trigger an event at later time. 
Active object decouple the method execution from the method invocation for objects each reside their own thread control(for concurrency).
* Command interface: `do()`,`undo()`
* Active Object: `run()` 

Similar pattern: `FutureTask and Executor`
