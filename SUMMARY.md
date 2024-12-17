# Table of contents

* [Welcome to Ghostwriter](README.md)

## Getting Started

* [Quickstart](getting-started/quickstart.md)
* [Updating Ghostwriter](getting-started/updating-ghostwriter.md)
* [Managing the Server with Ghostwriter CLI](getting-started/managing-the-server-with-ghostwriter-cli/README.md)
  * [Managing Logon Sessions](getting-started/managing-the-server-with-ghostwriter-cli/managing-logon-sessions.md)
  * [Creating and Managing Backups](getting-started/managing-the-server-with-ghostwriter-cli/creating-and-managing-backups.md)

## Configuring Global Settings

* [Introduction to Command Center](configuring-global-settings/introduction-to-command-center.md)
* [Personalizing Company Information](configuring-global-settings/personalizing-company-information.md)
* [Configuring Global Report Options](configuring-global-settings/configuring-global-report-options.md)
* [Configuring APIs](configuring-global-settings/configuring-apis/README.md)
  * [Configuring Slack](configuring-global-settings/configuring-apis/configuring-slack.md)
  * [Configuring VirusTotal](configuring-global-settings/configuring-apis/configuring-virustotal.md)
  * [Configuring Namecheap](configuring-global-settings/configuring-apis/configuring-namecheap.md)
  * [Configuring Cloud Services](configuring-global-settings/configuring-apis/configuring-cloud-services.md)
* [Configuring Extra Fields](configuring-global-settings/configuring-extra-fields.md)

## Features

* [Access, Authentication, & Session Controls](features/access-authentication-and-session-controls/README.md)
  * [Single Sign-On](features/access-authentication-and-session-controls/single-sign-on.md)
  * [Two-Factor Authentication](features/access-authentication-and-session-controls/two-factor-authentication.md)
  * [Role-Based Access Controls](features/access-authentication-and-session-controls/role-based-access-controls.md)
  * [Session Management](features/access-authentication-and-session-controls/session-management.md)
* [Client & Project Management](features/client-and-project-management/README.md)
  * [Client Dashboard](features/client-and-project-management/client-dashboard.md)
  * [Project Dashboard](features/client-and-project-management/project-dashboard/README.md)
    * [Project Points of Contact & Assignments](features/client-and-project-management/project-dashboard/project-points-of-contact-and-assignments.md)
    * [Scope Tracking](features/client-and-project-management/project-dashboard/scope-tracking.md)
    * [Objective Tracking](features/client-and-project-management/project-dashboard/objective-tracking.md)
    * [Deconfliction Tracking](features/client-and-project-management/project-dashboard/deconfliction-tracking.md)
    * [White Card Tracking](features/client-and-project-management/project-dashboard/white-card-tracking.md)
* [Findings Library](features/findings-library/README.md)
  * [Populating the Findings Library](features/findings-library/populating-the-findings-library.md)
  * [Template Values for Findings](features/findings-library/finding-keywords.md)
* [Observations Library](features/observations-library.md)
* [Infrastructure Management](features/infrastructure-management/README.md)
  * [Server Management](features/infrastructure-management/server-management/README.md)
    * [Populating the Server Library](features/infrastructure-management/server-management/populating-the-server-library.md)
    * [Server Checkout](features/infrastructure-management/server-management/server-management.md)
    * [Monitoring Servers](features/infrastructure-management/server-management/monitoring-servers.md)
  * [Domains Management](features/infrastructure-management/domains-management/README.md)
    * [Populating the Domain Library](features/infrastructure-management/domains-management/populating-the-domain-library.md)
    * [Domain Checkout](features/infrastructure-management/domains-management/domain-checkout-1.md)
    * [Monitoring Domains](features/infrastructure-management/domains-management/monitoring-domains.md)
* [Operation Logs](features/operation-logs/README.md)
  * [Creating a New Operation Log](features/operation-logs/creating-a-new-oplog.md)
  * [Interacting with the Operation Log Table](features/operation-logs/create-a-new-entry.md)
  * [Setting up Automated Logging](features/operation-logs/setting-up-automated-logging.md)
  * [Exporting / Importing Operation Logs](features/operation-logs/exporting-importing-oplogs.md)
