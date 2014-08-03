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

