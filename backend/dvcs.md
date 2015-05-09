## Clone a repo from Helix
```
# login
p4 -ume -pssl:p1.me.atlas.dev:1667 login

# clone remote to local
p4 clone -ume -pssl:p1.me.atlas.dev:1667 -f //p1/main/...

# create a submit locally and then push to remote
echo "hello world">a.txt; p4 reconcile; p4 submit -d"add a file"; p4 push
```

## Add a `post-rmt-Push` trigger
The trigger will pass callback `%user%`, `%firstPushedChange%`, `%lastPushedChange%`, and the changes in between are consecutive.
