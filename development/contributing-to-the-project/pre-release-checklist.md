---
description: Step-by-step instructions for preparing a release or PR
---

# Pre-Release Checklist

## Preparing New Code for a Release

So, you want to release some code? The Ghostwriter release workflow is simple to follow. The developer of the new code must perform a few actions:

* Update or create unit tests that cover their changes to the codebase
* Update documentation (this wiki), as needed
* Write an itemized CHANGELOG of their changes

Everything else is handled automatically by the continuous integration pipeline.

The following sections walk you through what to do to prepare for a new release or a pull request.

### Prepare a Release or Pull Request

1. Review any linter alerts in VS Code (under the _PROBLEMS_ tab in the console)
   1. See the [Code Style Guide](../../coding-style-guide/style-guide.md) for information about linting and proper formatting
2. Review and update wiki documentation as needed
   1. The Ghostwriter team should update the wiki immediately via a commit or edit in GitBook
   2. External contributions should submit changes as a pull request to the [documentation repository](https://github.com/GhostManager/Documentation)
3. Write or update unit tests for changes
4. Run all unit tests with Python _Coverage_: `docker-compose -f local.yml run django coverage run manage.py test`
5. Review Coverage report for changes in "missing"
   1. Run a report with "missing" displayed: `docker-compose -f local.yml run django coverage report -m`
   2. Look for code branches not covered by unit tests (e.g., `except` blocks)
6. If _Coverage_ reports testing gaps in new or changed code, return to step 3

### Ready to Conviser a Release or Pull Request

1. Merge feature branches into a `dev` branch
2. Test deployment of the new branch on a development server
3. Test new and changed features and anything potentially affected by the changes
4. If all is well, merge into `main` or create the pull request with an itemized _CHANGELOG_
5. Wait some time (usually \~25 minutes) and review the results of the GitHub Actions
6. If required Actions succeeded, the code may be merged and Ghostwriter team members will review the code prior to a release
7. If code is approved, the Ghostwriter team will update the [Change Logs](broken-reference) section, tag a release on GitHub, and link the new Change Logs page
