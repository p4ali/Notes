## Angular
* Client-side templates - Template and data shipped to the browser and then are assembled there. Most classic multi-page web 
appliction create their HTML by assembling and joining it with data on the server, and then shipping the finished pages up to 
the browser.
* Model View Controller
** The `model` is what views are displayed, or what retrieved/save from/to the server. Model is JSON objects.
** The `controller` holds the business logic : how to retrieve model, operations on the model. Validation of the model, make
server calls, bootstrapping your view with the right data. Controller is javascripts.
** The `view` or the `template` represents how to display models. View is html/css files.
* Data Binding - Before AJAX single-page apps were common, plaforms like Rails, PHP or JSP helped us create user interface by
mergeing strings of HTML with data before sending it to the users to display it. Libraries lik jQuery extended thsi model to 
the client and give us the ability to update, part of the DOM separately by setting `innerHtml` on a placeholder element. Angularr
helps us to automate this kind of operation with data binding. You can declare the mapping between UI and javascript properties,
Angular will keep them sync automatically.
