MVC
----
* The **model** is the truth. Your entire application is driven by the model.
* The **controller** is the business logic or behavior: how to retrive the model, what kind of operation to perform on it.
* The template represents how your model is displayed, and how the user will interact withe the application. The **view** is the compiled versino of the template that gets executed.
 * display your model
 * define the way user can interact with the applicaiton (clicks, input fields, and so on)
 * styling the app
 * filtering and formatting your data (both input and output)

Kepping the template simple allow a proper separation of concerns, and also ensures simple testing:
* test most code using only unit test (with Mockup and Jasmine). 
* test Templates with scenario (integration) test.
 * load application
 * browse to a certain page
 * click around the enter text willy-nilly
 * ensure that the right thing happen

DI
---
Dependency Injection (DI) is a software design pattern that deals with how objects and functions get
created and how they get a hold of their dependencies. Everything within Angular (directives, filters
controllers, services,...) is created and wired using depnendency injection. Within Angular, the DI
contriner is called the 'injector'.

To use DI, there needs to be a place where all the things that should work together are registered.
In Angular, this is the purpse of the so called **'modules'**. When Angular starts, it will use
the configuration of the module with the name defined by the **ng-app** directive, including the 
configuration of all modules that this module depends on.

Scope
-----
Scope is a context that you use to make changes to your model observables. Or a container contains
your model observables. Any changes made to your model observable will automatically propagate
to all handlers (which will update the UI). You need manually call scope.$apply() to get your 
bindings update when Angular does not know whether the code will make changes to model observables
or not. For example, if you made changes to model observables inside of an native key event 
handler etc. Or any changes to scope from outside of AngularJS api.

You may have other data in your application, but Angular only considers it part of the model when
it can reach these properties through a scope.

### Javascript is turn based
Remember, Angular databinding is property-based, not method based. So setter method will not 
trigger model update event. The model update event is only triggered at the end of current turn
invoked by AngularJS API. 
```javascript
function Ctrl($scope) {
  $scope.message = "Waiting 2000ms for update";
    
    setTimeout(function () {
        $scope.$apply(function () {
            //Following code must be called within apply(), since AngularJS unaware of the update to $scope.
            $scope.message = "Timeout called!"; 
        });
    }, 2000);
}
```

Scopes are necessary to provide isolated namespaces and avoid variable name collision.
each $scope is an instance of the *Scope* class. THe *Scope* class has methos that control the scope's
lifecycle, provide event-propagation facility, and support the template rendering process.

AngularJS will create a new instanceof the *Scope* class whenever it encounters a scope-creating
directive in the DOM tree. A newly-created scope will point to its parent scope using the **$parent**
property.

Scope form a parent-child, tree-like relationship rooted at the **$rootScope** instance. As scopes'
creationg is driven by the DOM tree, it is not surprising that scopes' tree will mimic the DOM structure.

### Scope lifecycle
WHen one of the scopes is not longer needed, it can be destroyed. As a resutl, model and functionality
exposed on this scope will be eligible for garbage collection. Related methods from **Scope** type:

* **$new** - create a scope
* **$destroy** - destroy a scope

### Heriarchy of scopes and the eventing system
Scope organized in a hierachy can be used as an event bus. An event can be dispatched starting
from any scope and travel either upwards (**$emit**) or downwards (**$broadcast**)

AngularJS core services and directives makd use of this event bus to signal important changes
in the application's state. The **$scope.$on** method available on each scope instance
can be invoked ot reigster a scope-event handler. e.g.

```javascript
$scope.$on('$locationChangeSuccess', function(event, newUrl, oldUrl){
// react on the location change here
// for example, update breadcrumbs based on the newUrl
});

```

Similar to DOM events, we can call preventDefault() and stopPropagation() methods on event 
object. The latter will prevent an event from bubbling up the scopes' hierarchy, and only
availabel for events dispatched upwards in the hierarchy ($emit)

### Communicationg between Scopes with $on, $emit and $broadcast

```javascript
scope.$on('planetDestroyed', function(event, galaxy, planet){
  // Custom event, so what planet was destroyed
  scope.alertNearbyPlanets(galaxy, planet);
});

// communicate upwards
scope.$emit('planetDestroyed', scope.myGalaxy, scope.myPlanet);

// communicate downwards
// parent scope
scope.$broadcast('selfDestructSystem', targetSystem);

// child scope
scope.$on('selfDestructSyste', function(event, targetSystem){
  if(scope.mySystem === targetSystem) {
    scope.selfDestruct(); 
  }
});
```

### event object
| Property of Event   | description |
|---------------------|:-------------:|
| event.targetScope | the scope which emited or broadcasted the event originally |
| event.currentScope | the scope currently handling the event |
| event.name | the name of the event |
| event.stopPropagation() | A function which will prevent further event propagation (only available for $emitted event)|
| event.preventDefault() | do nothing, but set defaultPrevented=true|
| event.defaultPrevented | true if preventDefault() was called |

Observing Model Changes with $watch
------------------------------------
You can watch individule object properties and computed results(functions), i.e., anything that
could be accessed as a property or computed as JavaScript function.
```javascript
// watchFn a string with Angular expression a a function that return the current value of 
//         the model you want to watch. This expression must have no side effects.
// watchAction the function or expression to be called when watchFn changes. And its 
//        signature is function(newValue,oldValue,scope)
// deepWatch if true, Angular will examine each property within the watched object for changes.
//        You'd use this if you want ot watch individual elements in an array or properties
//        in an object instead of just a simple value. It's optional. default false.
// return a function that will de-register the listener when you no longer to receive change 
//        notifications
$watch(watchFn, watchAction, deepWatch)


// An alternative, which is a sort-of mid-ground between deep watch and shallow watch.
$watchCollection(watchFn, watchAction)

//e.g. to deregister after finish
var dereg = $scope.$watch('comeModel.someProperty',callbackOnChange());
...
dereg();

```

Imperative and Declarative Approach
------------------------------------
* The imperative style of programming focuses on describing individual steps leading to a desired outcome.
* The declarative approach focus on describing the desired result. The individual steps taken
  to reach to the result are taken care of by a supporting framework. It's like saying "Dear
  AngularJS, here is how I want my UI to look when the model ends up witn a certain state. Now
  please go and figure out when and how to repaint the UI". 

Declarative template view - the imperative controller logic
------------------------------------------------------------
Directives in AngularJS templates declaratively express the desired effect, so we are freed
from providing step-by-step instructions on how to change individual properties of DOM elements.
AngularJS heavily promotes declarative style pf programming for templates and imperative one
for the JavaScript code (controllers and business logic).

The recurring pattern: To manipulate UI,  we only need to touch a small part of a template
and describe a desired outcome interms of the model's state. We don't need to keep any references
to DOM elements in the JavaScript code and we are not obliged to manipulate the DOM elements
explicitly. Instead we can simply focus on model mutations and let AngularJS dot he heavy lifting.
All we need to do is to prvide some his in the form of directives.
