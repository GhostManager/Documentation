---
description: Introducing the Word template variables
---

# Word Template Variables & Filters

## Jinja2 Statements, Expressions, & Filters

When you request a Word document, Ghostwriter opens your selected template file and processes any Jinja2 expressions within the document to create a new document. The new document is saved in memory and sent to you for download.

Jinja2 uses _statements_, _expressions_, and _filters_. These equate to lines of code and variables:

* **Statement** – `{% ... %}`
  * Statements are lines of code like `{% if some_variable %}`
* **Expression** – `{{ ... }}`
  * In general, an expression works like a variable in most cases, like `{{ client.name }}`
* **Filter** – `... |filter ...`
  * You can pipe a value into a filter to modify it, like `{{ client.name|title }}`

Templates can contain basic expressions and more complicated statements (e.g., for loops, if/else). In addition to the custom expressions and filters documented on this page, Jinja2 offers built-in statements, expressions, and filters you can use with Ghostwriter templates.

{% hint style="info" %}
To prevent cross-site scripting (XSS), Ghostwriter sanitizes formatted text fields. This sanitization creates a minor conflict with Jinja2 because it will escape `<` and `>` (e.g., replace the character with `%gt;`). If you want to check if something is greater or less than a value, use Jinja2's `gt()` and `lt()` tests.

