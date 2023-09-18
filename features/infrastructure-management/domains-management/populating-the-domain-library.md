---
description: Adding domains to the library
---

# Populating the Domain Library

## Adding Domains

Domains can be added to the library one at a time or loaded en masse from a file.

{% tabs %}
{% tab title="Add One Domain" %}
To add just one domain name to the library, click the **Domains** tab on the menu bar and **Add New Domain**. This opens the domain form for documenting and submitting a single domain name.

You may not know the latest health information for the domain. You can set the domain to **Healthy** as a default value and leave categories blank.

<figure><img src="../../../.gitbook/assets/image (17).png" alt="New domain form"><figcaption><p>New Domain Form</p></figcaption></figure>
{% endtab %}

{% tab title="Upload Domains" %}
To bulk add servers to the library, visit the admin panel and navigate to the **Domains** model.  Click the **Import** button and follow the on-screen instructions.

![Domain Import](<../../../.gitbook/assets/image (13) (1).png>)

You can upload csv, xls, xlsx, tsv, json, and yaml files. Select the matching format from the dropdown menu.

After a moment, the admin panel will display a diff screen and ask you to approve the changes.

{% hint style="success" %}
If a domain already exists in the library, the import will update the existing record instead of discarding the data or duplicating the entry.
{% endhint %}
{% endtab %}
{% endtabs %}

