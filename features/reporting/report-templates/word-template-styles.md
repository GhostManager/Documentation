---
description: Using Ghostwriter's Word styles
---

# Word Template Styles

## Ghostwriter's Styles

You can configure many variables for a style in a Word document. In some cases, Ghostwriter must apply styles for you (ex: formatting text evidence in a finding). You can refine these styles by creating and editing these styles in your templates.

### Customizing the Styles

These styles are called by name:

| Style Name     | Description                                                                                                                                        |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| CodeBlock      | Style text evidence and anything in the WYSIWYG editor's code editor (must be a _Paragraph_ style type)                                            |
| CodeInline     | Style runs of text formatted as code in the WYSIWYG editor (must be a _Character_ style type)                                                      |
| Number List    | Style numbered (ordered) lists                                                                                                                     |
| Bullet List    | Style bulleted (unordered) lists                                                                                                                   |
| Caption        | Built-in style used for captions below evidence and lines pre-ceded by the _\{{.caption\}}_ expression                                             |
| List Paragraph | Built-in base style used for bulleted and numbered lists; the fallback built-in style if your template lacks the customized styles mentioned above |
| BlockQuote     | Style used for block quotes.                                                                                                                       |
| Table Grid     | Built-in style used for tables                                                                                                                     |

{% hint style="success" %}
**Note on List Styles**

You can choose not to create these list styles, but lists will probably not look right in your document. Word's built-in styles (e.g., _List Paragraph_) do not apply the proper/expected indentation. This leads to problems during later editing.

When you create a list in Word, the application applies _List Paragraph_ and additional styling depending on your selection (numbered or bulleted). The style will appear as _List Paragraph,DAI2_ or similar.

This style **does not exist** in your template until you use it once, so Ghostwriter can't default to using it. (See below.)

Create a numbered list, open the styles tab, and save the style as a new style named _Numbered List_. Repeat this for bulleted lists.

Feel free to modify the indentation for nested list items and any other style variables before you save your new style.
{% endhint %}

{% hint style="success" %}
**Note on Built-in Styles**

Word offers many, many built-in styles you might expect to be available to Ghostwriter; however, these styles only exist in the Word _application_. Word will only add a style to your template's styles.xml when you use it to keep file size down.

This means a style like _Caption_ will not exist in your template until you've applied it or created it yourself.

To add one of the built-in styles to your template, apply it once and save your document. You can undo the style. The important thing is the style appears under the _In current document_ list in the Styles Pane.
{% endhint %}
