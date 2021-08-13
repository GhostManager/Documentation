---
description: Developing and running unit tests
---

# Testing Code

## Introducing Test Cases

Ghostwriter follows Django's best practices and recommendations for unit testing. You can read more about Django unit tests here:

{% embed url="https://docs.djangoproject.com/en/3.2/topics/testing/overview/" %}

The project organizes unit tests by application. Each application \(e.g., Rolodex, Shepherd\) has a _tests_ folder that contains scripts for testing forms, views, and models. Each type of test case includes a baseline of unit tests that are detailed below.

## Running Tests & Examining Coverage

Tests are run through Django's _manage.py_ and the `test` command. You can run all tests, a subset of tests, or individual tests. See below for examples:

```bash
# Run all tests
docker-compose -f local.yml run django coverage run manage.py test

# Run only "Rolodex" tests
docker-compose -f local.yml run django coverage run manage.py test ghostwriter.rolodex.tests

# Run a specific test
docker-compose -f local.yml run django coverage run manage.py test ghostwriter.rolodex.tests.test_models.ClientModelTests
```

A successful run of all unit tests may still display errors. Many of the unit tests intentionally trigger errors by passing invalid data to the server. The logger output can be disabled but this is generally unnecessary. A successful run will output something like this at the end:

```text
----------------------------------------------------------------------
Ran 488 tests in 45.331s

OK
Destroying test database for alias 'default'...
```

A test run with failures or errors will report the number of each at the end. Review the test output to see which test\(s\) failed to determine what needs to be fixed.

### Test Coverage

The above commands include the usage of Python's _coverage_ library. Coverage compares the executed tests against the codebase to identify lines of code that were not tested.

{% embed url="https://coverage.readthedocs.io/en/coverage-5.5/" %}

A Coverage report can be generated once a test run is complete. This command will generate a command line report that displays the lines that were missed during testing:

`docker-compose -f local.yml run django coverage report -m`

A GitHub Action executes unit tests, generates an XML Coverage report, and uploads the report to CodeCov. This Action fires after commits to the `master` branch and whenever a PR is submitted to the repository. CodeCov tracks unit test coverage and makes it easier to view which folders, files, and individual lines of code need more unit testing.

{% embed url="https://app.codecov.io/gh/ghostmanager/Ghostwriter" %}







