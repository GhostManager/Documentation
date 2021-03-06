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

### Monitoring

* VirusTotal Configuration
* Cloud Services Configuration

### Syncing & Updating

* Namecheap Configuration

### Notifications

* Slack Configuration

{% hint style="info" %}
Other notification options may be added in the future. Email and services such as Pushover are possibilities. That said, you can add your own notification mediums and tasks in _tasks.py_.

See [Background Tasks](../features/background-tasks/) for more information.
{% endhint %}

