---
description: Configuring report styles
---

# Configuring Global Report Options

Ghostwriter's report engine pulls several default style settings from Command Center. The settings apply to all projects and reports.

## Report Configuration

The Global Report Configuration section contains options for customizing the contents of your Word documents.

{% hint style="info" %}
Due to limitations in the PowerPoint API (and the nature of PowerPoint compared to Word), options for borders, tables, and figures only apply to Word documents right now.
{% endhint %}

### Borders

If you enable borders for pictures, you can set a border line weight (in EMUs; default is `12700` EMUs, or 1pt) and a border color code (e.g., `2D2B6B`).

<figure><img src="../.gitbook/assets/image (5) (2).png" alt="Border configuration options"><figcaption><p>Border Configuration</p></figcaption></figure>

### Tables and Figures

Ghostwriter will add cross-reference (e.g., bookmarks) for figures and tables. Configure a label and a separator character that will appear between your label and your captions. The default separator is an en dash (–).

<figure><img src="../.gitbook/assets/image (3) (1).png" alt="Table and figure options"><figcaption><p>Table and Figure Options</p></figcaption></figure>

### Report Generation Options

Finally, you can select default report templates and configure a default filename for new report downloads. The filenames can be generic or include placeholders to create dynamic filenames.

<figure><img src="../.gitbook/assets/image (2) (2).png" alt="Report generation options for filename and templates"><figcaption><p>Report generation Options for Filenames and Default Templates</p></figcaption></figure>

Filenames can use the following placeholder strings or [date formatting characters](https://docs.djangoproject.com/en/4.1/ref/templates/builtins/#date):

<table><thead><tr><th width="238">Placeholder</th><th>Description</th></tr></thead><tbody><tr><td><code>{date}</code></td><td>The current date formatted with your <a href="https://www.ghostwriter.wiki/getting-started/quickstart#customizing-the-date-format">configured date format</a>.</td></tr><tr><td><code>{client}</code></td><td>The name of the client associated with the project.</td></tr><tr><td><code>{company}</code></td><td>Your configured company name (<a href="personalizing-company-information.md">Company Information</a>).</td></tr><tr><td><code>{assessment_type}</code></td><td>The assessment type set for the project.</td></tr></tbody></table>

{% hint style="success" %}
The default filename value is a good example of how to use these dynamic elements. The default value is:

`{Y-m-d`}`_{His} {company} - {client} {assessment_type} Report`

That translates into a string like this:

`2022-11-02_185117 SpecterOps - Ghostwriter Red Team Report`
{% endhint %}

### Severity Categories and Color

Severity categories are managed under the _Reporting_ application and the _Severity_ model. Set a color code for each severity category (e.g., `966FD6`).

{% hint style="danger" %}
Do not change the name or weights without updating the HTML templates. Some templates modify styles based on specific severity names. Categories cannot be sorted alphabetically, so weights maintain the ordering in reports.
{% endhint %}

<figure><img src="../.gitbook/assets/image (9) (1).png" alt="Severity settings for name, weight, and color"><figcaption><p>Severity Settings</p></figcaption></figure>
