---
description: Description and explanation of common API actions
---

# Common API Actions

## Getting to Know the API Schema

The Hasura Console (see [Using the Hasura Console](using-the-hasura-console.md)) is the easiest option for browsing the GraphQL schema. It's built into Ghostwriter, offers to autocomplete queries and mutations, and makes suggestions based on the schema, so it's very friendly for people learning how GraphQL queries work.

However, the Hasura Console should not be made widely accessible to anyone except the server administrator.&#x20;

For a safer option that offers all users access to something like the console, you can use an API testing/browsing application like Postman. Import the GraphQL SDL from the _schema.graphql_ in the code repository and configure your personal API token.

For example, using Postman:

1. Click _New_ and add a new API
2. Give it a name (e.g., Ghostwriter) and set the schema type to GraphQL
3. Save it and then open your newly created API
4. Click the _Definition_ tab and paste in the contents of the _schema.graphql_ file
5. Click _Save_ and wait a bit for Postman to process the definitions

Back in the _Collections_ tab, you can create new requests with the newly created API. Select GraphQL as the format for the body and then select the API you just created from the dropdown list. Postman should auto-fetch the schema to enable auto-complete.

![Atocomplete in the Postman Application](<../../.gitbook/assets/image (32).png>)

Now you can explore the API and get comfortable before trying to convert queries and mutations into code for automation.

### Basic Query and Mutations

Most queries and mutations are straightforward and follow a naming convention that will become familiar. You can expect the following to be consistent:

* To query for something, you need only to name it (e.g., `domain` to query domains)
* To create a new record, you will use a mutation that begins with `insert_*`
* To update an existing record, you will use a mutation that begins with `update_*`
* To delete a record, you will use a mutation that begins with `delete_*`

You will also notice copies of queries and mutations with the `*_by_pk` suffix. These are handy when you want to act against a singular entry and write a simpler query. Instead of writing a `_where` clause that filters the results by the `id` you want, these queries and mutations need only an `id` argument.

Both of these queries return the same result:

```
query DomainByPk {
  domain_by_pk(id: 1) {
    name
  }
}

query Domain {
  domain(where: {id: {_eq: "1"}}) {
    name
  }
}
```

### Special Queries and Mutations

There are some queries and mutations that have to break away from the above rules. These are special actions that trigger additional business logic before or after the action is completed.

#### Domain and Server Checkouts

The API uses `checkoutDomain` and `checkoutServer` mutations to prevent overlapping checkouts, attempts to checkout unavailable resources, and issues with bad dates. If a checkout passes all the checks, these actions also update the resource being checked out to mark them as unavailable.

Likewise, there are matching `deleteDomainCheckout` and `deleteServerCheckout` mutations. These mutations set the domain or server back to the "available" status, as needed.

#### Managing Evidence and Report Template Files

If deleting evidence uploads or report template files via the API, you will want to use the `deleteEvidence` and `deleteTemplate` mutations. These actions delete the database records and the files stored on the server's filesystem.

#### Generating Reports

You can use the `generateReport` mutation to get report data for a given report ID. The results offer the download URLs for docx, xlsx, and pptx. You can also request `reportData`, which is the raw JSON report data encoded as base64.

```
mutation GenerateReport {
  generateReport(id: 1) {
    docxUrl
    pptxUrl
    xlsxUrl
    reportData
  }
}
```
