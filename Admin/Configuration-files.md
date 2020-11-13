# General okerr configuration scheme
Almost all okerr programs which works with database (Okerr UI, okerr-process, okerr-smtpd, okerr-telebot, okerr-mqsender but not okerr-poster and okerr-sensor ) uses this configuration scheme. All configuration files are python modules (if not specified other).

Main configuration file is `okerr/settings.py` in okerr sources (all other configuration files are imported from this file).

Then `okerr/settings_defaults.py` imported to provide default values. 

Important values from there:
- `MAIN_CONF_DIR='/etc/okerr'`, 
- `SITE_PRECONF_FILES = MAIN_CONF_DIR/okerr.conf`,
- `SITE_CONF_DIRS` - subdirectories of `MAIN_CONF_DIR` ending with '.d' (alphabetically sorted). Including symlinks to directories.
- `SITE_POSTCONF_FILES = MAIN_CONF_DIR/post.conf`
- `SITE_JSON_DIRS = '/etc/okerr/json/'` (files here are in JSON format)

Files listed above should not be edited by user. 
Then imported:
- preconf file(s)
- all '*.conf' files from SITE_CONF_DIRS
- postconf file(s) 

Any value could be overriden in  these files.

JKEYS_TPL imported from `SITE_JSON_DIRS/keys-template.json`.

## Conditional import
It's possible to include config files conditionally. When importing files, okerr checks variables `__IF_VARIABLE = VALUE`. If it least one such check failed, file not imported. 

Example:
/etc/okerr/okerr.conf:
~~~
CLUSTER_NAME = 'MY'
~~~

/etc/okerr/local.d/local.conf:
~~~
__IF_CLUSTER_NAME = 'LOCAL'
~~~

/etc/okerr/local.d/my.conf:
~~~
__IF_CLUSTER_NAME = 'MY'
~~~

In this example local.conf will not be imported, but my.conf will be imported.
 

## Debugging importing
Variable `IMPORT_VERBOSITY` (default: 0) initialized in `settings.conf` defines how verbose importing procedure should be. New value for this variable can be set in `MAIN_CONF_DIR/okerr.conf`. Files `settings.py`, `settings_default.py` and `MAIN_CONF_DIR/okerr.conf` imported with IMPORT_VERBOSITY=0. But other config files will be imported with new value for verbosity.

With IMPORT_VERBOSITY=0, all symbols from files are imported silently.

If IMPORT_VERBOSITY>=1, filenames of imported files, and filenames of rejected files will be printed. Example:
~~~
Load /etc/okerr/conf.d/admin.conf
Load /etc/okerr/conf.d/cluster_first.conf
Skip loading /etc/okerr/conf.d/cluster_local.conf ( __IF_CLUSTER_NAME = 'LOCAL' != 'FIRST')
Skip loading /etc/okerr/conf.d/cluster_test.conf ( __IF_CLUSTER_NAME = 'TEST' != 'FIRST')
Load /etc/okerr/conf.d/demo.conf
~~~

If IMPORT_VERBOSITY>=2, it will also report each imported symbol:
~~~
Load /etc/okerr/conf.d/admin.conf
    ADMINS
    ADMINUSER
    LOGMAIL
    USUALUSER
Load /etc/okerr/conf.d/cluster_first.conf
    MACHINES
    SYNC_MAP
    UP_MAP
...
~~~

If IMPORT_VERBOSITY>=3, it will also report values for each symbol (sometimes this could be lot of text):
~~~
Load /etc/okerr/local.d/local.conf
    ALLOWED_HOSTS = ['localhost', '127.0.0.1', 'localhost.okerr.com', 'dev.okerr.com']
    MYIP = '1.2.3.4'
    SITEURL = 'http://localhost/'
~~~




