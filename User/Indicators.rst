Indicators
----------

Indicator properties
====================

.. rubric:: Name

Each indicator belongs to one project, must have unique name in project and can by addressed by full name of indicator is *name@textid* where *textid* is any of project textid. 

.. rubric:: Status

Status is either "OK" or "ERR" (Yes, that's why it's OKERR). When status of indicator changes, alert is sent to email/telegram.

.. rubric:: Details

Details is more detailed info about status. For example, free disk space indicator may have status "OK", current value 59.3% and details: "59.30%, 5.8G/9.8G used, 3.5G free"

.. rubric:: Check method

Most important property of indicator is "check method". All available methods are described in [Check methods](en/Check methods). Indicator status is updated according to check method.

.. rubric:: Changed

Date/time when indicator changed status last time.

.. rubric:: Updated

Date/time when indicator was checked and status updated (even if not changed).

.. rubric:: Expected

Date/time when okerr server expects to receives update (when it will be re-checked)

.. rubric:: Scheduled

Date/time when okerr server schedules some action if indicator will not be updated in time. For active indicator - server will re-send task to sensor. For passive - switch status to ERR.

.. rubric:: Disabled

If indicator is disabled, it will not work any more (until enabled again). Updates will be discarded. Status will never change and alerts will not be send. 

.. rubric:: Problem

Rarely used (only in logical indicators). Problem flag means something is wrong with indicator configuration and you should reconfigure it and remove flag. Indicator will not be executed until problem flag is removed.

.. rubric:: Silent

Silent indicator sends no alerts. You may want to set silent flag if ERR state is expected for indicator. 

.. rubric:: Maintenance

Maintenance state is very similar to silent (alerts are not send) but displayed differently and designed to be used temporary for work in progress (e.g. when you reconfigure remote service or indicator). Date/time when and who set maintenance mode is recorded.

.. rubric:: Policy

Policy (set of configuration options) applied to this indicator. Each indicator uses exactly one policy. When indicator created, policy 'Default' is used.

.. rubric:: Location suffix

For active indicators specified sensor(s) which will execute it. For example, you can ping server from specific sensor 'echo@paris.fr' or any sensor in 'paris.fr' or any sensor in 'fr'. For passive indicators it's not used.

.. rubric:: Description

Just informational field, comment to indicator, reminder for yourself or other team members.

Indicator user settings
=======================
Indicator user settings are specific for user (if many users are working on project, one user settings does not affects other users).

.. rubric:: Star

If indicator is 'starred' it will be displayed with 'star' icon on project indicators dashboard. This is useful to quickly find important indicators if project has many indicators.

.. rubric:: Subscribe

Even if user has alerts turned off, but subscribed to indicator, he will get alerts from this indicator. This is useful, for example, if project manager do not want to be alerted about any problem, but want to be alerted about important indicators (such as escalation logical indicator).
