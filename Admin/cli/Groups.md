# Groups

Groups are okerr word for 'plan' or user limits (e.g. number of indicators, number of projects, minimal period).

```shell
# list all groups
./manage.py group --list

# Make user Admin
./manage.py group --assign Admin --user test@test.com --infinite

# Assign user usual group 
./manage.py group --assign Bellatrix --user test@test.com --days 31
```