# 15 September 2021, v2.2.2-rc1

## v2.2.2-rc1

{% embed url="https://github.com/GhostManager/Ghostwriter/releases/tag/v2.2.2-rc1" %}

This is a release candidate \(RC\) for v2.2.2. This release contains mostly optimizations and unit testing, but there are several impactful bug fixes and changes.

### New Features

* None

### Fixed

* Fixed issue with the JavaScript for deleting entries in a formset selecting other checkboxes
* Fixed `WhoisStatus` model's `count` property

  Upgraded TinyMCE to v5.8.2 to address potential XSS discovered in older TinyMCE versions

* Fixed error handling that could suppress report generation error messages when generating all reports
* Fixed error that could lead to WebSocket disconnections and errors when editing the timestamp values of a log entry
* Fixed a typo in the emoji used by the default Slack message for an untracked server
* Fixed a logic issue that could result in an "ignore tag" being missed when reporting on cloud infrastructure

### Changed

* Adjusted the report data to replace a blank short name with the client's full name \(rather than a blank space in a report\)
* Moved some form validation logic to Django Signals in preparation for the API
* Added a custom "division by zero" error message for times when a Jinja2 template attempts to divide a value \(e.g., total num of completed objectives\) that is zero without first checking the value
* Bumped Toastr message opacity to `.9` \(up from `.8`\) to improve readability
* Bumped 50 character limit on certain `OplogEntry` values to 255 \(the standard for other models\)
* Condensed Docker image layers and disabled caching for `pip` and `apk` to reduce image sizes by about 0.2 to 0.3GB
* Optimized and improved code quality throughout the project based on recommendations from Code Factor \([https://www.codefactor.io/repository/github/ghostmanager/ghostwriter](https://www.codefactor.io/repository/github/ghostmanager/ghostwriter)\)

### Security Changes

* Upgraded the Django image to Alpine v3.14 to address potential security vulnerabilities in the base image
* Upgraded Postgres image to Postgres v11.12 to address potential security vulnerabilities in previously used version/base image
* Pinned nginx image to v1.12.1 for security and stability

