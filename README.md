# Minio & Monk
This repository contains Monk.io template to deploy Minio Cluster either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

# Prerequisites
- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

#### Make sure monkd is running.
```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository
```bash
git clone https://github.com/Burakhan/monk-airflow
```

## Load Template
```bash
cd monk-airflow
monk load MANIFEST
```


#### Let's take a look at the themes I have installed.
```bash
foo@bar:~$ monk list monk-airflow
✔ Got the list
Type      Template                        Repository  Version  Tags
runnable  monk-airflow/airflow-db         local       -        -
runnable  monk-airflow/airflow-init       local       -        -
runnable  monk-airflow/airflow-redis      local       -        -
runnable  monk-airflow/airflow-scheduler  local       -        -
runnable  monk-airflow/airflow-triggerer  local       -        -
runnable  monk-airflow/airflow-webserver  local       -        -
runnable  monk-airflow/airflow-worker     local       -        -
group     monk-airflow/stack              local       -        -

```

## Deploy Stack
```bash
foo@bar:~$ monk run monk-airflow/stack
? Select tag to run [local/monk-airflow/stack] on: mnk
✔ Starting the job: local/monk-airflow/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% redis:latest mnk-1
✔ [================================================] 100% apache/airflow:latest mnk-1
✔ [================================================] 100% apache/airflow:latest mnk-2
✔ [================================================] 100% apache/airflow:latest mnk-3
✔ [================================================] 100% postgres:latest mnk-2
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Started local/monk-airflow/stack

🔩 templates/local/monk-airflow/stack
 ├─🧊 Peer mnk-3
 │  ├─🔩 templates/local/monk-airflow/airflow-webserver
 │  │  └─📦 60a902f21e83d5d530e4cefb7817465a-bserver-monk-airflow-webserver
 │  │     ├─🧩 apache/airflow:latest
 │  │     ├─💾 /var/lib/monkd/volumes/monk-airflow/plugins -> /opt/airflow/plugins
 │  │     ├─💾 /var/lib/monkd/volumes/monk-airflow/logs -> /opt/airflow/logs
 │  │     ├─💾 /var/lib/monkd/volumes/monk-airflow/dags -> /opt/airflow/dags
 │  │     └─🔌 open 16.170.15.2:8080 (0.0.0.0:8080) -> 8080
 │  └─🔩 templates/local/monk-airflow/airflow-scheduler
 │     └─📦 20eec77a186a3db7e5c614a66a395b15-heduler-monk-airflow-scheduler
 │        ├─🧩 apache/airflow:latest
 │        ├─💾 /var/lib/monkd/volumes/monk-airflow/dags -> /opt/airflow/dags
 │        ├─💾 /var/lib/monkd/volumes/monk-airflow/logs -> /opt/airflow/logs
 │        └─💾 /var/lib/monkd/volumes/monk-airflow/plugins -> /opt/airflow/plugins
 ├─🧊 Peer mnk-2
 │  ├─🔩 templates/local/monk-airflow/airflow-worker
 │  │  └─📦 d9601c1aaa9701c9c785068324913590-low-worker-monk-airflow-worker
 │  │     ├─🧩 apache/airflow:latest
 │  │     ├─💾 /var/lib/monkd/volumes/monk-airflow/plugins -> /opt/airflow/plugins
 │  │     ├─💾 /var/lib/monkd/volumes/monk-airflow/dags -> /opt/airflow/dags
 │  │     └─💾 /var/lib/monkd/volumes/monk-airflow/logs -> /opt/airflow/logs
 │  └─🔩 templates/local/monk-airflow/airflow-db
 │     └─📦 de055128a0fe44f24c11305442a60ef6-nk-airflow-airflow-db-postgres
 │        ├─🧩 postgres:latest
 │        ├─💾 /var/lib/monkd/volumes/db_data -> /var/lib/postgresql/data
 │        └─🔌 open 16.170.207.32:5432 (0.0.0.0:5432) -> 5432
 └─🧊 Peer mnk-1
    ├─🔩 templates/local/monk-airflow/airflow-init
    │  └─📦 80ba963efac1b18cd7bdccd76746c45a-airflow-init-monk-airflow-init
    │     ├─🧩 apache/airflow:latest
    │     ├─💾 /var/lib/monkd/volumes/monk-airflow/plugins -> /opt/airflow/plugins
    │     ├─💾 /var/lib/monkd/volumes/monk-airflow/logs -> /opt/airflow/logs
    │     └─💾 /var/lib/monkd/volumes/monk-airflow/dags -> /opt/airflow/dags
    ├─🔩 templates/local/monk-airflow/airflow-triggerer
    │  └─📦 d2ed4da697f7abcd364821b6fb1e18a8-iggerer-monk-airflow-triggerer
    │     ├─🧩 apache/airflow:latest
    │     ├─💾 /var/lib/monkd/volumes/monk-airflow/plugins -> /opt/airflow/plugins
    │     ├─💾 /var/lib/monkd/volumes/monk-airflow/dags -> /opt/airflow/dags
    │     └─💾 /var/lib/monkd/volumes/monk-airflow/logs -> /opt/airflow/logs
    └─🔩 templates/local/monk-airflow/airflow-redis
       └─📦 412c35109961fed970701b5e92162f52-rflow-redis-monk-airflow-redis
          └─🧩 redis:latest

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/monk-airflow/stack - Inspect logs
	monk shell     local/monk-airflow/stack - Connect to the container's shell
	monk do        local/monk-airflow/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```
 
 ## Check web service

 ```
http://16.170.15.2:8080/
```


## Variables
The variables are in `stack.yml` file. You can quickly setup by editing the values here.

| Variable                     	| Description                               	|
|------------------------------	|-------------------------------------------	|
| database_name                 | Airflow database name, Default: airflow 	               |
| database_password             | Airflow database password, Default airflow               	|
| database_user                 | Airflow database user, Default: airflow                  	|
| database_port                 | Airflow database port, Default: 5432                     	|
| webserver_port                | Airflow Proxy listen, Default 8080                     	|
| airflow_password               | Airflow admin username, Default airflow                 	|
| airflow_username               | Airflow admin password, Default airflow                 	|



## Stop, remove and clean up workloads and templates

```bash
monk purge -x -a
```