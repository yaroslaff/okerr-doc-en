# Update indicators over HTTP(s)

## Update
~~~shell
# get active server for project
$ curl https://bravo.okerr.com/api/director/MyProject
https://bravo.okerr.com/

# update
$ curl -d 'textid=MyProject' -d 'name=MyIndicator' -d 'secret=mysecret' -d 'status=OK' https://bravo.okerr.com/update
OK
~~~

## Reverse engineering
You can 'sniff' HTTP traffic from okerrupdate/okerrmod to see examples of actual HTTP requests, e.g.:
~~~shell
$ okerrupdate -v MyIndicator OK 
got url https://bravo.okerr.com/ from director https://cp.okerr.com/api/director/MyProject
geturl: return https://bravo.okerr.com/
update: MyIndicator@okerr = OK (None) url: https://bravo.okerr.com/update
okerr updated (200 OK) MyIndicator@okerr = OK
Request to URL https://bravo.okerr.com/update:
textid=MyProject&name=MyIndicator&status=OK&secret=MySecret&method=heartbeat&policy=Default&tags=&keypath=&origkeypath=&desc=
b'OK'
took 0.3524200916290283 sec.
~~~