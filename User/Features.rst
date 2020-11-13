
Features of Okerr
##################

.. rubric:: Easy to try

Not required to install okerr server, you can use our [okerr server](https://okerr.com/). Even no need to register, you can use demo account.

If you want to perform *optional* local checks (from inside server, e.g. check free disk space) you need to install one small python package.

.. rubric:: Service
You can install full software on your server for your project. But also you can install software and provide service to your customers.

.. rubric:: Team work
It's possible to work on project alone or with other team members.

.. rubric:: Network checks
- SSL certificate expiration (for websites, mail servers etc.)
- Ping (check if host is online)
- TCP Port (check if TCP port is listening)
- HTTP(s) page status code (e.g. must be `200 (OK)` not `404 (Not found)` or `500 (Internal Server Error)`)
- HTTP(s) page text (e.g. must have some text, must not have text 'PHP Error')
- HTTP(s) page content (sha1 checksum). Warns if page content changed.  
- DNS name
- WHOIS domain name expiration
- Antispam checks in 50+ DNS Black Lists

It's possible to specify particular sensor to perform check (e.g. from Paris, France). In case of problem, re-check will be done from other sensor.

.. rubric:: Local checks

For local checks okerr server provides these types of indicators:

**Heartbeat** 
Simplest check, just OK or ERR. Will automatically switch to ERR after specified time unless OK status will be confirmed. Heartbeats are useful for indicator where you can implement all logic on client (check, make decision, send OK or ERR).

**Numerical**
Will alert if number from local server is 'not good' (over maximal/minimal limits, or difference is too high).  

**String**
Alerts if string from local server does not matches old string.

These three types allows to make many different checks, performed by `okerrmod` utility from [okerrupdate](https://gitlab.com/yaroslaff/okerrupdate) package.

Among other, local checks will warn if:

- Backup files are outdated
- TCP port is open or closed (e.g. application crashed)
- Too low free disk space
- Server is heavily loaded (high load average)
- Server rebooted
- Some directory size (e.g. customer website content) is too high
- Any application error log is not empty
- Log files are growing fast (e.g. mailserver is hacked and used to send spam or some problem exists and error messages are logged every second)
- Any SQL database anomaly, from db server is down and to "usually we have about 100 purchases every hour, and now just 10, probably something goes wrong".

`Full list of built-in local check modules <https://gitlab.com/yaroslaff/okerrupdate/-/wikis/basic%20okerrmod%20modules>`_. 

.. rubric:: Logical (Lamdba expressions)
Logical indicators allows to make complex logical conditions based on basic indicators. For example:
- Server X may go down for maintenance from 3:00 to 5:00 of night
- We may have up to 2 servers down, if we have at least 3 servers up.

.. rubric:: Telegram 
Okerr uses telegram bot for mobile interface. You can instantly check status of your projects and re-test failed indicators from any location. 

.. rubric:: Free
Okerr server is free and open source software distributed with [AGPL](https://www.gnu.org/licenses/agpl-3.0.en.html) license. Client-side modules uses even more free [MIT](https://opensource.org/licenses/MIT) license.

.. rubric:: Low resources
Client-side python module is less then 50Kb in size (requires python3 and few other requirements, most of it is already installed on typical server).

Resident size (in RAM) is *ZERO*, just runs as cron job every 20 minutes.

.. rubric:: Open source, open API
Everything (client, libraries, server) are open-source. Interactions are based on standard protocols, so you can integrate your software with okerr, write your own okerr modules and clients. You can even update indicators right from shell with `curl`.

.. rubric:: Easy to integrate
If your software is in python or shell, you can use [okerrupdate](https://gitlab.com/yaroslaff/okerrupdate) library. You can use your favorite HTTP client library for any other programming language.

.. rubric:: Easy to extend
Client-side checks uses very simple interface and can be implemented in any programming languages.
