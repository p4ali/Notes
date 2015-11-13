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
git clone git@gfprod.perforce.com:ali_hws_mac // you must use git as user name
```
you will get ali_p4eclipse in your current folder, and ali_p4eclipse/.git is also created

## Alternative
```
export GIT_SSL_NO_VERIFY=true
git clone https://gfprod.perforce.com/ali_hws

ali_hws client:
//depot/dev/helix-web-services_helix-cloud/source/... //ali_hws/...
```
