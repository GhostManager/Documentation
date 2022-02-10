# 9 February 2022, v2.2.3-rc2

## v2.2.3-rc2

[https://github.com/GhostManager/Ghostwriter/releases/tag/v2.2.3-rc2](https://github.com/GhostManager/Ghostwriter/releases/tag/v2.2.3-rc2)

This is the second release candidate for v2.2.3. This release contains everything from v2.2.3-rc1 with the addition of some new changes.

## New Features

* See v2.2.3-rc1

## Fixed

* Fixed "incomplete file" issue when attempting to download a report template
* Fixed report archiving failing to write zip file
* Fixed toast messages not showing up when swapping report templates
* Fixed sidebar tab appearing below delete confirmations
* See v2.2.3-rc1

## Changed

* Upgraded dependencies to their latest versions (where possible)
  * Django v3.1.13 -> v3.2.11
  * Did not upgrade `docxtpl`&#x20;
    * Awaiting to see how the developer wants to proceed with [issue #114](https://github.com/elapouya/python-docx-template/issues/414)
    * Not upgrading from 0.12 to the latest 0.15.2 has no effect on Ghostwriter at this time
* Collapsed the `Domain` model's various categorization fields into a single `categorization` field with PostgreSQL's `JSONField` type
  * An important milestone/change for the upcoming GraphQL API
  * Categorization is no longer limited to specific vendors
  * Going forward, the field can be manually updated with valid JSON
  * Ghostwriter will look for JSON formatted as a series of keys and values: `{"COMPANY": "CATEGORY", "COMPANY": "CATEGORY",}`
* Converted the `ReportTemplate` model's `lint_result` field to a PostgreSQL `JSONField`
  * An important milestone/change for the upcoming GraphQL API
  * This change increases reliability and performance by removing any need to transform a string representation back into a `dict`
  * Little to no impact on users but templates may need to be linted again after the upgrade
  * If a template is affected, the status will change to "Unknown" with a single warning note: "Need to re-run linting following your Ghostwriter upgrade"
* Converted the `Domain` model's `dns_record` field to a PostgreSQL `JSONField` and renamed it to `dns` for simplicity
  * An important milestone/change for the upcoming GraphQL API
  * This change increases reliability and performance by removing any need to transform a string representation back into a `dict`
  * This field was always intended to be edited only by the server, so this change should not require any actions before or after upgrading
  * If an existing record's DNS data cannot be converted to JSON it will be cleared and user's can re-run the DNS update task
* Added a "sticky" sidebar tracker to user sessions so the sidebar will stay open or closed between visits and page changes
* Removed the legacy `health_dns` field from the `Domain` model
  * This field was part of the original Shepherd project and was an interesting experiment in using passive DNS monitoring to try to determine if a domain was "burned"
  * It became mostly irrelevant when services that supported this feature (e.g., eSentire's Cymon) were retired
* Changed some code that will be deprecated in future versions of Django v4.x and Python Faker
* Made it possible to sort the report template list
  * Sorting on this table is reversed so clicking "Status" once will sort templates with passing linter checks first
* Updated the admin panel to make it easier to manage domains for those who prefer the admin panel
* Some general code clean-up for maintainability
* See v2.2.3-rc1

## Security Changes

* Updated Django to v3.2.11 as v3.1 is no longer supported and considered "insecure" going forward
* Fixed unauthenticated access to domain and server library exports
* See v2.2.3-rc1

###
