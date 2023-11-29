## **non-capture group: (?:pattern)**

```javascript
    // match 'ip:10.103.129.171,10.103.129.172'
    const regex = /(?<=(ip:|))(?:\d{1,3}\.){3}\d{1,3}(?=(,|))/g;
    const matches = query.match(regex)
    if (matches != null) {
        let i = 0
        while(i < matches.length) {
            console.log(matches[i])
            i++
        }
        return false;
    }
```

## ** match prefix but exclude: (?<=pattern) **

## ** match suffix buy exclude: (?=pattern) **
