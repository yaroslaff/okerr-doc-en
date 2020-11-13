### HTTP SHA1 hash dynamic

Fetches HTTP page from **url** and alerts if it's SHA1 hash different from value set in **hash** option.

**hash** is updated to current value

Option **options** (usually empty) can have values 'ssl_noverify' and 'addr=a.b.c.d'. 
If ssl_noverify specified, check will be successful even if SSL certificate verification fails.
If addr is specified, request will be to IPv4 address a.b.c.d.
