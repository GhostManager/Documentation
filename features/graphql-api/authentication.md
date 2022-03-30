# Authentication

## Introduction

Authentication is handled via JSON Web Tokens (JWT). There are two methods for retrieving a JWT:

* Submit credentials as described below and receive a JWT that is capable of interacting with Ghostwriter as the authenticated user
* Login via the web interface and generate a JWT with your preferred expiration date to use as an API token for automation **(Not yet an option)**

The JWT secret key is defined in the environment variables, `GRAPHQL_JWT_SECRET_KEY`.  If you plug a Ghostwriter JWT into a debugger like the one at [https://jwt.io/](https://jwt.io), you will see something similar to the following:

{% code title="Header" %}
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
{% endcode %}

{% code title="PAYLOAD" %}
```json
{
  "username": "benny",
  "sub": "1",
  "sub_name": "benny",
  "sub_email": "benny@ghostwriter.wiki",
  "aud": "Ghostwriter",
  "iat": 1646088460,
  "exp": 1646117260,
  "https://hasura.io/jwt/claims": {
    "X-Hasura-Role": "user",
    "X-Hasura-User-Id": "1",
    "X-Hasura-User-Name": "benny"
  }
}
```
{% endcode %}

{% code title="SIGNATURE" %}
```json
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
{% endcode %}

The tokens follow the JWT open standard ([RFC7519](https://datatracker.ietf.org/doc/html/rfc7519)) and Hasura's guidance for the custom `https://hasura.io/jwt/claims` values. Hasura looks for values under this claim that start with `X-Hasura-` using them for authorization and filling in certain fields.

These claim values are derived from the authenticated user's profile. These `X-Hasura-Role` claim is the most important for authorization and is covered in-depth under the [Authorization](authorization.md) section.

When performing an action like creating a new log entry or project note, Hasura uses the `X-Hasura-User-Id` and `X-Hasura-User-Name` values to fill certain fields.

### User Tokens

User JWTs are generated with a `login` mutation, a type of GraphQL query that performs a server-side action. The resulting JWT holds the same privileges as the authenticated user.

```
mutation Login {
  login(password: "password", username: "username") {
    token
    expires
  }
}
```

The response includes the token that can be used with the `Authorization` header of future requests.

By default, the user JWTs are valid for 15 minutes. This configuration is changeable in Ghostwriter's _base.py_ configuration file. Update `JWT_EXPIRATION_DELTA` to a new time delta (e.g., `timedelta(hours=8)`).

### API Tokens

{% hint style="info" %}
This is not yet available for testing.
{% endhint %}

A key difference between API tokens and user tokens is they have user-defined expiration dates. They are intended to be used for long-running automation tasks, like operational logging. If an expiration date is not set they do not expire until revoked.

## Token Authentication

Requests authenticate with the `Authorization` header: `Authorization: Bearer TOKEN`

Hasura will connect to an authentication webhook before a request. The webhook takes several steps to thoroughly examine the JWT before allowing a request to proceed:

1. Check the JWT is present
2. Attempt to decode the JWT and verify the signature, audience, and expiration
3. Verify the JWT contains the proper claims
4. Finally, verify the user details are correct and the account is still active

If the token passes the above checks and your user's role is authorized (see [Authorization](authorization.md))to perform the query or mutation you will receive a `200 OK` response with your requested data.

If the token is not accepted the authorization webhook will return a `401 Unauthorized` response with an error like this:

```json
{
  "errors": [
    {
      "extensions": {
        "path": "$",
        "code": "access-denied"
      },
      "message": "Authentication hook unauthorized this request"
    }
  ]
}
```

Any unauthorized request will be treated as having the `public` role with the username `anonymous`. This is not a real user or role and is only used to manage access to resources designed to be accessed without authentication.

At this time the only action available for this `anonymous` user is the `Login` mutation.
