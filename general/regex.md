## `(?<=xyz).*`
[positive lookbehind](http://www.regular-expressions.info/lookaround.html), which match anything following `xyz` (exclusive `xyz`). This is useful.
```bash
$ echo "Email: ali@perforce.com" | grep -Po "(?<=Email: ).*"
# return ali@perforce.com
```

## Reference
* [Five Invaluable Techniques to Improve Regex Performance](https://www.loggly.com/blog/five-invaluable-techniques-to-improve-regex-performance/)
* [Regex tutorial](http://www.rexegg.com/regex-quickstart.html)
