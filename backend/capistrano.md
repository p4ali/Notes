## Deploy and test
```bash
cap vcloud deploy
cap vcstage deploy
cap vcloud sanity[test1]
cap vcloud atlas:restart
cap vcloud atlas:check

ssh-add ~/.ssh/id_rsa ## This enable key forwarding to access pdnet form remote using local authenticaton/ssh agent
```
