---
description: How to use Ghostwriter CLI to monitor and manage your Ghostwriter installation
---

# Managing the Server

## Using Ghostwriter CLI to Manage the Server

Ghostwriter CLI can handle just about anything you might need or want to do with your Ghostwriter installation. Running the tool with the `help` command–or no command at all–will print the latest usage information.

The following sections explain some of the core functionality.

### Managing Docker Containers

Ghostwriter CLI can perform the following actions:

* `up <dev|prod>` : Bring up Ghostwriter containers
* `down <dev|prod>` : Bring down Ghostwriter containers
* `build <dev|prod>` : Rebuild the containers (not the data volumes)
* `restart <dev|prod>` : Bring all containers down and then back up

If you ever need to check which containers are running, issue the `running` command. This command lists all running containers related to Ghostwriter. The output will look something like this:

```
$ ./ghostwriter-cli running
[+] Collecting list of running Ghostwriter containers...
* ghostwriter_local_graphql
* ghostwriter_local_queue
* ghostwriter_local_django
* ghostwriter_production_postgres
```

### Collecting Logs

You can use the `logs` command to view a particular container's recent log events. The command requires the name of a running container. Valid container names are:

* ghostwriter\_django
* ghostwriter\_graphql
* ghostwriter\_nginx
* ghostwriter\_postgres
* ghostwriter\_redis

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
