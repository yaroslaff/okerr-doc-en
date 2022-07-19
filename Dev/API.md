# Using okerr API
## API Key
To use API get API key from project page (record it somewhere, it's displayed just once).

To use key with okerrclient, either write it in `/etc/okerr/okerrupdate` like this:
~~~
OKERR_TEXTID=mytextid
OKERR_API_KEY=MyAPIKey
~~~

or set it in environement:
~~~
export OKERR_API_KEY=MyAPIKey
~~~

or provide it as okerrapi option `--key=MyAPIKey`. Later in examples we will suppose you have configured project name and textid in `/etc/okerr/okerrupdate`.


## Basic functions
### List indicators
~~~
$ okerrapi indicators
$ okerrapi indicators servername:
~~~
If optional argument is given, will list only indicators matching prefix.

### Show indicator info
~~~
okerrapi --name IndicatorName indicator
okerrapi -n IndicatorName indicator
~~~

### List by filter
Show indicators which matches all conditions. Condition is either tag (like `sslcert` or `maintenance`) or option value (like `host=google.com` for sslcert type). Use exclamation mark for logical NOT (use quotes in shell for this purpose)

~~~
$ okerrapi filter host=google.com sslcert '!maintenance'
~~~


### Modify indicator (set any properties)
Can set multiple properties at once
~~~
$ okerrapi -n IndicatorName set location=ru retest=1
Changed IndicatorName {'location': 'ru', 'retest': 1}
~~~

## Useful examples
### Set location for all sslcert indicators and retest
```shell
#!/bin/sh

for indicator in `okerrapi filter sslcert`
do
	echo set location for $indicator
	okerrapi -n $indicator set location=ru retest=1
done
```

## Using API directly over HTTP 
While `okerrapi` (part of [OkerrUpdate](https://github.com/yaroslaff/okerrupdate) is recommended way to start working with okerr API, but you may want to use it directly using `curl` or your own software.

First step is to find proper okerr server which will serve your request:

~~~shell
$ curl https://cp.okerr.com/api/director/mytextid
https://golf.okerr.com/
~~~

Here we ask main Control Panel server about server for project with textid `mytextid` and server points us to server https://golf.okerr.com/. Okerrclient makes this request automatically for any API calls. You may either do this request every time, or store it value and re-use it, projects are moved to other servers rarely (~once per year).

Second step is to perform actual HTTP request to this server. To know request URL, you may use -v option of okerrapi, e.g. okerrapi -v filter sslcert:

~~~shell
$ okerrapi -v filter sslcert
got url https://golf.okerr.com/ from director https://cp.okerr.com/api/director/mytextid
GET request to https://golf.okerr.com/api/filter/mytextid/sslcert
~~~

Now, you can perform this query:
~~~shell
$ export OKERR_API_KEY=LxxUwXRlIBEjwHDn59i6086TXAQ1J2pp06A40auB
$ curl -H "X-API-KEY: $OKERR_API_KEY" 'https://golf.okerr.com/api/filter/mytextid/sslcert'
~~~

Or, you can work without API key, providing user/pass:
~~~shell
$curl --user $OKERR_API_USER:$OKERR_API_PASS 'https://golf.okerr.com/api/filter/mytextid/sslcert'
~~~

Change indicators:
~~~shell
curl -H "X-API-KEY: $OKERR_API_KEY" 'https://golf.okerr.com/api/set/mytextid/IndicatorName' -d 'retest=1&domain=google.com'
Changed IndicatorName {'retest': 1, 'domain': 'google.com'}
~~~
