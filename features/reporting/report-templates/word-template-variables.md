---
description: Introducing the Word template variables
---

# Word Template Variables

## Jinja2 Statements, Expressions, & Filters

When you request a Word document, Ghostwriter opens your selected template file and processes any Jinja2 expressions within the document to create a new document. The new document is saved in memory and sent to you for download.

Jinja2 uses _statements_, _expressions_, and _filters_. These equate to lines of code and variables:

* **Statement** – `{% ... %}`
  * Statements are lines of code, like `{% if some_variable %}`
* **Expression** – `{{ ... }}`
  * In general, an expression works like a variable in most cases, like `{{ client }}`
* **Filter** – `... |filter ...`
  * You can pipe a value into a filter to modify it, like `{{ client|title }}`

Templates can contain basic expressions that Ghostwriter simply replaces and more complicated statements \(e.g., for loops, if/else\). In addition to the custom expressions and filters documented on this page, Jinja2 offers built-in statements, expressions, and filters you can use with Ghostwriter templates.

The official Jinja2 documentation contains all of the information you need to get started using its more advanced features:

{% embed url="https://jinja.palletsprojects.com/en/2.11.x/templates/" caption="Jinja2 Template API" %}

There are also various considerations covered in the official Jinja2 documentation, such as whitespace control:

{% embed url="https://jinja.palletsprojects.com/en/2.11.x/templates/" caption="Whitespace Control" %}

{% hint style="danger" %}
All of Ghostwriter's expressions and statements should be wrapped in curly braces with one space to either side \(`{{ client }}`or `{% if ... %}` \) – unless otherwise noted.

If you do not include the spaces, that expression will not be recognized as valid and ignored by Jinja2.
{% endhint %}

### Potentially Useful Jinja2 Expressions

These expressions are built into Jinja2 and might be useful in your Word documents:

| Expression | Description |
| :--- | :--- |
| `capitalize(string)` | Capitalize the first character and convert the rest to lowercase |
| `lower(string)` | Convert a value to all lowercase |
| `replace(string, old, new)` | Replace the old string with a new string |
| `title(string)` | Return a titlecased string |
| `trim(value, chars=None)` | Strip leading and trailing characters \(default is whitespace\) |
| `unique(value, case_sensitive=False)` | Return a list of unique items from an iterable |
| `upper(string)` | Convert a value to all uppercase |

### Ghostwriter Expressions

To see what all is available for your report, generate the JSON report. Everything in the resulting JSON will be available to a report template. The following table describes the top-level keys:

| Expression | Description |
| :--- | :--- |
| report\_date | \[`String`\] Full date the report was generated \(localized based on server settings\) |
| project | \[`Dict`\] All information about the project |
| client | \[`Dict`\] All information about the project's client |
| team | \[`Dict`\] All team information \(individuals assigned to the project\) |
| objectives | \[`Dict`\] All objectives information |
| targets | \[`Dict`\] All project targets |
| scope | \[`Dict`\] All project scope lists |
| findings | \[`Dict`\] All information about a project's findings |
| docx\_template | \[`Dict`\] All information about the selected DOCX template |
| pptx\_template | \[`Dict`\] All information about the selected PPTX template |
| company | \[`Dict`\] All information about your company \(configured in the admin panel\) |
| title | \[`String`\] The report's title |
| complete | \[`Bool`\] Value indicating if the report has been marked as complete |
| archived | \[`Bool`\] Value indicating if the report has been marked as archived |
| delivered | \[`Bool`\] Value indicating if the report has been marked as delivered |
| totals | \[`Dict`\] Various sums and counts of different project-related values \(e.g., total findings, objectives, and targets\) |

{% hint style="info" %}
Dates are localized based on your locale configuration in the server settings. The default is `en-us`, so the default date format is _M d, Y_ \(e.g., June 22, 2021\).

The `project` key has separate values for the day, month, and year the project started and ended. Use these to assemble your own date or date range formats if you need to represent a date differently or only want part of the date.
{% endhint %}

{% hint style="success" %}
If you do not have a client `short_name` value set, Ghostwriter will replace references to `client.short_name` with the client's full name.
{% endhint %}

#### Findings Attributes – HTML & Rich Text Attributes

You write your findings in Ghostwriter's WYSIWYG editor where you can style text like you would directly in Word. The WYSIWYG editor uses HTML, so Ghostwriter stores your content as HTML.

Let's say you put the following Jinja2 code in a template:

```text
{% for finding in findings %}
{{ finding.description }}
{% endfor %}
```

