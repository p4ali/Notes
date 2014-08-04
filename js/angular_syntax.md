# Module
Modules allow us to declaratively specify application's dependencies, and how the wiring
and bootstrapping happends. Reason:
* It's declarative - easier to write and understand
* It's modular - it forces you to think about how to define your components and dependencies,
  and make them explicit.
* It allows easy testing.

## Modules loading
Two distinct phases:
* The Config block - AngularJS hooks up and registers all the providers in this phase.
  Only providers and constants can be injected intothe config blocks. Services that
  may or may not have been initialized cannnot be injected.
* The Run block - to kickstart your application, and start executing after the injector
  is finished creating. Only instances and constants can be injected into the Run blocks.
  The Run block is the closest you are going to find a main method in AngularJS.

## Convenience Msthods

| API method   | Description  |
|--------------|:-------------|
| config(configFn) | register work that needs to be done when the module is loading |
| constant(name, object) | This happens first. You can declare all constants app-wide, and have them available at all configuration and instance methods |
| controller(name, constructor) | setup a controller for use |
| directive(name, directiveFactory) | to create a directive withing app|
| filter(name, filterFactory) | to create AngularJS filters|
| run(initializationFn) | To perform work that needs to happen once the injector is setup, right before your app is available to the user|
| value(name, object) | allows values to be injected across the application|
| service(name, serviceFactory) | Service incoke "new" on the constructor method passed to it and returns the result |
| factory(name, factoryFn) | A factory is a function that is responsible for creating a certain value (or object) |
| provider(name, providerFn) | tbd |


# Controller #

invoice.js defines a module invoice which has  a invoice controller, which depends on the 
currencyConverter of the finance module.

```javascript
angular.module('invoice',[finance]).
  controller('InvoiceController',
    ['currencyConverter',function(currencyConverter{...};)])
```

finance.js defines a module finance which has a factory member of currencyController

```
angular.module('finance',[]).
  factory('currencyConverter',function(){...};)
```

# $http
The Promise interface contains *success* and *error* methods. The request type may also be 
JSONP in addition to GET, HEAD, POST, DELETE, and PUT.

```javascript
$http.get('api/user', {params: {id: '5'}})
.success(function(data, status, headers, config) {
// Do something successful.
})
.error(function(data, status, headers, config) {
// Handle the error
});

$http({
  method: string,
  url: string,
  params: object, # url parameters
  data: string or object, # message data
  headers: object,
  transformRequest: function transform(data, headersGetter) or
    an array of functions,
  transformResponse: function transform(data, headersGetter) or
    an array of functions,
  cache: boolean or Cache object,
  timout: number,
  withCredentials: boolean
})
```

# Directive
Note, you must use camel-cased name for the directive (Augular use a  normalized naming
scheme for directives ), e.g, namespaceDirectiveName. And this way Angular will be able 
to handle different formats, e.g., namespace-directive-name, namespace-directive:name, 
data-namespace-directive-name, and x-namespace-directive-name.

Angular's initilization process:
* **Script loads** - Angular loads and looks for **ng-app** directive to find the 
  application boundaries.
* **Compile phase** - Angular walks the DOM to identify all the registered directives
  in the template. For each directive, it then transforms the DOM based on the
  directive's rules (template, replace, transclude, and so on), and calls the **compile**
  function if it exists. The result is a compliled **template** function, which will
  involve the **link** functions collected from all of the directives.
* **Link phase** - To makd the view dynamic. Augular runs a **link** function for 
  each directive. The **link** functions typlically creates listeners on hte DOM or
  the model. These listeners keep the view and model in sync at all times.

```javascript
var myModule = angular.module(...);
myModule.directive('namespaceDirectiveName', function factory(injectables) {
  var directiveDefinitionObject = {
    restrict: string,
    priority: number,
    template: string,
    templateUrl: string,
    replace: bool,
    transclude: bool,
    scope: bool or object,
    controller: function controllerConstructor($scope,
                                               $element,
                                               $attrs,
                                               $transclude),
    require: string,
    link: function postLink(scope, iElement, iAttrs) { ... },
    compile: function compile(tElement, tAttrs, transclude) {
      return {
        pre: function preLink(scope, iElement, iAttrs, controller) { ... },
        post: function postLink(scope, iElement, iAttrs, controller) { ... }
      }
    }
  };
  return directiveDefinitionObject;
});
```
**angular.element()** can be used to query DOM, it includes:
* addClass()
* bind()
* find()
* toggleClass()
...

some example of using angular.element():
```javascript
link: function(scope, element, attrs) {
  var titleElement = angular.element(element.children().eq(0));
  var bodyElement = angular.element(element.children().eq(1));
  
  titleElement.bind('click',toggle);
  
  function toggle() {
    bodyElement.toggleClass('closed');
  }
}

.closed {
  display: none:
}
```

## Controllers
If you have nested directives that need to communicate with each other, then you 
need a controller. You can declare a controller as part of a directive:

```javascript
controller: function controllerConstructor($scope, $element, $attrs, $transclude)
```

A *<menu>* item may need to know about the *<menu-item>* elements 
inside it, so it can show and hide them appropriately. Or a *<tab-set>* knowing
about its *<tab>* elements, or a *<grid-view>* knowing its *<grid-element>* elements.

# $location
The $location service is a wrapper around the *window.location* that is present
in any browser.

