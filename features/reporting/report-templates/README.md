---
description: Managing report templates for Word and PowerPoint
---

# Report Templates

## Introducing Report Templates

Ghostwriters builds Word and PowerPoint reports using templates. The project includes two basic templates: _template.docxs_ and _template.pptx_.

### Report Template Library

The template library is where you manage all of your templates. The library lives under `/reporting/templates/`.

#### Uploading Templates

Upload a template by selecting **Reports** from the sidebar and then clicking **Add New Report Template**. The template form has a few options for controlling how the template is used and who can edit it.

![The Report Template Form](../../../.gitbook/assets/image%20%2815%29.png)

The **Template Name** is what users will select from the report template dropdown menus. The **Doc Type** \(_docx_ or _pptx_\) affects under which reports the template will appear as an option and how the template linter reads the document. These two fields are required.

The only other required field is the template document.

These other fields are all optional:

* **Description** – Use to describe a template or note how it should be used
* **Client** – If a client is selected, the template will only be available when generating a report for a project linked to the selected client
* **Protected** – If checked, only administrators will be able to edit the template
* **Change Log** – Use it to keep track of changes \(pre-filled with the current date and an initial message when a template is first uploaded\)

### How Ghostwriter Uses Templates

When you generate a Word or PowerPoint document, Ghostwriter will follow this workflow:

1. Fetch your selected template OR the default template for that document type \(docx or pptx\)
2. Check the linter status of the template
3. If the linter status is acceptable \(not `failed` or `error`\), proceed with report generation

Pay attention to the linter messages for your templates. You want to use templates that have successfully passed linting. A template with warnings is acceptable but the results may not be what you want.

Review the [Report Template Linting](report-template-linting.md) section for details.