* [Reporting](features/reporting/README.md)
  * [Report Types](features/reporting/report-types/README.md)
    * [Word Document Customization](features/reporting/report-types/word-document-customization.md)
    * [PowerPoint Deck Customization](features/reporting/report-types/powerpoint-deck-customization.md)
    * [Excel Spreadsheet Customization](features/reporting/report-types/excel-spreadsheet-customization.md)
  * [Report Templates](features/reporting/report-templates/README.md)
    * [Report Template Linting](features/reporting/report-templates/report-template-linting.md)
    * [Word Template Variables & Filters](features/reporting/report-templates/word-template-variables.md)
    * [Word Template Styles](features/reporting/report-templates/word-template-styles.md)
    * [Troubleshooting Word Templates](features/reporting/report-templates/troubleshooting-word-templates.md)
  * [Templating and Rich Text Fields](features/reporting/templating-and-rich-text-fields.md)
  * [Jinja2 Tips & Tricks](features/reporting/jinja2-tips-and-tricks.md)
* [Background Tasks](features/background-tasks/README.md)
  * [Scheduling Tasks](features/background-tasks/scheduled-tasks.md)
  * [Prebuilt Tasks](features/background-tasks/prebuilt-tasks.md)
* [GraphQL API](features/graphql-api/README.md)
  * [Authentication](features/graphql-api/authentication.md)
  * [Common API Actions](features/graphql-api/common-api-actions.md)
  * [Using the Hasura Console](features/graphql-api/using-the-hasura-console.md)
  * [GraphQL Usage Examples](features/graphql-api/graphql-usage-examples/README.md)
    * [Recording Cloud Server Deployments](features/graphql-api/graphql-usage-examples/recording-cloud-server-deployments.md)
    * [Integrating with BloodHound for Reporting](features/graphql-api/graphql-usage-examples/integrating-with-bloodhound-for-reporting.md)
* [Health Monitoring](features/health-monitoring.md)

## Getting Help

* [FAQ](getting-help/faq.md)
* [Reporting a Problem](getting-help/getting-help-with-a-problem.md)
* [Reporting a Security Issue](getting-help/reporting-a-security-issue.md)

## Workflow & Usage <a href="#workflow" id="workflow"></a>

* [The Workflow](workflow/basic-usage/README.md)
  * [Finding Edit & Review Workflow](workflow/basic-usage/finding-review-workflow.md)

## Coding Style Guide

* [Basic Formatting](coding-style-guide/style-guide.md)
* [Form Layouts & Design](coding-style-guide/form-layouts-and-design/README.md)
  * [Formset Layout & Design](coding-style-guide/form-layouts-and-design/formset-layout-and-design.md)
  * [Custom Layout Objects](coding-style-guide/form-layouts-and-design/custom-layout-objects.md)

## Development

* [Stack Overview](development/stack-overview/README.md)
  * [Behind the Scenes](development/stack-overview/behind-the-scenes.md)
* [Modifying Code](development/modifying-code/README.md)
  * [Modifying Environment Variables](development/modifying-code/modifying-environment-variables.md)
* [Testing Code](development/testing-code.md)
* [Contributing to the Project](development/contributing-to-the-project/README.md)
  * [Pre-Release Checklist](development/contributing-to-the-project/pre-release-checklist.md)
* [Database Models](development/database-models/README.md)
  * [API Models](development/database-models/api-models.md)
  * [Client & Project Models](development/database-models/rolodex-models.md)
  * [Configuration Models](development/database-models/configuration-models.md)
  * [Home & User Models](development/database-models/home-models.md)
  * [Infrastructure Models](development/database-models/infrastructure-models.md)
  * [Oplog Models](development/database-models/oplog-models.md)
  * [Reporting Models](development/database-models/reporting-models.md)
* [Expected Services & Processes](development/expected-services-and-processes.md)
