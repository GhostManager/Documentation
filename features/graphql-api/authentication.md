# Authentication

## Introduction

Authentication is handled via JSON Web Tokens (JWT). There are two methods for retrieving a JWT:

* Submit credentials as described below and receive a JWT that is capable of interacting with Ghostwriter as the authenticated user
* Login via the web interface and generate a JWT with a different scope to use as an API token for automation **(Not yet an option)**

The JWT secret key is defined in the environment variables, `HASURA_GRAPHQL_JWT_SECRET`.  If you plug a Ghostwriter JWT into a debugger like the one at [https://jwt.io/](https://jwt.io), you will see something similar to the following:

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
    "x-hasura-allowed-roles": [
      "user",
      "manager"
    ],
    "x-hasura-default-role": "user",
    "x-hasura-user-id": "1",
    "x-hasura-user-name": "benny"
  }
}
```
{% endcode %}

{% code title="SIGNATURE" %}
```json
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
{% endcode %}

The tokens follow the JWT open standard ([RFC7519](https://datatracker.ietf.org/doc/html/rfc7519)) and Hasura's guidance for the custom `https://hasura.io/jwt/claims` values. Hasura looks for values under this claim that start with `X-Hasura-` to use them for authorization and filling in certain fields.

These claims–specifically the roles–are covered in-depth under the [Authorization](authorization.md) section. For now, know that these claim values are derived from the authenticated user's profile.

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

{% hint style="info" %}
There will be the ability to use a refresh token with a longer expiry period to refresh an expired JWT without needing to provide credentials again. This will be available prior to the full release of v2.3.0.
{% endhint %}

### API Tokens

{% hint style="info" %}
This is not yet available for testing.
{% endhint %}

A key difference between API tokens and user tokens is they have user-defined expiration dates. They are intended to be used for long-running automation tasks, like operational logging. They do not expire until revoked.

Further, these tokens do not automatically inherit the requesting user's privileges. Users must define permissions for these tokens.

## Token Authentication

Requests authenticate with the `Authorization` header: `Authorization: Bearer TOKEN`

If the token is valid and the associated user account is authorized to perform the query or mutation you will receive a response with your requested data. There are unique responses for situations where the user is not authorized or the JWT fails verification.

An expired JWT will return a `JWTExpired` message:

```json
{
  "errors": [
    {
      "extensions": {
        "path": "$",
        "code": "invalid-jwt"
      },
      "message": "Could not verify JWT: JWTExpired"
    }
  ]
}
```

A bad token value will generate an error message like this one with a `JWSError`.  An error like this usually means the token is incomplete.

```json
{
  "errors": [
    {
      "extensions": {
        "path": "$",
        "code": "invalid-jwt"
      },
      "message": "Could not verify JWT: JWSError (JSONDecodeError \"Not valid base64url\")"
    }
  ]
}
```

An invalid JWT will return a `JWSError` with the `JWSInvalidSignature`. This means Hasura decoded the JWT's payload, but the signature did not match your configured key. Either the secret key changed, the JWT came from a different application, or someone is up to some funny business.

```json
{
  "errors": [
    {
      "extensions": {
        "path": "$",
        "code": "invalid-jwt"
      },
      "message": "Could not verify JWT: JWSError JWSInvalidSignature"
    }
  ]
}
```

Finally, you might see an error code like `jwt-invalid-claims` after a successful decode and verification. This means the JWT decoded and verified, but did not include the necessary claims. If you are generating JWTs externally, the tokens must match the example included on this page.

```json
{
  "errors": [
    {
      "extensions": {
        "path": "$",
        "code": "jwt-invalid-claims"
      },
      "message": "claims key: 'https://hasura.io/jwt/claims' not found"
    }
  ]
}
```
