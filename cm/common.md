# Common properties

Different checkmethods sometimes has common properties

## Options

**options** parameter (usually empty) used in HTTP checks and may contain `ssl_noverify` and `addr=a.b.c.d`. 

If `ssl_noverify` option present - check will be successful even if cert is not valid (expired or self-signed)

If `addr` (addr=a.b.c.d) option present, HTTP request will be performed to host a.b.c.d, no matter what host is specified in `URL`.

## Patience и Secret

Passive indicators has parameters `Patience` and `Secret` which (if set) overrides corresponding values from [Policy](../User/Policies).