That would drop-in raw HTML using whatever style you had assigned to `{{ finding.description }}` in the template. It's unlikely you would want that.

Jinja2's `striptags` filter can help, but that removes all HTML without preserving new lines. Ghostwriter's custom `strip_html` filter will strip the tags and preserve newlines, but the output will still be all plaintext. You will have to re-apply character and paragraph styles, font changes, and other options. Your evidence files will also appear as their text placeholders.

To get what you see in the WYSIWYG editor in your Word document, add `_rt` \(for rich text\) to the attribute's name and use the `p` tag \(see **Ghostwriter Tags** below\). The above example becomes:

```text
{% for finding in findings %}
{{p finding.description_rt }}
{% endfor %}
```

This will drop-in your WYSIWYG HTML converted to Open XML for Word. Your image and text evidence will be present \(with style and border options applied\) and all of your text will be styled.

Each finding also has a unique `severity_rt` attribute. You don't style this text in the WYSIWYG editor. Ghostwriter creates a rich text version of your severity category that is colored using your configured color code for that category.

The `severity_rt` attribute only styles the color of the text run, so you can apply a paragraph style to it directly in your Word template. Use it with the `r` tag like so:

![Severity Category Styled as Heading 4](../../../.gitbook/assets/image%20%2819%29.png)

That template renders as:

![Rendered Severity Category with Chosen Color](../../../.gitbook/assets/image%20%288%29.png)

### Ghostwriter Tags

There are several tags used for Word documents that are not built into Jinja2. These tags are added after you open an expression or statement \(before the space\).

Example: `{{p findings_subdoc }}`

| Tag | Description |
| :--- | :--- |
| p | New paragraph |
| tr | Table row |
| tc | Table column |
| r | Text run |

The table tags may appear complicated at first. You can create a table with a row for each point of contact using the provided statements, expressions, and tags like this:

![Example of Dynamic Table Generation](../../../.gitbook/assets/image%20%2820%29.png)

{% hint style="danger" %}
Per `python-docx-template`, do not use `{%p`, `{%tr`, `{%tc` or `{%r` twice in the same paragraph, row, column or run.

Bad:

```text
{%p if display_paragraph %}Here is my paragraph {%p endif %}
```

Good:

```text
{%p if display_paragraph %}
Here is my paragraph
{%p endif %}
```
{% endhint %}

### Ghostwriter Statements

There are several statements for Word documents that are not built into Jinja2:

| Statement | Description |
| :--- | :--- |
| {% cellbg color\_var %} | Color a table cell where `color_var` is a hex value without the \# |
| {% colspan some\_number %} | Span a table cell over a `some_number` of columns |

### Ghostwriter Filters

Ghostwriter offers some custom filters you can use to quickly modify report values:

{% hint style="success" %}
The filter collection is under development and will continue to grow.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Filter</th>
      <th style="text-align:left">Usage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">filter_severity(list)</td>
      <td style="text-align:left">
        <p>Accepts the <code>findings</code> variable and filters it with a list of
          severities</p>
        <p>Example: This statement loops over only findings rated as <em>High</em> or <em>Medium</em> severity:</p>
        <p> <code>{% for x in findings|filter_severity([&quot;High&quot;, &quot;Medium&quot;]) %}</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">strip_html</td>
      <td style="text-align:left">Accepts HTML strings and strips all tags.</td>
    </tr>
    <tr>
      <td style="text-align:left">compromised</td>
      <td style="text-align:left">Accepts <code>targets</code> value and filters it to only include hosts
        marked as compromised.</td>
    </tr>
    <tr>
      <td style="text-align:left">filter_type(list)</td>
      <td style="text-align:left">
        <p>Accepts the <code>findings</code> variable and filters it with a list of
          categories</p>
        <p>Example: This statement loops over only findings with the type <em>Network</em>:</p>
        <p> <code>{% for x in findings|filter_type([&quot;Network&quot;]) %}</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

### Subdocuments

Subdocuments are like other variables except they are pre-rendered Word documents. When a subdocument is inserted into the template, it is like copy/pasting content from one document into another. A subdocument can be a small paragraph or much larger sections.

Ghostwriter uses subdocuments to translate your WYSIWYG content \(e.g., findings\) to Office Open XML.

Subdocuments are referenced as `{{p VARIABLE }}`.That variable is automatically replaced with the contents of the subdocument.

## Debugging a Template

Ghostwriter uses the `jinja2.ext.debug` extension to make it easier for you to debug a template. Place a `{% debug %}` tag somewhere in your template.

The next time you generate a report with that template, Ghostwriter will replace the tag with the template's available context \(the report and project data\) and filters.

