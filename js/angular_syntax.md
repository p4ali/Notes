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
