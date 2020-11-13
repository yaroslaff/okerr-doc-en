### Heartbeat
Simplest type of passive indicator. Indicator has status sent from client machine. (If client sets OK (which means, e.g. 'apache2 on server X is OK'), indicator will be OK), if client sets ERR, indicator will be ERR). 

If client did not send new update in specified time (policy.period + patience), heartbeat indicator expires and automatically switches to 'ERR'.

**patience** (in seconds) and **secret** (if set) overrides patience and secret from policy.
