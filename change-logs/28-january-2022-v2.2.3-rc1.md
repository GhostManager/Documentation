# 28 January 2022, v2.2.3-rc1

## v2.2.3-rc1

{% embed url="https://github.com/GhostManager/Ghostwriter/releases/tag/v2.2.3-rc1" %}

This is the final release of v2.2.3-rc1. This release contains some small new features, fixes several minor to moderate issues related to v2.2.2, and addresses some feedback.

## New Features

* Expanded user profiles for project management and planning
  * Now visible to all users under /users/
  * Include timezone and phone number fields -Users can now edit their profiles to update their preferred name, phone, timezone, and email address

## Fixed

* Fixed cloud server forms requiring users to fill in all auxiliary IP addresses
* Fixed project serialization issue that prevented project data from loading automatically for domain and server checkout forms
* Fixed active project filtering for the list in the sidebar so it will no longer contain some projects marked as completed
* Fixed a rare reporting error that could occur if the WYSIWYG editor created a block of nested HTML tags with no content
* Fixed ignore tags not working for Digital Ocean assets
* Fixed an error caused by cascading deletes when deleting a report under some circumstances
* Fixed template linter not recognizing phone numbers for project team members as valid (Fixes #190)
* Fixed a rare reporting issue related to nested lists that could occur if a nested list existed below an otherwise blank list item

## Changed

* Projects now sort in reverse so the most recent projects appear first
* Updated the report selection section of the sidebar to make it easier to switch reports when working on multiple and navigate to your current report
* The logging API key message now includes the project ID to make it easier to set up a tool like mythic\_sync
* Removed the "Upload Evidence" button from editors where it does not apply (e.g., in the Finding Library outside of a report) (Fixes #185)
* Updated the Namecheap sync task to use paging so Namecheap libraries with more than 100 domains can be fully synced (Fixes #188)
* Dashboard once again has a "Project Assignments" card to make it easier to see and click projects
  * The calendar remains on the dashboard and is still clickable, but some people found it less intuitive as a shortcut
* Some general clean-up of CSS

## Security Changes

* Updated TinyMCE to v5.10.1 to address several moderate security issues with <5.10

###
