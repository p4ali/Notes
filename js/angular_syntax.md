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
The Promise interface contains *success* and *error* methods.

```javascript
$http.get('api/user', {params: {id: '5'}})
.success(function(data, status, headers, config) {
// Do something successful.
})
.error(function(data, status, headers, config) {
// Handle the error
});
```
