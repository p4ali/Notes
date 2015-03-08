## Shell patten filtering

These POSIX shells use four different pattern filtering:
* ${var#pattern} - Removes smallest string from the left side that matches the pattern.
* ${var##pattern} - Removes the largest string from the left side that matches the pattern.
* ${var%pattern} - Removes the smallest string from the right side that matches the pattern.
* ${var%%pattern} - Removes the largest string from the right side that matches the pattern.

Here are a few examples:
```bash
foo="foo-bar-foobar"
echo ${foo#*-}   # echoes 'bar-foobar'  (Removes 'foo-' because that matches '*-')
echo ${foo##*-}  # echoes 'foobar' (Removes 'foo-bar-')
echo ${foo%-*}   # echoes 'foo-bar'
echo ${foo%%-*}  # echoes 'foo'

# if you have a folder has file 1.dot 2.dot and you wan to mv them to 1.doc and 2.doc
for i in $(ls); do mv $i ${i%%[a-z\.]*}.doc; done
```

## Refs
* [www.unixwiz.net](http://www.unixwiz.net/techtips)
  * [An Illustrated Guide to SSH Agent Forwarding](http://www.unixwiz.net/techtips/ssh-agent-forwarding.html) 
    * [Using ssh-agent forwarding on github](https://developer.github.com/guides/using-ssh-agent-forwarding/)
