
### SSL Certificate check
Performs network SSL Certificate validation. SSL certificate is checked from `host`/`port` 
(which makes possible to check not only HTTPS services, but IMAPS, POP3S and anything else).

Alert if certificate is not valid (e.g. self-signed) or if it's already expired or will expire in less then `days`.

If `addr` specified in `options`, certificate will by downloaded from this address. This is 
sometimes needed if you have more then one server with same DNS name, and want to check each of them.

If `noverify` (or `ssl_noverify`) specified in options, SSL verification will be skipped. This makes
possible to check self-signed certificates.
