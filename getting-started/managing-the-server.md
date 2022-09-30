---
description: How to use Ghostwriter CLI to monitor and manage your Ghostwriter installation
---

# Managing the Server

## Using Ghostwriter CLI to Manage the Server

Ghostwriter CLI can handle just about anything you might need or want to do with your Ghostwriter installation. Running the tool with the `help` command–or no command–will print the latest usage information.

The following sections explain some of the core functionality.

### Managing Docker Containers

Ghostwriter CLI's `containers` command contains the following subcommands:

* `build` : Rebuild the containers (not the data volumes)
* `up` : Bring up Ghostwriter containers
* `down` : Bring down Ghostwriter containers
* `start`: Start all stopped services and restart any running services
* `stop`: Stop all running serviced
* `restart` : Stop and restart all services

If you ever need to check which containers are running, issue the `running` command. This command lists all running containers related to Ghostwriter. The output will look something like this (edited for easier display):

```
$ ./ghostwriter-cli running
[+] Collecting list of running Ghostwriter containers...
[+] Found 4 running Ghostwriter containers

 Name                   Container ID    Image                           Status                  Ports
 ––––––––––––           ––––––––––––    ––––––––––––                    ––––––––––––            ––––––––––––
 ghostwriter_graphql    d90e....        ghostwriter_local_graphql       Up 43 hours (healthy)   0.0.0.0:9691:9691 » 9691/tcp, 0.0.0.0:8080:8080 » 8080/tcp
 ghostwriter_queue      0559....        ghostwriter_local_queue         Up 43 hours (healthy)
 ghostwriter_django     bf9b....        ghostwriter_local_django        Up 43 hours (healthy)   0.0.0.0:8000:8000 » 8000/tcp
 ghostwriter_postgres   9992....        ghostwriter_production_postgres Up 43 hours (healthy)   0.0.0.0:5432:5432 » 5432/tcp
```

{% hint style="success" %}
The `Status` column shows the uptime and health of the service. If you see an `unhealthy` status, that means that service failed a health check and it may not be working properly. Learn more about health checks here: [Health Monitoring](../features/health-monitoring.md)
{% endhint %}

You can use the `logs` command to view a particular container's recent log events. The command requires the name of a running container. Valid container names are:

* all
* django
* graphql
* nginx
* postgres
* redis

Using `all` for the container name will return logs from all running containers. By default, `logs` will return up to 500 lines. You can use the `--lines` flag to adjust how many lines you want to retrieve.

```
$ ./ghostwriter-cli logs ghostwriter_django

PostgreSQL is available
No changes detected
Operations to perform:
  Apply all migrations: account, admin, api, auth, commandcenter, contenttypes, django_q, home, oplog, reporting, rest_framework_api_key, rolodex, sessions, shepherd, sites, socialaccount, users
Running migrations:
  No migrations to apply.
INFO:     Will watch for changes in these directories: ['/app']
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [18] using watchgod
INFO:     Started server process [20]
INFO 2022-06-06 21:47:37,673 server 20 140231754545992 Started server process [20]
INFO:     Waiting for application startup.
INFO 2022-06-06 21:47:37,673 on 20 140231754545992 Waiting for application startup.
INFO:     ASGI 'lifespan' protocol appears unsupported.
INFO 2022-06-06 21:47:37,674 on 20 140231754545992 ASGI 'lifespan' protocol appears unsupported.
INFO:     Application startup complete.
INFO 2022-06-06 21:47:37,674 on 20 140231754545992 Application startup complete.
```
