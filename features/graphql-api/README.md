---
description: Documentation for the GraphQL API
---

# GraphQL API

{% hint style="success" %}
There are no v2.3.x-alpha releases that are available today. Once available, this page will update and pre-release builds will be tagged on GitHub.
{% endhint %}

{% hint style="warning" %}
The GraphQL API is currently available with v2.3.x-alpha releases. Until the final release, it should be used for testing purposes only.

This documentation is a work in progress and published for visibility and feedback.
{% endhint %}

## Introduction

Starting in v2.3.0, Ghostwriter includes a Docker container for [Hasura](https://hasura.io) used to manage access to the back-end PostgreSQL database via GraphQL. Hasura is invisible unless console access is enabled.

If the `HASURA_GRAPHQL_ENABLE_CONSOLE` environment variable is set to `true`, `t`, `yes`, or `y`, the Hasura console is available on port 8080. The console is where administrators can manage role-based access controls and other configurations.

The console is useful for crafting queries and experimenting.

{% hint style="danger" %}
Hasura is connected **directly** to the PostgreSQL database! Changes made in the Hasura console take immediate effect. Changing the schema or deleting data will irreversibly change your database and could render Ghostwriter unusable.

In most situations, administrators should leave configurations alone and only use the console for experimenting with the GraphQL requests and possibly adjusting user role permissions.

Once done, disable the console access to better protect against unauthorized access to your database.
{% endhint %}

## Interacting with the API

With the default configuration, the GraphQL endpoints are:

* Local: [http://127.0.0.1:8080/v1/graphql](http://127.0.0.1:8000/graphql)
* Production: [https://domain/v1/graphql](http://127.0.0.1:8000/graphql)

In production, the nginx server acts as a proxy for all GraphQL traffic, so users do not need to submit queries to a different port or subdomain.

Unlike a REST API, a GraphQL API does not have specific endpoints you must query with a particular HTTP request type to receive a predetermined set of results. You submit queries with POST requests to one of the above endpoints as JSON. The JSON includes your personalized query and the data you selected to get back. That means you can get exactly what you need without making multiple requests or parsing extra data.

A standard query is submitted with `Content-Type: application/json` in the headers and a body like this:

```json
{
    "query": "...",
    "operationName": "...",
    "variables": { "foo": "bar", ... }
}
```

The `query` and `operationName` keys are required. The `variables` key is only provided in certain situations. Different query scenarios are covered in the following sections.

The response will always come back in this form:

```json
{
  "data": { ... },
  "errors": [ ... ]
}
```

For more information, review the GraphQL documentation on queries:

{% embed url="https://graphql.org/learn/queries" %}

{% hint style="success" %}
Some basic GraphQL knowledge, such as what the difference between a query and a mutation is, will make the following sections easier to understand. You will also be better prepared to write your own custom queries.
{% endhint %}

### Basic Queries

A basic query looks like this:

```
query MyQuery {
  client {
    id
  }
}

```

It identifies itself as a query with an arbitrary name, states which table it wants to query, and what fields it wants to be returned. This query would return the `id` field of all client records accessible to the requesting user.

The query can be modified to return additional data, like the `codename` and `note` fields:

```
query MyQuery {
  client {
    id
    codename
    note
  }
}

```

{% hint style="success" %}
Field names are often placed on their own lines in GraphQL examples and in the Hasura Console, but this is not required. You can separate field names with spaces, too. This option is easier to use when preparing queries for web requests because it removes the need to convert newlines to `\n`.
{% endhint %}

Queries can also request related data. For a client, you might request the contact information for all related points of contact:

```
query MyQuery {
  clients {
    id
    codename
    note
    contacts {
      email
    }
  }
}
```

You can include multiple queries in a single request. Here we add a query to fetch the `id` and `title` of every finding in the database to get all our data back in a single request:

```
query MyQuery {
  clients {
    id
    codename
    note
    contacts {
      email
    }
  }
  finding {
    id
    title
  }
}
```

Finally, you might want to try to take the result of one query and use it as a variable for a subsequent query. When GraphQL receives multiple queries, like in the above example, GraphQL resolves all queries **simultaneously**, so the results of one cannot feed into another.

In most cases, you can accomplish your goal with a single query. Always remember, you are crafting database queries, so you can leverage relationships tracked in Hasura.

For this final example, assume you want to get the title and severity of every finding ever associated with a particular client's projects where the `title` contains `SMB`. This can be accomplished with nested database relationships and the addition of a condition:

```
query MyQuery {
  clients {
    projects {
      reports {
        reportedFindings(where: {title: {_like: "%SMB%"}}) {
          title
          severity {
            severity
          }
        }
      }
    }
  }
}
```

{% hint style="success" %}
Note how the above example references the `severity` relationship, instead of returning the findings `severityId` field. The `severityId` is just the foreign key, an integer. The query uses the relationship to return the string value set to represent that severity (e.g., High).
{% endhint %}

### Interacting via Automation

Queries are simple until you need to pack them into the nested JSON for a web request. You should use a script to craft the proper payloads and make them repeatable.

{% hint style="info" %}
If you are using the Hasura console to build your query you can click the _Code Exporter_ button to view the query in a code snippet. Hasura generates code for JavaScript, TypeScript, and the Apollo Client right now, but the examples can be easily adapted to another language.
{% endhint %}

You can write your query in a human-readable format and then use something like JavaScript's `JSON.stringify()` or Python;'s `json.dumps()` to create the properly formatted payload for the POST request.

Here is an example query request in Python:

```python
import json
import requests


TOKEN = "JWT_VALUE"

headers = {
  "Content-Type": "application/json",
  "Authorization": f"Bearer {TOKEN}"
}
Pyth
query = \
{
  "query": """
  query getClient {
    client(where: {name: {_eq: "Murphy Group"}}) {
      id name note shortName timezone
    }
  }
  """,
  "operationName": "getClient"
}

req = requests.post(
  "http://127.0.0.1:8080/v1/graphql",
  headers=headers,
  data=json.dumps(query)
)

print(req.json())
```

