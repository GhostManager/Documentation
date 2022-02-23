# Authentication

## Introduction

Authentication is handled via JSON Web Tokens (JWT). There are two methods for retrieving a JWT:

* Submit credentials as described below and receive a JWT that is capable of interacting with Ghostwriter as the authenticated user
* Login via the web interface and generate a JWT to use as an API token with restrictions for automation

### User Tokens

User JWTs are generated with a `TokenAuth` mutation, a type of GraphQL query that performs a server-side action. The resulting JWT holds the same privileges as the authenticated user.

```
{
    "query":"mutation TokenAuth { tokenAuth(username: \"USERNAME\", password: \"PASSWORD\") { token payload refreshExpiresIn refreshToken } }",
    "operationName": "TokenAuth"
}
```

{% swagger method="post" path="/graphql" baseUrl="https://ghostwriter" summary="Token Authentication" %}
{% swagger-description %}
Authenticate with a user account and receive a JWT, refresh token, and expiration date.
{% endswagger-description %}

{% swagger-parameter in="body" name="query" required="true" %}
"mutation TokenAuth { tokenAuth(username: \\"USERNAME\\", password: \\"PASSWORD\\") { token payload refreshExpiresIn refreshToken } }",
{% endswagger-parameter %}

{% swagger-parameter in="body" name="operationName" required="true" %}
TokenAuth
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "data": {
        "tokenAuth": {
            "token": "eyJ0eXAiOiJKV1...",
            "payload": {
                "username": "benny",
                "sub": "1",
                "sub_name": "benny",
                "sub_email": "benny@getghostwriter.io",
                "exp": 1645567282,
                "https://hasura.io/jwt/claims": {
                    "x-hasura-allowed-roles": [
                        "user"
                    ],
                    "x-hasura-default-role": "user",
                    "x-hasura-user-id": "1"
                }
            },
            "refreshExpiresIn": 1646170282,
            "refreshToken": "b3108bf8499888c..."
        }
    }
}
```
{% endswagger-response %}
{% endswagger %}

The response includes the token that can be used with the `Authorization` header of future requests.

By default, the user JWTs are valid for 30 minutes. This configuration is changeable in Ghostwriter's _base.py_ configuration file. Update `JWT_EXPIRATION_DELTA` to a new time delta (e.g., `timedelta(hours=8)`).

The `payload` key includes information about the JWT, like the username and expiration epoch time.

The response also includes a `refreshToken` value that is valid for seven (7) days. This token can be used to refresh a login session after it expires. The response includes a new JWT.

{% swagger method="post" path="/graphql" baseUrl="" summary="Token Refresh" %}
{% swagger-description %}
Refresh the expiration time on a login session.
{% endswagger-description %}

{% swagger-parameter in="body" name="query" required="true" %}
"mutation refreshToken { refreshToken(refreshToken: \\"TOKEN\\") { token } }"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="operatonName" %}
RefreshToken
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "data": {
        "refreshToken": {
            "token": "eyJ0eXAiOiJKV1Q..."
        }
    }
}
```
{% endswagger-response %}
{% endswagger %}

### API Tokens

A key difference between API tokens and user tokens is they have user-defined expiration dates. They are intended to be used for long-running automation tasks, like operational logging.

Further, these tokens do not automatically inherit the requesting user's privileges. Users must define permissions for these tokens.

**TODO**

## Token Authentication

Requests authenticate with the `Authorization` header: `Authorization: Bearer TOKEN`

You can the `whoami` query to check your login and verify account details. The query can return standrad user profile data, like username and timezone. This example returns the requesting user's username.

```
{
    "query":"query whoami {whoami {username}}",
    "operationName": "whoami"
}
```

&#x20;
