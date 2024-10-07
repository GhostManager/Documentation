---
description: Pass BloodHound data to Ghostwriter for inclusion in your reports
---

# Integrating with BloodHound for Reporting

BloodHound Community Edition (BHCE) is a part of many assessments, but using the data in reports can be difficult. The BHCE data is readily available as JSON, but the JSON files are typically large for most Active Directory (AD) environments outside of a lab environment. Also, the data’s full value comes from your analysis, so feeding the raw JSON to Ghostwriter isn’t the way to go. No one wants to copy and paste the contents of a dozen JSON files into fields anyway.

We can leverage BHCE and Ghostwriter’s robust APIs to perform analysis, automatically pass the JSON to Ghostwriter, and store it in a JSON field. This example is covered more in-depth in this article:

{% embed url="https://posts.specterops.io/ghostwriter-v4-3-sso-json-fields-and-reporting-with-bloodhound-976835a7edba" %}

A proof-of-concept script is available here:

{% embed url="https://github.com/GhostManager/bloodhound-integration-poc?source=post_page-----976835a7edba--------------------------------" %}

