---
description: Tracking hostnames and IP addresses in-scope for the project
---

# Scope Tracking

Offensive assessments need scope lists, which should be easy to reference. The _Scope_ tab helps you track as many lists as you like and assign them different properties. Each scope list is a newline-separated list of strings (IP addresses or hostnames).

<figure><img src="../../../.gitbook/assets/image (79).png" alt=""><figcaption><p>Example of Tracked Scope Lists</p></figcaption></figure>

Each list has a name, a preview of the first five lines, a button to expand it into a modal, a note field, and some properties displayed as icons next to the name. You can assign these properties:

* Disallowed – The list includes hosts that should _not be_ touched.
* Requires Caution – The list contains hosts that require a "white glove" approach during testing.

Clicking the _Expand_ button opens a modal that displays the complete list. The _Copy to Clipboard_ button copies the full list to your clipboard in a format suitable for standard tools (e.g., _Nmap_).

<figure><img src="../../../.gitbook/assets/image (80).png" alt=""><figcaption><p>Scope List Modal</p></figcaption></figure>

Additionally, you can click the settings gear to access an _Export Text File_ option. This option downloads a text file with the scope contents. The file can be imported using standard tools like _Nmap_ or Burp Suite.

<figure><img src="../../../.gitbook/assets/image (81).png" alt=""><figcaption><p>Exporting a Text File for Tooling</p></figcaption></figure>
