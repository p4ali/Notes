# Controller
invoice.js defines a module invoice which has  a invoice controller, which depends on the currencyConverter of the finance module.
`
angular.module('invoice',[finance]).controller('InvoiceController',['currencyConverter',function(currencyConverter{...};)])
`

finance.js defines a module finance which has a factory member of currencyController
`
angular.module('finance',[]).factory('currencyConverter',function(){...};)
`
