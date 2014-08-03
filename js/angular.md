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
each $scope is an instance of the *Scope* class. THe *Scope* class has methos that control the scope's
lifecycle, provide event-propagation facility, and support the template rendering process.

AngularJS will create a new instanceof the *Scope* class whenever it encounters a scope-creating
directive in the DOM tree. A newly-created scope will point to its parent scope using the **$parent**
property.

Scope form a parent-child, tree-like relationship rooted at teh **$rootScope** instance. As scopes'
creationg is driven by the DOM tree, it is not surprising that scopes' tree will mimic the DOM structure.

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
