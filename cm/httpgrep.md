### HTTP grep
Loads page specified by *URL* option. If fails (not HTTP code 200 OK ) - sets ERR status. 
If page content do not have text specified in **musthave** option - sets ERR status.
If page content has text specified in **mustnothave** option - sets ERR status.

Otherwise sets OK status.

Option **options** (usually empty) can have values 'ssl_noverify' and 'addr=a.b.c.d'. 
If ssl_noverify specified, check will be successful even if SSL certificate verification fails.
If addr is specified, request will be to IPv4 address a.b.c.d.

