## To set cookie

```javascript
document.cookie="abc=123; expires=Thu, 18 Feb 2017 12:00:00 UTC; path=/";"
```

## TO post

```javascript
function post(json) {
  var xhr = new XMLHttpRequest();
  xhr.open('POST', '/myapi', true);
  xhr.responseType = 'json';
  xhr.setRequestHeader('Content-Type', 'application/json');
  xhr.onload = function(e) {
      console.log(this.response);
  };
  xhr.send(json);
}

post('{"abc": 123}')
```
