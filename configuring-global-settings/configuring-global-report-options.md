---
description: Configuring report styles
---

# Configuring Global Report Options

Ghostwriter's report engine pulls several default style settings from Command Center. The settings apply to all projects and reports.

{% hint style="success" %}
Per report overrides are in development for all of the following settings.
{% endhint %}

You can set a weight \(in EMUs; default is `12700` EMUs, or 1pt\) for borders, a color code for border color \(e.g., `2D2B6B`\), and the separator between "Table" or "Figure" and your captions. The default separator is an en dash \(–\).

{% hint style="info" %}
These values only affect Word documents, but any future additions will also live here.
{% endhint %}

![Global Report Configuration](../.gitbook/assets/image%20%2823%29.png)

### Severity Categories and Color

Severity categories are managed in the Reporting application, under the Severity model. Set a color code for each severity category \(e.g., `966FD6`\).

{% hint style="danger" %}
Do not change the name or weights without updating the HTML templates. Some templates modify styles based on specific severity names. Categories cannot be sorted alphabetically, so weights maintain the ordering in reports.
{% endhint %}

![Severity Settings](../.gitbook/assets/image%20%289%29.png)

