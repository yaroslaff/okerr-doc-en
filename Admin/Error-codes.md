# Okerr HTTP Error codes
Okerr server may report HTTP error if something is wrong. Error is reported with HTTP status code 400 (Bad request) and error message. Error message may start with 'ERROR:*code* *details*' Where code is one of following:

**BAD_METHOD** - Indicator with this name already exist in project and (based on it's CheckMethod) it's not passive (so, update from client is not expected for this type of indicator)

**LIMIT_MAXINDICATORS** - Project already has maximum allowed number of indicators. To see how many indicators allowed, check 'profile' page and look for 'maxindicators' limit there. Ways to fix problem:
- Delete indicators which you do not use
- Disable indicators (they will exists in project, but will not count)
- Switch to higher plan. If you're new user in group 'Space' you can switch to group 'Alcor' after completing training.
