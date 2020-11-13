# Third party software cheatsheet

This is our internal cheatsheet which we use for our purposes on our servers. 

**WARNING**: It may be not compatible for your installations. 

## Dehydrated

~~~shell
apt install dehydrated
pip3 install dns-lexicon 
pip uninstall pyOpenSSL
/usr/bin/dehydrated --register --accept-terms
echo "example.okerr.com cat.okerr.com" > /etc/dehydrated/domains.txt
./dehydrated-renew.sh
~~~

if `AttributeError: module 'lib' has no attribute 'X509_up_ref'` happens:
~~~shell
sudo python3 -m easy_install --upgrade pyOpenSSL
~~~


## RabbitMQ

```shell
sudo rabbitmqctl stop_app
sudo rabbitmqctl reset
sudo rabbitmqctl start_app

rabbitmqctl add_vhost okerr
rabbitmqctl list_vhosts
rabbitmqctl add_user okerr 'OkerrSecretPassword'
rabbitmqctl set_permissions -p okerr okerr ".*" ".*" ".*"

# delete queues
rabbitmqctl -p okerr delete_queue MyQueueName
for qname in `rabbitmqctl -qp okerr list_queues |cut -f 1 `; do echo delete $qname; rabbitmqctl -p okerr delete_queue $qname;  done
```