[https://jinja.palletsprojects.com/en/3.1.x/templates/#jinja-tests.gt](https://jinja.palletsprojects.com/en/3.1.x/templates/#jinja-tests.gt)

[https://jinja.palletsprojects.com/en/3.1.x/templates/#jinja-tests.lt](https://jinja.palletsprojects.com/en/3.1.x/templates/#jinja-tests.lt)

You can freely use `<` or `>` if you use it inside your report template (not a text field inside Ghostwriter).
{% endhint %}

The official Jinja2 documentation contains all of the information you need to get started using its more advanced features:

{% embed url="https://jinja.palletsprojects.com/en/2.11.x/templates/" %}
Jinja2 Template API
{% endembed %}

There are also various considerations covered in the official Jinja2 documentation, such as whitespace control:

{% embed url="https://jinja.palletsprojects.com/en/2.11.x/templates/" %}
Whitespace Control
{% endembed %}

{% hint style="warning" %}
All of Ghostwriter's expressions and statements should be wrapped in curly braces with one space to either side (`{{ client.name }}`or `{% if ... %}` ) – unless otherwise noted.

If you do not include the spaces, Jinja2 will not recognize the expression as valid and will ignore it.
{% endhint %}

{% hint style="success" %}
If you ever need to include double curly braces or Jinja2 code inside a report and you **do not** want it to be rendered, you can escape your text in a couple of ways.

One option is the `{% raw %}{% endraw %}` block.  Another is using Jinja2 's "literal variable delimiter" (`{{`) inside a variable expression (e.g., `{{ '{{' }}`).

[https://jinja.palletsprojects.com/en/3.0.x/templates/#escaping](https://jinja.palletsprojects.com/en/3.0.x/templates/#escaping)
{% endhint %}

### Using Conditionals

One of the easiest and most powerful things you can do in your templates is leverage conditional statements to control content.

For example, you can use an `if` block to check a value to determine the content or formatting. Conditional blocks are powerful when combined with things like [your custom extra fields](../../../configuring-global-settings/configuring-extra-fields.md).

Conditional blocks can be written in a couple of different ways.  The simplest will be familiar to anyone who has written a script in a language like Python:

```
{% raw %}
{% if SomeCondition %}
<Your Content>
{% else %}
<Alternate Content>
{% endif %}
{% endraw %}
```

This approach is easy to read as is, but can be very messy and difficult to follow. Written on several lines like this will cause erroneous blank lines in the final document. You want to remove the newlines before finalizing your template for the best outcome.

```
{% raw %}
{% if SomeCondition %}<Your Content>{% else %}<Alternate Content>{% endif %}
{% endraw %}
```

You can see how this version could become difficult to read. You can often condense your conditional down into a simpler version. For example, let's say you are looping over your project objectives for a table and want a cell colored green if the objective is complete or red if not. This single line handles that formatting:

```
{% raw %}
{% cellbg "A8D08D" if obj.complete else "FF7E79" %}
{% endraw %}
```

{% hint style="success" %}
More on `cellbg`, creating tables, and other functionality below!
{% endhint %}

### Potentially Useful Jinja2 Expressions

These expressions are built into Jinja2 and might be helpful in your Word documents:

| **Expression**                                                        | **Description**                                                                                                                                                                                                                                                                                                               |
| --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `capitalize(string)`                                                  | Capitalize the first character and convert the rest to lowercase                                                                                                                                                                                                                                                              |
| `lower(string)`                                                       | Convert a value to all lowercase                                                                                                                                                                                                                                                                                              |
| `replace(string, old, new)`                                           | Replace the old string (a substring of the first argument) with a new string                                                                                                                                                                                                                                                  |
| `title(string)`                                                       | Return a titlecased string                                                                                                                                                                                                                                                                                                    |
| `trim(value, chars=None)`                                             | Strip leading and trailing characters (default is whitespace)                                                                                                                                                                                                                                                                 |
| `unique(value, case_sensitive=False)`                                 | Return a list of unique items from an iterable                                                                                                                                                                                                                                                                                |
| `upper(string)`                                                       | Convert a value to all uppercase                                                                                                                                                                                                                                                                                              |
| `sort(iterable, reverse=False, case_sensitive=False, attribute=None)` | <p>Sort an iterable with Python's <code>sorted()</code><br><br><a href="https://jinja.palletsprojects.com/en/3.0.x/templates/#jinja-filters.sort"><br></a><a href="https://jinja.palletsprojects.com/en/3.0.x/templates/#jinja-filters.sort">https://jinja.palletsprojects.com/en/3.0.x/templates/#jinja-filters.sort</a></p> |
| `dictsort(mapping, case_sensitive=False, by="key", reverse=False)`    | <p>Like <code>sort</code> but accepts a mapping of key and value pairs to yield a dictionary<br><br><a href="https://jinja.palletsprojects.com/en/3.0.x/templates/#jinja-filters.dictsort">https://jinja.palletsprojects.com/en/3.0.x/templates/#jinja-filters.dictsort</a></p>                                               |

{% hint style="info" %}
There are many other expressions and filters available. If you want to do something, there is probably a way to accomplish it with a built-in expression cleanly. You can perform math, logic, string mutations, and more.

Check the Jinja2 documentation: [https://jinja.palletsprojects.com/en/3.1.x/templates/#expressions](https://jinja.palletsprojects.com/en/3.1.x/templates/#expressions)
{% endhint %}

### Ghostwriter Expressions

To see what is available for your report, generate the JSON report. Everything in the resulting JSON will be available in a report template. The following table describes the top-level keys:

| **Expression** | **Description**                                                                                                       |
| -------------- | --------------------------------------------------------------------------------------------------------------------- |
| report\_date   | \[`String`] Full date the report was generated (localized based on server settings)                                   |
| project        | \[`Dict`] All information about the project                                                                           |
| client         | \[`Dict`] All information about the project's client                                                                  |
| team           | \[`Dict`] All team information (individuals assigned to the project)                                                  |
| objectives     | \[`Dict`] All objectives information                                                                                  |
| targets        | \[`Dict`] All project targets                                                                                         |
| scope          | \[`Dict`] All project scope lists                                                                                     |
| infrastructure | \[`Dict`] All project infrastructure information                                                                      |
| logs           | \[`Dict`] All activity logs and related entries from the project                                                      |
| findings       | \[`Dict`] All information about a project's findings                                                                  |
| observations   | \[`Dict`] All information about a project's observations                                                              |
| docx\_template | \[`Dict`] All information about the selected DOCX template                                                            |
| pptx\_template | \[`Dict`] All information about the selected PPTX template                                                            |
| company        | \[`Dict`] All information about your company (configured in the admin panel)                                          |
| title          | \[`String`] The report's title set in Ghostwriter                                                                     |
| complete       | \[`Bool`] Value indicating if the report has been marked as complete                                                  |
| archived       | \[`Bool`] Value indicating if the report has been marked as archived                                                  |
| delivered      | \[`Bool`] Value indicating if the report has been marked as delivered                                                 |
| totals         | \[`Dict`] Various sums and counts of different project-related values (e.g., total findings, objectives, and targets) |

{% hint style="info" %}
Dates are localized based on your locale configuration in the server settings. The default date format is _M d, Y_ (e.g., June 22, 2021).

The `project` key has separate values for the day, month, and year the project started and ended. Use these to assemble your own date or date range formats if you need to represent a date differently or only want part of the date.
{% endhint %}

{% hint style="success" %}
If you do not have a client `short_name` value set, Ghostwriter will replace references to `client.short_name` with the client's full name.
{% endhint %}

#### Findings Attributes – HTML & Rich Text Attributes

You write your findings in Ghostwriter's WYSIWYG editor, where you can style text as you would directly in Word. The WYSIWYG editor uses HTML, so Ghostwriter stores your content as HTML.

Let's say you put the following Jinja2 code in a template:

```
{% raw %}
{% for finding in findings %}
{{ finding.description }}
{% endfor %}
{% endraw %}
```

That would drop in raw HTML using whatever style you had assigned to `{{ finding.description }}` in the template. It's unlikely you would want that.

Jinja2's `striptags` filter can help, but it removes all HTML without preserving new lines. Ghostwriter's custom `strip_html` filter will strip the tags and preserve newlines, but the output will still be all plaintext. You must re-apply character and paragraph styles, font changes, and other options. Your evidence files will also appear as text placeholders.

To get what you see in the WYSIWYG editor in your Word document, add `_rt` (for rich text) to the attribute's name, use the `p` tag (see **Ghostwriter Tags** below). The above example becomes:

```
{% raw %}
{% for finding in findings %}
{{p finding.description_rt }}
{% endfor %}
{% endraw %}
```

This will drop in your WYSIWYG HTML converted to Open XML for Word. Your image and text evidence will be present (with style and border options applied), and your text will be styled.

Each finding also has a unique `severity_rt` attribute. You don't style this text in the WYSIWYG editor. Ghostwriter creates a rich text version of your severity category that is colored using your configured color code.

The `severity_rt` attribute only styles the color of the text run so that you can apply a paragraph style to it directly in your Word template. Use it with the `r` tag (for a run) like so:

![Severity Category Styled as Heading 4](<../../../.gitbook/assets/image (39).png>)

That template renders as:

![Rendered Severity Category with Chosen Color](<../../../.gitbook/assets/image (70).png>)

### Ghostwriter Tags

Several tags used for Word documents are not built into Jinja2. These tags are added after you open an expression or statement (before the space).

Example: `{{p findings_subdoc }}`

| **Tag** | **Description** |
| ------- | --------------- |
| r       | Text run        |
| p       | New paragraph   |
| tr      | Table row       |
| tc      | Table column    |

The table tags may appear complicated at first. You can create a table with a row for each point of contact using the provided statements, expressions, and tags like this:

![Example of Dynamic Table Generation](<../../../.gitbook/assets/image (47).png>)

{% hint style="danger" %}
Per `python-docx-template`, do not use `{%p`, `{%tr`, `{%tc` or `{%r` twice in the same paragraph, row, column, or run.

Bad:

```
{% raw %}
{%p if display_paragraph %}Here is my paragraph {%p endif %}
{% endraw %}
```

Good:

```
{% raw %}
{%p if display_paragraph %}
Here is my paragraph
{%p endif %}
{% endraw %}
```
{% endhint %}

### Ghostwriter Statements

There are several statements for Word documents that are not built into Jinja2:

| **Statement**               | **Description**                                                   |
| --------------------------- | ----------------------------------------------------------------- |
| `{% cellbg color_var %}`    | Color a table cell where `color_var` is a hex value without the # |
| `{% colspan some_number %}` | Span a table cell over a `some_number` of columns                 |

### Ghostwriter Filters

Ghostwriter offers some custom filters you can use to modify report values quickly:

{% hint style="success" %}
The filter collection is under development and will continue to grow.
{% endhint %}

| **Filter**                          | **Usage**                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `filter_severity(list)`             | <p>Accepts the <code>findings</code> variable and filters it with a list of severities</p><p><br><strong>Example</strong><br>This statement loops over only findings rated as <em>High</em> or <em>Medium</em> severity:</p><p> <code>{% for x in findings|filter_severity(["High", "Medium"]) %}</code></p>                                                                                                                                                  |
| `strip_html(string)`                | Accepts HTML strings and strips all tags.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `compromised(targets)`              | Accepts `targets` value and filters it to only include hosts marked as compromised.                                                                                                                                                                                                                                                                                                                                                                           |
| `filter_type(list)`                 | <p>Accepts the <code>findings</code> variable and filter it with a list of categories.</p><p></p><p><strong>Example</strong><br>This statement loops over only findings with the type <em>Network</em>:</p><p> <code>{% for x in findings|filter_type(["Network"]) %}</code></p>                                                                                                                                                                              |
| `add_days(date,` `days)`            | <p>Provide a date and a number of days (integer) to add or subtract. Use negative numbers for subtraction and Python's date format strings.<br><br><strong>Example</strong><br><code>Feb. 1, 2022 | add_days(-10)</code></p>                                                                                                                                                                                                                                  |
| `format_datetimedate, format_str)`  | <p>Provide a date and a format string. Use Python's date format strings.<br><br><strong>Example</strong><br><code>Feb. 1, 2022 | format_datetime("%B %-d, %Y")</code></p>                                                                                                                                                                                                                                                                                     |
| `get_item(list, index)`             | <p>Provide a list and an index to retrieve the list item at that index.</p><p></p><p><strong>Example</strong><br><code>["ghostwriter", "report", "ghost"] | get_item(0)</code> returns <code>ghostwriter</code></p>                                                                                                                                                                                                                                           |
| `filter_tags(list, allowlist)`      | <p>Accepts a list of objects (e.g., <code>findings</code>) and filters it with a list of tags.</p><p></p><p><strong>Example</strong><br>This statement loops over only findings tagged with <code>xss</code>:</p><p> <code>{% for x in findings|filter_tags(["xss"]) %}</code></p>                                                                                                                                                                            |
| `regex_search(text, regex)`         | Perform a search with a regular expression and get the first match.                                                                                                                                                                                                                                                                                                                                                                                           |
| `replace_blanks(list, placeholder)` | <p>Replace null dictionary keys with <code>""</code> (default) or the specified placeholder value.<br><br><strong>Example</strong><br>Attempting to use Jinja2's <code>sort</code> filter with a list of dictionaries with null values will cause an error. This statement loops over all entries in an activity log while also replacing blank values and then sorting.<br><br><code>{% for entry in log|replace_blanks|sort(attribute="tool") %}</code></p> |

### Subdocuments

Subdocuments are like other variables, except they are pre-rendered Word documents. Inserting a subdocument is like copying and pasting content from one document into another. A subdocument can be a small paragraph or a much larger section.

Ghostwriter uses subdocuments to translate your WYSIWYG content (e.g., findings) to Office Open XML.

Subdocuments are referenced as `{{p VARIABLE }}`. That variable is automatically replaced with the contents of the subdocument.

## Debugging a Template

Ghostwriter uses the `jinja2.ext.debug` extension to make it easier for you to debug a template. Place a `{% debug %}` tag somewhere in your template.

The next time you generate a report with that template, Ghostwriter will replace the tag with the template's available context (the report and project data) and filters.

Also, see [Troubleshooting Word Templates](troubleshooting-word-templates.md) for a more in-depth explanation of how to troubleshoot a template that gives you problems.
