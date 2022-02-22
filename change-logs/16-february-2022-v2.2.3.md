# 16 February 2022, v2.2.3

## v2.2.3

{% embed url="https://github.com/GhostManager/Ghostwriter/releases/tag/v2.2.3" %}

This is the final release of v2.2.3. This release contains everything from the release candidates with the addition of some minor changes. This page contains a complete changelog from v2.2.3-rc1, v2.2.3-rc2, and v2.2.3.

## New Features

* Expanded user profiles for project management and planning
  * Now visible to all users under /users/
  * Include timezone and phone number fields -Users can now edit their profiles to update their preferred name, phone, timezone, and email address

## Fixed

* Fixed display of minutes for project working hours
* Fixed "incomplete file" issue when attempting to download a report template
* Fixed report archiving failing to write zip file
* Fixed toast messages not showing up when swapping report templates
* Fixed sidebar tab appearing below delete confirmations
* Fixed cloud server forms requiring users to fill in all auxiliary IP addresses
* Fixed project serialization issue that prevented project data from loading automatically for domain and server checkout forms
* Fixed active project filtering for the list in the sidebar so it will no longer contain some projects marked as completed
* Fixed a rare reporting error that could occur if the WYSIWYG editor created a block of nested HTML tags with no content
* Fixed ignore tags not working for Digital Ocean assets
* Fixed an error caused by cascading deletes when deleting a report under some circumstances
* Fixed template linter not recognizing phone numbers for project team members as valid (Fixes #190)
* Fixed a rare reporting issue related to nested lists that could occur if a nested list existed below an otherwise blank list item

## Changed

* Updated project list filtering
  * Added client name as a filter field
  * Changed default display filter to only show active projects
  * Adjusted project status filter to have three options: all projects, active projects, and completed projects
* Updated dashboard and calendar to show past and current events for browsing history within the calendar
  * Past events marked as completed will appear dimed with a strikethrough and `: Complete` added to the end
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
* Projects now sort in reverse so the most recent projects appear first
* Updated the report selection section of the sidebar to make it easier to switch reports when working on multiple and navigate to your current report
* The logging API key message now includes the project ID to make it easier to set up a tool like mythic\_sync
* Removed the "Upload Evidence" button from editors where it does not apply (e.g., in the Finding Library outside of a report) (Fixes #185)
* Updated the Namecheap sync task to use paging so Namecheap libraries with more than 100 domains can be fully synced (Fixes #188)
* Dashboard once again has a "Project Assignments" card to make it easier to see and click projects
  * The calendar remains on the dashboard and is still clickable, but some people found it less intuitive as a shortcut
* Some general code clean-up for maintainability

## Security Changes

* Updated Django to v3.2.11 as v3.1 is no longer supported and considered "insecure" going forward
* Fixed unauthenticated access to domain and server library exports
* Updated TinyMCE to v5.10.1 to address several moderate security issues with <5.10

###
