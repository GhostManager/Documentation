---
description: Enabling cloud service APIs
---

# Configuring Cloud Services

Ghostwriter can track cloud resources used for projects. If you provide access tokens for Amazon Web Services \(AWS\) and Digital Ocean \(DO\), Ghostwriter has a task that will collect all running server instances and check if any of them are attached to a completed project.

The task will report back with JSON detailing your active \(powered-on\) cloud servers. If you have Slack enabled, Ghostwriter will send notifications to let you know if an active cloud server is not tracked as part of a project or is tracked as part of a project that has ended.

### Ignoring Specific Assets

You may spin up cloud servers on the same account that you do not want to be monitored. You can tag these servers with an "ignore tag" of your choosing. Provide a comma-separated list of tags for Ghostwriter to ignore when checking cloud assets.

![Cloud Services Configuration](../../.gitbook/assets/image%20%2810%29.png)

{% hint style="info" %}
This task is under development to support monitoring Microsoft Azure and additional AWS resources \(e.g., Elastic IPs\).
{% endhint %}

### Configuring AWS Keys

Ghostwriter is designed to use minimal AWS privileges so you do not need to assign any serious privileges to the keys you use for monitoring your AWS resources. Ghostwriteraccesses and reads from the following services:

* STS – Ghostwriter connects and calls `get-caller-identity` to test your keys
* Lightsail – Collects your running Lightsail instances and related identifiers
* EC2 – Collects your running EC2 instances and related identifiers
* S3 – Collects your list of buckets

{% hint style="success" %}
Keep Ghostwriter's privileges limited. Ghostwriter does not need to be able to upload files to S3 or modify instances, or access storage volumes. The monitoring task only needs to read resource information \(i.e., use "Get," "List," and "Describe" permissions\).
{% endhint %}

Fetching instance information from Lightdail and EC2 requires specifying a region. To determine which regions your account has enabled, Ghostwriter calls EC2's `describe-regions` and Lightsail's `get-regions`.

Then, Ghostwriter uses an EC2 resource to call `instances` and a Lightsail client to call `get-instances` to build a list of instances. This data includes:

* ARN
* Name
* State
* Private IP\(s\)
* Public IP\(s\)
* LaunchTime
* Tags
* Misc. Networking and Hardware

For S3, Ghostwriter calls `list-buckets` to get a list of all buckets.  This data includes the bucket's name and the date on which it was created.





