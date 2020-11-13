# Policy

*Policy* is set of indicator properties which is applied to all indicator which uses this policy. Each indicator uses one policy, but one policy can be used by many indicators. 

## Policy properties

**Policy** (name) - you can name policy as you want, but it's recommended to name custom policies according to purpose (e.g. 'SSL certificates' or 'virtal servers'), but not parameters ('1 hour'). 

**Period** - main purpose of policy. Time period when indicator should be re-checked. Use int value (seconds) or with suffux (e.g 1h, 30min).

**Patience** - (Passive indicators only) Time after period when okerr server still expects update for indicator, before switching it to ERR stage. If period is 1h and patience is 20min, indicator will switch to ERR after 1h 20min.

**Secret** - Secret (password). Required to update indicator. You can use different secrets to different policies. This allows to use okerr client software on untrusted machines without revealing secret used on other machines.

**Retry schedule** - (Active indicators only) Set of delays (possible empty) for retries when indicator check fails. For example, if retry schedule is "20sec 5min" and check failed, it will be retried again after 20 sec. If failed again, it will be retried after 5min. And only if that check will fail too, indicator will switch to ERR state.

**Recovery retry schedule** - (Active indicators only) same as Retry schedule, but for indicators in ERR state switching to OK state.

**Alert reduction** - used to prevent 'flapping' when indicator switches state frequently, and alerts are not expected by user. For instance, you may have cron task which stops/locks database at night to make backup. You do not want to have one alert for 'DB is down' and one for 'DB is up' every night.

Format is: `<default reduction time> (<time specification> <reduction time>)*`
Example value: `0s 00:30-02:00 5m 14:00-14:30 30m`

This means default reduction time is 0s (no reduction, alerts are sent immediately). 00:30-02:00 reduction time is 5 min. and 14:00-14:30 reduction time is 30 minutes.

If indicator will switch to ERR at 01:00, alert will be generated but delayed for 5 min until 01:05. If indicator will recover (ERR -> OK) at 01:04, both alerts will be reducted (deleted), no alerts will be send. You will see in log of indicator that it was ERR, but no email or telegram alerts. But if not recovered until 01:05, first alert will be released at 01:05 and you will receive it.

**URL to report status changes** - URL where okerr will send HTTP POST requests with JSON data when indicator changes status.

Example client-side script:
```php
<?php
$filename = '/tmp/okerr-post.log';
$fh = fopen($filename, 'a');
fwrite($fh, date("Y/m/d H:i:s") . " ");
fwrite($fh, $_POST['data']);
fwrite($fh,"\n");
fclose($fh);
?>
```

Example lines from this logger:
```
2020/02/19 10:02:25 {"type": "status_change", "textid": "okrrdm", "name": "echo:empty", "old": "ERR", "new": "OK", "details": "0 files checked", "unixtime": 1582106544, "time": "2020-02-19 10:02:24 UTC (+0000)"}
2020/02/19 10:04:10 {"type": "status_change", "textid": "okrrdm", "name": "charlie:maxfilesz", "old": "ERR", "new": "OK", "details": "/var/log/btmp 61.9M", "unixtime": 1582106649, "time": "2020-02-19 10:04:09 UTC (+0000)"}
```

**Autocreate** - if checked, new indicators will be created if missing (if update specifies this policy and secret matches.

**Accept updates over HTTP** - if checked, HTTP updates (usual updates from okerrupdate, okerrmod, okerrclient) are allowed. Otherwise it's ignored.

**Accept updates over SMTP** - if checked SMTP updates are allowed.

### Allowed client subnets
Each policy has list of subnets in form a.b.c.d/mask (e.g. 1.2.3.0/24) from where okerr will accept updates. 0.0.0.0/0 matches any IPv4 address.

### Default policy
Policy with name 'Default' is special.
- You cannot delete Default policy
- If policy not specified (e.g. when sending update for non-existent indicator), Default policy is used.
