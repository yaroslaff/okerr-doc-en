# Failover

Okerr makes possible to implement failover feature with backup server(s), okerr monitoring and dynamic DNS service.

Failover schemes are created/configured in "Failover (Dynamic DNS)" section in project settings.

## Description
Each failover scheme in okerr consist of dynamic DNS name and set of indicators. Each indicator in scheme corresponds to one of project servers (indicator represents state of this server) and has priority corresponding to server priority.

When indicators changes, okerr selects highest priority indicator which has state 'OK' and not in maintenance, and sets DNS record pointing to this working server.

Okerr supports [Hurricane Electric](https://dns.he.net/) and [Cloudflare](https://cloudflare.com) dynamic DNS services. Both these services are free. Also, for tests and traning use may use okerr test account on Cloudflare (so you dont have to register there).


## Example failover cat.okerr.com
![failover](/_static/failover.png)

Thanks to failover technology, we run [World most reliable kitten website](https://cat.okerr.com/).

At screenshot above you may see, scheme consist of theee indicators: `charlie:cat`, `echo:cat` и `bravo:cat`. 
Main server is `charlie` (116.202.27.149), and state of this server is watched by indicator `charlie:cat` (this indicator has highest priotiry).

At screenshot moment, two servers with higher priority (charlie and echo) are in ERR state. Thats why okerr activated last backup server (bravo). It's IP (37.59.102.26) set as dynamic DNS record value for cat.he.okerr.com (This record always points to highest priority working server).

Right side of page shows how DNS changes are propagated, namely:

- Current value for DNS record (in okerr) and it's status
- DNS record queried from each of authoritative nameserver
- DNS record queried from world most popular DNS services
- Last reply from dynamic DNS service

To add new backup server, it' enough just to fill form shown at bottom of screenshot: select indicator, IPv4 address and priority.

Host record cat.okerr.com is actually static, it's CNAME to dynamic DNS record cat.he.okerr.com (which is managed by okerr), so it's always pointing to working server. You may use similar approach if your DNS server are not supporting dynamic DNS.

## How cat.okerr.com simulates outages
Project cat.okerr.com designed to demonstrate how good is our failover scheme, not to show how stable are our servers. So, we simulate outages for failover to work. Each indicator (`charlie:cat` ...) uses check method `HTTP grep` with parameters `URL: https://cat.okerr.com/`, `musthave: status=OK` and `options: addr=charlie.okerr.com`. 

Indicator is OK only if okerr could find text "status=OK" on URL page. If no such text, or if okerr could not fetch page (e.g. webserver is down) - indicator will switch to ERR.  We use rare option `addr=...` to check webpage on server charlie, even if cat.okerr.com DNS record currently points to other server.

Our cat servers work specially way. Main server starts to simulate problem (shows "status=ERR" on page) on HH:20 (20 minutes of every hour) and okerr will reconfigure DNS record to point to backup server. Second ('backup') server simulates problem on HH:40. At this moment, only last ('sorry') server is up. At HH:00 all servers are 'recovered' and okerr point DNS record to main server.