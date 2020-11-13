
# Alerts and False Positives
Main okerr idea is focused in alerts. You work in okerr to configure it and forget about it. And then, some day you will get alert which will save you. It will warn you about expected problem (certificate will expire soon), or anomaly (log size grows too fast / purchases in online shop are unusually low, need to check) or problem (website page shows "PHP Error").

Each alert from okerr (in perfect) requires you to make some actions (check things or fix problem). If you do not want react to alert - it's not needed for you. These alerts are called 'False positives' and should not happen. If okerr will alert often with false positives (shouting 'Wolves! Wolves!') some day you will not read real alert.

Okerr has different ways to keep number of false alerts very low or even zero. Check all of them, and decide which one is better for each indicator with false positives.

## Methods to reduce false positives

### Fix issue behind indicator
**Example**: You have alert for disk usage over 80%. And real disk usage is between 70-82%. Sometimes when you do something heavy on server (dump mysql databases) disk usage goes over 80% and you have alert.

Consider buying larger disk or clean up this disk. If this is possible - this could be best solution.


### Adjusting limits
Change indicator limit from 80% to 85% or even 90%. This will solve issue with alerts, but will make your risks higher.


### Subscribe only to your indicators
If you are not the only person who work in project, you may be interested only in part of indicators. In profile you may uncheck 'send alerts' checkbox (if checked it enables alerts from all indicators), but on indicator page enable checkbox 'subscribe' in User-specific settings. You will not get alerts from all indicators, but will get alerts only from indicators you're subscribed.

### Maintenance 
If you're working on indicator or on technical issue behind indicator (e.g. free disk space or high load average), during your work indicator may change status few times sending false-positive alerts to other team members. (They do not need it, you already fixing issue). Right way is to set indicator to 'maintenance' stage. To set it, click on '...' button on indicator page to open additional buttons and click 'Set maintenance'.

In maintenance mode alerts aren't sent (until youl will stop maintenance). Also, other team members will see that indicator is on maintenance, will see who set it and when. They will know you're working on it.

### Silent
Silent indicators do not send alerts. If you will check this checkbox - indicator will not send neither false positives nor real alerts. Use it with caution.

### Location-specific checks
For active indicators (e.g. sslcert, http status, ping) you may specify Country/City/Sensor name . If tests are not reliable from France, you may decide to use tests from Germany for example.

### Retry schedule
Active tests sometimes may fail because of short-time network problems. This is very rarely, but if you have hundreds of indicators - at least one of them may fail every few days. Instead of manually rechecking each alert, why not to make okerr do this?

Configure retry schedule like this: "30s 5min". Now, if check will fail, it will be repeated in 30 seconds. If it will fail again - it will be repeated in 5 minutes. And only if all retry schedule passed and failed - indicator will switch to ERR and send alerts. If you dont want to be alerted about short-time problems you will not.

Each next retry performed by other sensor (if possible), not the one which failed previous check. 

### Logical indicators
This is very powerful feature of okerr. Logical indicator has state defined by logical expression based on current project (indicators) status. You may set some usual indicators silent (they are called bottom-level indicators) and create logical (upper-level indicator) to watch them.

**Example 1**: Suppress short-time errors ('flapping'). Server Load Average is very low usually, you have limit to 1.0, but sometimes (e.g. when you compile large software projects) it may go over 5. But then, in 20 minutes it returns to normal value, no need to 'repair' anything. You may silent this indicator, it will switch to ERR when you have high LA, but will not send alerts. Then you configure logical indicator to send alert if bottom-level indicator has 'ERR' stage for longer then 20 minutes. Now, if high LA stays for short time - you dont get false positive, but if it stays for long time - you get alert and inspect situation.

**Example 2**: Scheduled problem. You have few servers and sysadmins sometimes need to do something with it, sometimes shutting off services or whole servers. You have designed two 'safe hours' from 03:00 to 05:00, this is time when downtime is less painful for business and sysadmins can do anything on that time. And you dont want to get alerts if server is down between 03:00-05:00. Now, you can make that indicators silent, and configure one logical indicators for all of them, it will alert only if at least one indicator failed and current time not in safe range.

**Example 3**: N or M. You have M identical webservers, they are sharing load. Since each of them can replace other and downtime of one-two servers are not problem, you can create logical indicator which will alert only if less then N servers are up. 5 servers total, and 3 servers up - OK, you don't have alert. 2 servers up - too low, you get alert.

### Alert reduction
Thats other nice and powerful feature of okerr. It's effective against examples 1 and 2 above, but often easier. You configure policy with alert reduction like: "0s 00:30-02:00 5m 14:00-14:30 30m"

This means default reduction time is 0s (no reduction, alerts are sent immediately). 00:30-02:00 reduction time is 5 min. and 14:00-14:30 reduction time is 30 minutes.

If indicator will switch to ERR at 01:00, alert will be generated but delayed for 5 min until 01:05. If opposite alert will be generated (ERR -> OK) at 01:04, both alerts will be reducted (deleted), no alerts will be send. You will see in log of indicator that it was ERR, but no email or telegram alerts. But if no opposite alert until 01:05, first alert will be released at 01:05 and you will receive it.

You may use short reduction period almost for every indicator all the time, and use very long reduction period for safe time, when downtime is expected.

### Just keep thigs as it is
If you get first false positive from indicator, maybe you should not fix it. Maybe it's good alert? And if not good, maybe it will happen once in year? If yes - forget about it.  For example, 17-18 July 2020 there were [large outage across whole Internet, because of network failure at CloudFlare](https://techcrunch.com/2020/07/17/cloudflare-dns-goes-down-taking-a-large-piece-of-the-internet-with-it/) (which provides very popular public DNS server 1.1.1.1). Such kind of problems happens quite rarely, so it's not a problem to get alert about this. Actually, when problem noticed - nobody knows where problem is, fill it be fixed 'itself', and how long outage will last, so getting alert is very important.


On other hand, if you have okerr alert in inbox and did not read it, or read it quickly, not thinking about it - probably this is false positive and you should find a solution.
