## [Section of simple command](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_09_01)
* [Tilde Expansion ~p4ali](https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html)
* [parameter expansion ${parameter:-word}](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)
  * ` x=abcDefg;  echo ${x^^@(a|b)}`
* [command substitution $(ls)](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html)
* [arithmetic expansion $((++x + --z))](https://www.gnu.org/software/bash/manual/html_node/Arithmetic-Expansion.html)
* [redirection](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_07), [Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)
* [Quotes](https://mywiki.wooledge.org/Quotes), [quote removal](https://www.gnu.org/software/bash/manual/html_node/Quote-Removal.html)


## [Grammar rules](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_10_02)

## [echo -e 'bla' vs echo $'...' vs cat heredoc](https://askubuntu.com/questions/537984/difference-between-echo-e-string-and-echo-string)

## Convert a `curl` command to `nc`

**curl**
```
$ curl -XPOST  -HContent-type:application/json -Hwm_svc.name:CE-ECHO-SERVICE -Hwm_svc.env:dev -Hwm_consumer.id:39e098e7-4049-495d-ac3d-36e98e0011b3 http://ce-echo-service-dev.wmt/graphql -v -d '{"query": "query{echo{message(message: \"it works\")}}"}' -v

> POST /graphql HTTP/1.1
> Host: ce-echo-service-dev.wmt
> User-Agent: curl/7.66.0
> Accept: */*
> Content-type:application/json
> wm_svc.name:CE-ECHO-SERVICE
> wm_svc.env:dev
> wm_consumer.id:39e098e7-4049-495d-ac3d-36e98e0011b3
> Content-Length: 56

* upload completely sent off: 56 out of 56 bytes
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< access-control-allow-origin: *
< content-type: application/json
< content-length: 41
< date: Tue, 07 Jul 2020 02:33:54 GMT
< x-envoy-upstream-service-time: 6
< server: envoy
<
{"data":{"echo":{"message":"it works"}}}
```
**nc**
```
$ echo -e 'POST /graphql HTTP/1.1\nHost: ce-echo-service-dev.wmt\nUser-Agent: curl/7.66.0\nAccept: */*\nContent-type:application/json\nwm_svc.name:CE-ECHO-SERVICE\nwm_svc.env:dev\nwm_consumer.id:39e098e7-4049-495d-ac3d-36e98e0011b3\nContent-Length: 56\n\n{"query": "query{echo{message(message: \"it works\")}}"}' |nc -i1 -v ce-echo-service-dev.wmt 80
ce-echo-service-dev.wmt (10.125.156.194:80) open
HTTP/1.1 200 OK
access-control-allow-origin: *
content-type: application/json
content-length: 41
date: Tue, 07 Jul 2020 02:39:05 GMT
x-envoy-upstream-service-time: 2
server: envoy

{"data":{"echo":{"message":"it works"}}}
```
**cat**
```
cat <<EOF | nc -i1 -v ce-echo-service-dev.wmt 80
POST /graphql HTTP/1.1
Host: ce-echo-service-dev.wmt
User-Agent: curl/7.66.0
Accept: */*
Content-type:application/json
wm_svc.name:CE-ECHO-SERVICE
wm_svc.env:dev
wm_consumer.id:39e098e7-4049-495d-ac3d-36e98e0011b3
Content-Length: 56

{"query": "query{echo{message(message: \"it works\")}}"}
EOF
```
