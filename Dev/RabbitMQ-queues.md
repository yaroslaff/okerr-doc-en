# RabbitMQ queues

## General info 

RabbitMQ queues used to pass check tasks (e.g. check ssl certificate expiration date) from task manager of okerr server to remote processors (network sensors). This serves following purposes:
- Split all checking load among many servers
- Perform location-specific checks (e.g. check if website is available from France)
- Perform duplicate check on other sensor to avoid false positives.

On okerr server side okerr-rabbitmq service (`mqsender.py`) puts tasks to queues, takes results from result queues and updates indicators.

Remote side (network sensors) performed by [sensor](https://gitlab.com/yaroslaff/sensor) project.

If one rabbitmq server and one rabbitmq vhost used by many okerr clusters, they will share sensors. (e.g. sensors from Cluster One will get tasks from Cluster Two), this scheme is fully working. If you want to isolate clusters, you may use different rabbitmq servers or vhosts.

## Queues
Main communication point between server and sensors is `hello_ex` exchange (type 'fanout'). Sensors sends there their 'hello' messages and lists their queues, servers reads it and then sends tasks to these queues.

### Server queues

Server (task manager) declares result queue, hello queue and two client-side queues `tasks:any` and `tasksq:any` (these two queues are must exists even if clients are not running and not declared them). 

#### Result queue
Random name, usually named `results:HOSTNAME:PID`. Each tasks from manager includes this queue name, and sensor will put result to this queue.

#### Hello queue
Random name, usually named `hello:HOSTNAME:PID`. This queue is bound to `hello_ex` exchange.

### Client queues
In examples we will suppose our server name is `mysensor@msk.ru` (Located in Russia, Moscow)

Client (network sensor) creates following queues:
- `ctl:NAME` - e.g. `ctl:mysensor@msk.ru` - control queue . 
- `tasks:any` - main regular tasks queue, there goes all tasks to perform check for 'regular' indicators with long period (longer then `MQ_QUICK_TIME`)
- `tasksq:any` - main queue tasks queue, there goes all tasks to perform check for 'quick' indicators with short period (less or equal `MQ_QUICK_TIME`)
- Location specific regular queues:
  - `tasks:mysensor@msk.ru`
  - `tasks:msk.ru`
  - `tasks:ru`
- Location specific quick queues:
  - `tasksq:mysensor@msk.ru`
  - `tasksq:msk.ru`
  - `tasksq:ru`

Regular task queues are processed by sensor worker processes, control and quick tasks queues are processes by sensor main process.


## RabbitMQ brief cheatsheet

```
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
