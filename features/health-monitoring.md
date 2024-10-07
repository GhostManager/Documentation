---
description: How to monitor the health of Ghostwriter services
---

# Health Monitoring

## Introducing Health Monitoring

Ghostwriter monitors the health of its services in two ways: Docker health checks and internal monitoring and testing.

### Docker Health Checks

Docker automatically monitors the containers via `HEALTHCHECK` commands (see [Docker documentation](https://docs.docker.com/engine/reference/builder/#healthcheck) for technical information). These commands check to make sure the service is responding and basic functionality is working.

The results of these commands can be checked with this command:

```
./ghostwriter-cli running
```

Each container runs a service-specific command on a schedule. If the command returns successfully (exit code `0`), the `Status` column will show `healthy`. Any other exit code will flip the status to `unhealthy`, indicating the service is likely not functioning properly.

By default, the commands run with these attributes that can be adjusted via Ghostwriter CLI's `config set` command:

* Start running after 30s (`HEALTHCHECK_START`, default is `30s`)
* Run every 120s (`HEALTHCHECK_INTERVAL`, default is `120s`)
* Timeout after 10s (`HEALTHCHECK_TIMEOUT`, default is `10s`)
* Will be retried once (`HEALTHCHECK_RETRIES`, default is `1`)

### Internal Testing and Monitoring

Ghostwriter also tests each service more thoroughly with two API endpoints:

* /status/
* /status/simple/

The first endpoint, _/status_, tests critical services and displays a table of results:

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption><p>Ghostwriter's Detailed System Status Dashboard</p></figcaption></figure>

These tests are more thorough than the Docker health checks. For example, Docker will verify the database back end is listening and accepting connections, but Ghostwriter runs tests to ensure the database is accepting connections and reading and writing are working as expected. Ghostwriter also checks disk usage is below 90% and at least 100MB of memory is available. These checks are can be adjusted via Ghostwriter CLI's `config set` command:

* `HEALTHCHECK_DISK_USAGE_MAX` (Default is `90`)
* `HEALTHCHECK_MEM_MIN` (Default is `100`)

{% hint style="success" %}
This endpoint can also return a JSON version of the test results if you set the `Accept: application/json` header.
{% endhint %}

Running these tests constantly could be a strain on the server, so the tests run on-demand when you visit this page.

You can visit the simplified endpoint, _/status/simple/_, to run lightweight checks. This endpoint checks the web server, database status, and cache status and returns one of the following responses:

<table><thead><tr><th width="158" align="center">Response</th><th width="172" align="center">Response Code</th><th>Description</th></tr></thead><tbody><tr><td align="center">OK</td><td align="center">200</td><td>System is healthy</td></tr><tr><td align="center">WARNING</td><td align="center">200</td><td>One or more tests did not pass and you should check the detailed status endpoint</td></tr><tr><td align="center">ERROR</td><td align="center">500</td><td>There was an unexpected error indicating a critical issue</td></tr></tbody></table>

The home dashboard displays a basic system health status. The status is based on the results from the simplified endpoint.

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption><p>Home Dashboard System Status Card</p></figcaption></figure>

## Automated Monitoring

You can automate monitoring with something like Uptime Kuma or a similar tool. The recommended logic is:

* Request the _/status/simple/_ endpoint
* Check for the response code and content
* If `OK` is not in the response or the response code is not `200`, send an alert with a link to _/status/_ for details

If the tool is capable of triggering a secondary action, you could have the tool pull the JSON data from _/status/_:

```json
$ curl -k https://ghostwriter.local/status/ -H "Accept: application/json"
{"Cache backend: default": "working", "DatabaseBackend": "working", "DefaultFileStorageHealthCheck": "working", "DiskUsage": "working", "MemoryUsage": "working", "MigrationsHealthCheck": "working", "RedisHealthCheck": "working"}
```

Working services will have a `working` status. A service experiencing a problem will have a descriptive warning or error message that will tell you why it failed the test. You can use this information to customize your monitoring alert.
