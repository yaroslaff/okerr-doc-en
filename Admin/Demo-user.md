# Okerr demo user
Okerr server uses demo user `okerrdemo@maildrop.cc`. Project id `okerrdemo`.

configration, `conf.d/demo.conf`:
~~~
__IF_HOSTNAME='golf'
IMPORT_PROFILE='okerrdemo@maildrop.cc'
~~~

## save current demo profile
`wget -O demo/okerrdemo@maildrop.cc https://golf.okerr.com/api/admin/export/okerrdemo@maildrop.cc`


## Manual import
`./manage.py impex --import --file demo/okerrdemo@maildrop.cc --overwrite`

