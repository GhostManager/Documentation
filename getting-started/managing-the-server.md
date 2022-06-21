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

 Container ID  Image                           Status          Ports                              Names
 ––––––––––––  ––––––––––––                    ––––––––––––    ––––––––––––                       ––––––––––––
 de943...       ghostwriter_local_graphql       Up 2 hours      0.0.0.0:8080:8080 » 8080/tcp      /ghostwriter_graphql_engine_1
 89814...       ghostwriter_local_django        Up 2 hours      0.0.0.0:8000:8000 » 8000/tcp      /ghostwriter_django_1
 bf572...       ghostwriter_local_queue         Up 2 hours                                        /ghostwriter_queue_1
 a6b71...       ghostwriter_production_postgres Up 2 hours      0.0.0.0:5432:5432 » 5432/tcp      /ghostwriter_postgres_1
```

### Collecting Logs

You can use the `logs` command to view a particular container's recent log events. The command requires the name of a running container. Valid container names are:

* all
* ghostwriter\_django
* ghostwriter\_graphql
* ghostwriter\_nginx
* ghostwriter\_postgres
* ghostwriter\_redis

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
