# Using okerr API
## API Key
To use API get API key from project page (record it somewhere, it's displayed just once).

To use key with okerrclient, either write it in `/etc/okerrclient.conf` like this:
~~~
api-key = sFeGeFCJPcD9DgcfS6y9OIxPA4D9JXYcl6rwbJlA
~~~

or set it in environement:
~~~
export OKERR_API_KEY=sFeGeFCJPcD9DgcfS6y9OIxPA4D9JXYcl6rwbJlA
~~~

or provide it as okerrclient option `--api-key=sFeGeFCJPcD9DgcfS6y9OIxPA4D9JXYcl6rwbJlA`. Later in examples we will suppose you have configured project name and textid in `/etc/okerrclient.conf`.


## Basic functions
### List indicators
~~~
$ okerrclient --api-indicators
$ okerrclient --api-indicators servername:
~~~
If optional argument is given, will list only indicators matching prefix.

### Show indicator info
~~~
okerrclient --api-indicator --name IndicatorName
~~~

### List by filter
Show indicators which matches all conditions. Condition is either tag (like `sslcert` or `maintenance`) or option value (like `host=google.com` for sslcert type). Use exclamation mark for logical NOT (use quotes in shell for this purpose)

~~~
$ okerrclient --api-filter host=google.com sslcert '!maintenance'
~~~


### Modify indicator (set any properties)
Can set multiple properties at once
~~~
$ okerrclient --api-set location=ru retest=1 --name IndicatorName
Changed IndicatorName {'location': 'ru', 'retest': 1}
~~~


## Useful examples
### Set location for all sslcert indicators and retest
```shell
#!/bin/sh

for indicator in `okerrclient --api-filter sslcert`
do
	echo set location for $indicator
	okerrclient --api-set location=ru retest=1 --name $indicator
done
```

## Using API directly over HTTP 
[Okerrclient](https://github.com/yaroslaff/okerrclient) is recommended way to start working with okerr API, but you may want to use it directly using `curl` or your own software.

First step is to find proper okerr server which will serve your request:

~~~shell
$ curl https://cp.okerr.com/api/director/mytextid
https://echo.okerr.com/
~~~

Here we ask main Control Panel server about server for project with textid `mytextid` and server points us to server https://echo.okerr.com/. Okerrclient makes this request automatically for any API calls. You may either do this request every time, or store it value and re-use it, projects are moved to other servers rarely (~once per year).

Second step is to perform actual HTTP request to this server. To know request URL, you may use `-v` option of okerrclient, e.g. `okerrclient -v --api-filter whois ERR -i mytextid`:

~~~shell
$ okerrclient -v --api-filter whois '!ERR' -i okerr 
getting project url from https://cp.okerr.com/api/director/okerr
status code: 200
getting filtered indicators from url https://echo.okerr.com/api/filter/okerr/whois/!ERR/
...
~~~

Now, you can perform this query:
~~~shell
$ export OKERR_API_KEY=LxxUwXRlIBEjwHDn59i6086TXAQ1J2pp06A40auB
$ curl -H "X-API-KEY: $OKERR_API_KEY" 'https://echo.okerr.com/api/filter/mytextid/whois/!ERR/'
~~~

Or, you can work without API key, providing user/pass:
~~~shell
$curl --user $OKERR_API_USER:$OKERR_API_PASS 'https://echo.okerr.com/api/filter/okerr/whois/!ERR/'
~~~

Reconfigure indicators (check URLs):
~~~shell
$ okerrclient -v -i mytextid --name test --api-set domain=google.com retest=1
getting project url from https://cp.okerr.com/api/director/mytextid
status code: 200
set options for indicator. url https://echo.okerr.com/api/set/mytextid/test
status code: 200
Changed test {'retest': 1, 'domain': 'google.com'}
~~~

Copy same method without okerrclient:
~~~shell
curl -H "X-API-KEY: $OKERR_API_KEY" 'https://echo.okerr.com/api/set/mytextid/text' -d 'retest=1&domain=google.com'
Changed test {'retest': 1, 'domain': 'google.com'}
~~~
