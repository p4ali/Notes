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
