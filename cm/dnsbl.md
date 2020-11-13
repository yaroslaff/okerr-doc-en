### Antispam DNS blocklist
Checks `host` in all (50+) configured DNSBL lists. Sets status ERR if host found in at least one list. You can add DNSBLs to `skip` option (separated by space or comma) to ignore it (it will not be checked). Or you can add extra DNSBL to `extra` list.
