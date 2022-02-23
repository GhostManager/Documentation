---
description: Documentation for the GraphQL API
---

# GraphQL API - ALPHA

{% hint style="warning" %}
The GraphQL API is currently available with the v2.3.0 ALPHA releases. Until the final release, it should be used for testing purposes only.

This documentation is a work in progress and published for visbiility and feedback,
{% endhint %}

## Introduction

Starting in v2.3.0, Ghostwriter includes a Docker container for [Hasura](https://hasura.io) used to manage access to the back-end PostgreSQL database via GraphQL. Hasura is invisible unless console access is enabled.

If the `HASURA_GRAPHQL_ENABLE_CONSOLE` environment variable is set to `true`, `t`, `yes`, or `y`, the Hasura console is available on port 8080. The console is where administrators can manage role-based access controls and other configurations.

{% hint style="danger" %}
Hasura is connected **directly** to the PostgreSQL database! Changes made in the Hasura console take immediate effect. Changing the schema or deleting data will irreversibly change your database and could render Ghostwriter unusable.

In most situations, administrators should leave configurations alone and only use the console for experimenting with the GraphQL requests and possibly adjusting user role permissions.

Once done, disable the console access to better protect against unauthorized access to your database.
{% endhint %}

## Interacting with the API

The Nginx server acts as a proxy for all GraphQL traffic, so users do not need to submit queries to a different port or subdomain. With the default configuration, the GraphQL endpoints are:

* Local: [http://127.0.0.1:8000/graphql](http://127.0.0.1:8000/graphql)
* Production: [https://domain/graphql](http://127.0.0.1:8000/graphql)

Unlike a REST API, a GraphQL API does not have specific endpoints you must query with a particular HTTP request type to receive a predetermined set of results. You submit queries with POST requests to one of the above endpoints as JSON. The JSON includes your personalized query and the data you selected to get back. That means you can get exactly what you need without making multiple requests or parsing extra data.

A standard query is submitted with `Content-Type: application/json` in the headers and a body like this:

```json
{
    "query": "...",
    "operationName": "...",
    "variables": { "foo": "bar", ... }
}
```

Only the `query` key is required. The other keys need only be provided in certain situations. Different query scenarios are covered in the following sections.

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

You can include multiple unrelated queries in a single request. Here we add a query to fetch the `id` and `title` of every finding in the database to get all our data back in a single request:

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

Finally, you might want to try to take the result of one query and use it as a variable for a subsequent query. When GraphQL receives multiple queries, like in the above example, GraphQL resolves all queries simultaneously, so the results of one cannot feed into another.

In most cases, you can accomplish your goal with a single query. Always remember, you are crafting database queries, so you can leverage database relationships.

For this final example, assume you want to get the title and severity of every finding ever associated with a particular client's projects where the `title` contains `SMB`. This can be accomplished with nested database relationships and the addition of a condition:

```
query MyQuery {
  clients {
    projects {
      reports {
        localfindings(where: {title: {_like: "%SMB%"}}) {
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

{% hint style="info" %}
Note how the above example references the `severity` relationship, instead of returning the findings `severity_id` field. The `severity_id` is just the foreign key, an integer. The query uses the relationship to return the string value set to represent that severity (e.g., High).
{% endhint %}

### Interacting via Automation

Queries are simple until you need to pack them into the nested JSON for a web request. You should use a script to craft the proper payloads and make them repeatable.

You can write your query in a human-readable format and then use something like JavaScript's `JSON.stringify()` or Python;'s `json.dumps()` to create the properly formatted payload for the POST request.

Here is an example authentication request in Python:

```python
import json
import requests

headers = {"Content-Type": "application/json"}

query = \
{
  "query":
    """mutation TokenAuth {
      tokenAuth(username: "benny", password: "password") {
        token
        payload
        refreshExpiresIn
        refreshToken
      }
    }""",
  "operationName": "TokenAuth"
}

data = json.dumps(query)

req = requests.post("http://127.0.0.1:8000/graphql", headers=headers, data=data)

```

