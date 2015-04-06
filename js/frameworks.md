## Angular
### Client-side templates - Template and data shipped to the browser and then are assembled there. Most classic multi-page web 
appliction create their HTML by assembling and joining it with data on the server, and then shipping the finished pages up to 
the browser.
### Model View Controller
* The `model` is what views are displayed, or what retrieved/save from/to the server. Model is JSON objects.
* The `controller` holds the business logic : how to retrieve model, operations on the model. Validation of the model, make
server calls, bootstrapping your view with the right data. Controller is javascripts.
* The `view` or the `template` represents how to display models. You create your view by writing a template as HTML page and merging it iwth data from your model. View is html/css files.
### Data Binding - Before AJAX single-page apps were common, plaforms like Rails, PHP or JSP helped us create user interface by
mergeing strings of HTML with data before sending it to the users to display it. Libraries lik jQuery extended thsi model to 
the client and give us the ability to update, part of the DOM separately by setting `innerHtml` on a placeholder element. Angular helps us to automate this kind of operation with data binding. You can declare the mapping between UI and javascript properties,Angular will keep them sync automatically.
### Dependency Injection
Angular maintain a group of variable available at different level, you can always ask for them without initializing it.
```javascript
function MyController($scope, $location){
  $scope.greeting - {text: 'hello'}
  ...
}
```
### Directives
These directive such as `ng-controller`, `ng-model` and so on, care HTML extension directives. You can also write you own.
Two phases for directive: compile and link. First, compile time will find and attach to DOM element. Second, link time will
link scope to the produced template in compile time.
### Services
Service are singletons which can be injected to Angular assets.
### Controllers
