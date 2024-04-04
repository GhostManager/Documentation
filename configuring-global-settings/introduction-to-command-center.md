---
description: Configuring global variables for Ghostwriter and report generation
---

# Introduction to Command Center

## Introducing Global Configurations

An administrator can update global configuration at any time in the Django admin panel under the _Command Center_ section. Visit _/admin/commandcenter/_ to view the current configuration on your server. Visit each configuration and personalize your settings.

{% hint style="info" %}
The Ghostwriter server does **not** need to be restarted for changes to take effect.
{% endhint %}

### Reporting Options

* Company Information Configuration
* Global Report Configuration

### Customizing Models

* Extra Fields Configuration

### Monitoring

* VirusTotal Configuration
* Cloud Services Configuration

### Syncing & Updating

* Namecheap Configuration

### Notifications

* Slack Configuration

{% hint style="info" %}
If you don't use Slack, you can customize notifications and tasks in _tasks.py_. You may also consider using the GraphQL API to create your own integrations.

See [Background Tasks](../features/background-tasks/) and [GraphQL API](../features/graphql-api/) for more information.
{% endhint %}

