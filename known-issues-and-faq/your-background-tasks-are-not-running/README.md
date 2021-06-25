---
description: Your background tasks may stop running properly
---

# Your Background Tasks Are Not Running

On 9 August 2019 an issue with Django Q was identified:

{% embed url="https://github.com/Koed00/django-q/issues/377" %}

If you initially built Ghostwriter before or around the same time as this issue was reported you may notice the `queue` container's logs repeat `ERROR unknown attribute: "days"` and scheduled tasks show the last run results as a grey `?` badge. This is caused by the container using the latest version of the `Arrow` library that is incompatible with your version of Django Q.

This issue was resolved \(see the GitHub issue thread\) but you will need to rebuild your Ghostwriter containers with `django-q==1.0.2` to make sure you have the version with the fixed code.

