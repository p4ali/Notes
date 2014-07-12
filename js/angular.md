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


