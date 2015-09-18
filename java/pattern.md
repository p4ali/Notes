# Caveat / mantras

## `Favor object composition over class inheritation`
* One reason is because multi-inherit may cause `diamond problem`, i.e., it's hard to make inherit distinct.
* Use interface to support `polymorphic` behavior. Classes who implements the interface can be added to the domain classes as needed.
* one drawback is you have to implement all methods in the interface. 

## Dependency Inversion Principle (DIP)
A specific form of decoupling software modules, where the conventional dependency from high-level to low-level is inverted.
Thus rendering high-level modules independent of the low-level module implementation details.
The principle state:
* High-level modules should not depend on low-level modules. Both should depend on abstractions.
* Abstractions should not depend on details. Details should depend on abstractions.

The DIP basically make the IoC possible.

In a direct application of dependency inversion, the abstracts are owned by the upper/policy layers. The architecture groups 
the higher components and abstracts that define the lower services together in the same package(platform). The lower-level
layers are created by inheritance/implementation of these abstract classes or interfaces.

In case uppler layers wants to recuse lower-level services, an `Adapter` can be used to mediate between the service and abstractions.


## Inversion of Control(IoC) / Hollywood principle
IoC descirbes a desigh in which custom-written portions of a computer program receive the flow control from a generic, reusable
library. The generic reuseable library usually called `framework`, and it provide extensions for custom code to plug in.
This is inversion of traditional design where task specific code will call generic library.

## Dependency Injection
A software desigh pattern that implements inversion of control for resolving dependencies.
A dependency(service) is an object that can be used. An injection is to passing of a dependency(service) to a dependent object(a client) who uses it. The service is made part of the client's state. Passing the service to the client rather than allowing client to build or find the service, is the fundamental requirement of the pattern.

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

## Singleton vs Monostate
Usually there is a one-to-many relationship between classes and instances. 
* Singleton pattern is a one-to-one mapping between class and instance.
* Monostata is one-to-many between class and instance, but since all instances share same variables(e.g., with static variables), they behave same
 
## Null Object
Always provide a valid object even when they fail(null object) instead of `null`. The null object is a default implementation of the interface. This way we can avoid the null check from caller.

# Behavioral pattern

## Command and Active Object
Command encapsulate all the information to excute an action or trigger an event at later time. 
Active object decouple the method execution from the method invocation for objects each reside their own thread control(for concurrency).
* Command interface: `do()`,`undo()`
* Active Object: `run()` 

Similar pattern: `FutureTask and Executor`

## Template method vs Strategy
They both solve the problem of separating a generic algorithm from a detailed context. The difference is:
* Template method use inheritance
* Strategy use delegation

## Facade vs Mediator
They both impose some kind of policy on another group of objects.
* Facade impose policy from above. The use of facade is visible and constraining.
* Mediator impose policy from below. The use of mediator is invisible and enabling.
* An example of Facade would be DB driver, it provide all functions client can use without go detail in to sql package. It's visible because you have to call it.
* An example of Mediator would be in the context of a dialog, mediator mediates the UI controls interaction, and decopuling the connection between all controls. It's invisible because you do not need to know it from outside of the dialog.

