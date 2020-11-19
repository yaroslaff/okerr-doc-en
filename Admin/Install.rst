############
Installation
############


Real machine or virtual?
========================

Okerr is rather big and complex project: web application (apache/uwsgi), database, mailserver, message broker, few okerr systemd daemons. If you run any other software on server, you will need to be accurate to integrate it. So, we **strongly recommend** to use dedicated (real or virtual) server for okerr. At least when trying to use it for first time.

`okerr-install.py` is recommended way to install and it will be described here. There is checklist for [manual install](Manual install) but it's not recommended unless you're very familiar with okerr.


Step 1 Prepare LXC container
============================

.. code-block:: shell

    # host
    lxc-create -n okerr -t download -- --dist=debian --release=buster --arch=amd64


Step 2a (recommended)
========================

.. code-block:: shell

    # host: start and attach
    lxc-start -n okerr
    lxc-attach -n okerr

    # VM
    apt install git python3
    git clone https://gitlab.com/yaroslaff/okerr-dev.git /opt/okerr

Step 2b (alternative way: okerr sources are on host machine)
============================================================

You may use this method (2b) instead of recommended (2a) but it's harder and not recommended.

.. code-block:: shell

    # host machine
    mkdir /var/lib/lxc/okerr/rootfs/opt/okerr

Edit `/var/lib/lxc/okerr/config`, add mount option to config (replace path to okerr-dev repo): 

.. code-block:: shell

    lxc.mount.entry = /home/USERNAME/repo/okerr-dev /var/lib/lxc/okerr/rootfs/opt/okerr none bind 0 0

Continue preparation:

.. code-block:: shell

    # start and attach
    lxc-start -n okerr
    lxc-attach -n okerr

    # VM
    mkdir /opt/okerr
    apt install git python3


Step 3: Install
================

Replace email in command and run:

.. code-block:: shell

    # inside VM
    cd /opt/okerr

    # All default values. user: okerr@example.com password: okerr_default_password
    ./okerr-install.py --local --email USER@EMAIL.COM

    # Or explicitly
    ./okerr-install.py --fix --apache --rmq --email USER@EMAIL.COM --pass MyPass

Now, wait a little, it will install lot of debian packages, python modules, configures everything, on my notebook this takes 11 minutes.

Step 4: Post-install configuration
====================================
By default, okerr configured to use host dev.okerr.com, you can set it in /etc/hosts (pointing to IP address of VM) e.g.:

.. code-block:: none

    192.168.122.219 dev.okerr.com

or set/add other hostname in `/etc/okerr/local.d/local.conf` and `/etc/apache2/sites-available/okerr.conf` .

Make sure you can send mail from this host. If needed - reconfigure postfix for this (by default it uses hostname 'okerr'). 

.. code-block:: none

    myhostname = okerr  #replace to your valid hostname

    # inet_protocols = all
    inet_protocols = ipv4


Also, you may want to set settings FROM and SERVER_EMAIL in local config (`/etc/okerr/okerr.conf`). Defaults:

.. code-block:: none

    SERVER_EMAIL = 'noreply@okerr.com'
    FROM = '"okerr robot" <noreply@okerr.com>'



Use
===
Log in to http://dev.okerr.com/ it's fully working.

Enable okerr SMTP server (optional)
===================================

make sure okerr-smtpd service is enabled and running
----------------------------------------------------
.. code-block:: shell

    systemctl enable okerr-smtpd
    systemctl start okerr-smtpd
    systemctl status okerr-smtpd


Configure postfix /etc/postfix/transport: 

.. code-block:: none

    .okerr.com    smtp:localhost:10025

then run: ``postmap /etc/postfix/transport``

in /etc/postfix/main.cf:
.. code-block:: none

    relay_domains = $mydestination, update.okerr.com, dev.okerr.com, localhost.okerr.com
    transport_maps = hash:/etc/postfix/transport
    smtp_host_lookup = dns, native 
    # disable ipv6
    inet_protocols = ipv4

Hostname (e.g. alpha.okerr.com or dev.okerr.com) should not be mentioned in: myhostname and mydestination
but should be in relay_domains (mail to these hosts should be relayed, not local).


format of text email
---------------------
.. code-block:: none

    %%% MyIndicator.status = OK
    %%% MyIndicator.details = %HOST% SMTP test
    %%% MyIndicator.secret = mySECRET

how to send test email
-----------------------
.. code-block:: shell

    cat msg | msmtp -f aaa@bbb.com --host=localhost --port=10025 admin@okerr.com
