# Introduction: flowchart and definitions

## Flowchart
![okerr diagram](/_static/okerr-diagram.png)

## Definitions
***Indicator*** - Main concept of okerr. Indicator may be in one of two states, either *OK* or *ERR*. When state is changed, *alert* is sent. Also, indicator has number of related properties (such as time of last check, time of last status change) and parameters for *check method*.


***Client*** - optional okerr client-side software, working on linux servers of end-users and sending *updates* for *indicators* to server. Clients are ether okerrmod/okerrupdate utilities from [okerrupdate](https://github.com/yaroslaff/okerrupdate/) or (much less often) [okerrclient](https://gitlab.com/yaroslaff/okerrclient/).

Also, user can use it's own in-house software as client or even `curl` utility.

***Check method*** - technical way which defines state of indicator. For every check method there are it's own set of parameters. For example, check method 'Ping' has parameter 'host'. Indicator with this checkmethod will be 'OK'
if ping is successful (and 'ERR' otherwise).

***Update*** - (indicator update). Internal technical message about indicator state which client sends to okerr server.

***Alert*** - Message about indicator status change, which okerr server sends to user over email or Telegram. Alerts may be suppressed sometimes (for example, when *indicator* has flag *silent*)

***Policy*** - Set of indicator properties which could be applied to many indicators at once. For example, all indicators which uses policy 'Hourly' are checked once per hour.

***Project*** - Set of indicators, policies and users working together. One user may have many projects and many users may work on one project.

***Sensor*** - external part of okerr sensor which makes network checks (ping, http status, antispam, ...)



