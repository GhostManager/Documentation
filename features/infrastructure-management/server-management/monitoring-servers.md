---
description: Performing health checkups on servers
---

# Monitoring Servers

## Server Health Checks

Ghostwriter can monitor servers for open ports and alert you if a service is exposed to the internet. By default, the `tasks.scan_servers` function will scan all servers in the server library for open ports.

{% hint style="danger" %}
For this to work properly the Ghostwriter server should **not** be in your management block, i.e. the IP block allowed to talk to your servers.
{% endhint %}

If a server is found to be in use on an active assessment with an open port, the task will send a Slack notification \(if enabled\) to the project's channel.

### Scheduling Server Scans

Use your own judgement to determine how best to handle scanning. You may wish to setup the task to run once an hour, or even more frequently.

To increase speed and only scan servers in active use, set `only_active=True` in the kwargs.

