---
description: Enabling cloud service APIs
---

# Configuring Cloud Services

Ghostwriter can track cloud resources used for projects. If you provide access tokens for Amazon Web Services \(AWS\) and Digital Ocean \(DO\), Ghostwriter has a task that will collect all running server instances and check if any of them are attached to a completed project.

The task will report back with JSON detailing your active \(powered-on\) cloud servers. If you have Slack enabled, Ghostwriter will send notifications to let you know if an active cloud server is not tracked as part of a project or is tracked as part of a project that has ended.

### Ignoring Specific Assets

You may spin-up cloud servers on the same account that you do not want monitored. You can tag these servers with an "ignore tag" of your choosing. Provide a comma-separated list of tags for Ghostwriter to ignore when checking cloud assets.

![Cloud Services Configuration](../../.gitbook/assets/image%20%2810%29.png)

{% hint style="success" %}
This task is under development to support monitoring Microsoft Azure and additional AWS resources \(S3 buckets, Elastic IPs\).
{% endhint %}

