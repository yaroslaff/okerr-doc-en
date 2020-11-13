# User management

## Create user

###  Create user and assign to group in one command:
```shell
(okerr) okerr@okerr:~$ ./manage.py profile --create test@test.com --pass MyPassword --group Bellatrix --infinite
create user test@test.com
No such user. good!
create it.
created user test@test.com pass MyPassword
assign to group Bellatrix
```
Or you may use `--days <NNN>` instead of `--infinite`. Use `./manage.py group --list` to see all available groups.

### Create user (without adding to groups)
```shell
./manage.py profile --list

./manage.py profile --create test@test.com --pass mypassword
```
And do not forget to add user to [Groups](Groups).



## Delete users
~~~shell
./manage.py profile --delete --user user@example.com --really
~~~