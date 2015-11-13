# Setup Git-fusionEdit
## Prerequisite
* perforce:1666//.git-fusion maintain the user authentication key
* user account in perforce:1666
* git on client machine
* predefined client ali_p4eclipse

##Edit
To clone ali_p4eclipse as a git repo
* generate a keypair, and upload the id_rsa.pub to //.git-fusion/users/ali/keys/id_rsa.pub
``` 
git clone perforce@gf.perforce.com:ali_p4eclipse // you must use perforce
```
you will get ali_p4eclipse in your current folder, and ali_p4eclipse/.git is also created
