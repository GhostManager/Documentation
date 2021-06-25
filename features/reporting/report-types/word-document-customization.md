---
description: Customizing Word document generation
---

# Word Document Customization

## Getting Started with Word

Ghostwriter uses template documents and the Jinja2 template language \([https://jinja.palletsprojects.com/en/2.11.x/](https://jinja.palletsprojects.com/en/2.11.x/)\) to give you as much control over the document generation as possible.

Learn more about managing report templates here:

{% page-ref page="../report-templates/" %}

You will need to upload at least one Word document to use as a template. Once you have a template you can pick from, open you report and select the template from the dropdown menu under the **Generate Reports** section. You should see a notification when the template selection is saved. You can then click the Word icon to generate a report.

Depending on the size of your report and template, rendering can take a few seconds. Once the report is done, your browser will download a new Word document.

The default filename will be: `YYYYMMDD_HHMMSS_CLIENT-NAME_ASESSMENT-TYPE.docx`

### Word Templates

Your templates can be simple documents or complete reports. If you currently manage one or more report templates for different types of projects, you can convert those to Ghostwriter templates.

One of the simplest examples of how Ghostwriter can save a team time and effort is replacements. You can create complex dynamic Word templates for Ghostwriter, but the most basic Jinja2 expression is a simple variable, like this one: `{{ report_date }}`

Ghostwriter will replace every instance of `{{ report_date }}` in your template with the current date \(e.g., October 31, 2020\). The replacement does not affect any formatting or styling, so this variable becomes this date and keeps the original font style, color, and placement.

![](../../../.gitbook/assets/image%20%287%29.png)

![](../../../.gitbook/assets/image%20%2817%29.png)

{% hint style="success" %}
Everything seen in the report [JSON](./#json) is accessible as a variable within your template. There are also additional variables, filters, and expressions you can use to refine or customize the report content.

You decide what to include in your templates.
{% endhint %}

Learn more about the available filters, expressions, and variables in this section:

{% page-ref page="../report-templates/word-template-variables.md" %}

### Inserting Evidence

Ghostwriter will insert any evidence files you have added to your findings. The report will have the evidence file's contents followed by your caption. The default behavior is slightly different for images and text.

#### Inline Image Evidence

The report engine will insert images with the following default characteristics:

* 6.5" wide \(full page width\)
* Center aligned
* 1pt border
* Figure caption using: _Figure – Your Caption_

#### Inline Text Evidence

The report engine will insert text evidence with the following default characteristics:

* Applies **Code Block** style
* Left aligned text \(per above style\)
* 1pt border
* Figure caption using: _Figure – Your Caption_

You can edit the weight and color of image borders and the character that follows _Figure_ in the global report configuration.

{% page-ref page="../../../configuring-global-settings/configuring-global-report-options.md" %}



