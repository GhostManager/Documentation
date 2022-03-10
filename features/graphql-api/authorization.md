# Authorization

{% hint style="success" %}
This page does not necessarily reflect the permissions set in Hasura. Consider this information to be a draft or proposal of what is to come. Hasura's metadata can be easily changed for testing.

To adjust permissions, visit [http://127.0.0.1:8080/console/data/default/schema/public/permissions](http://127.0.0.1:8080/console/data/default/schema/public/permissions) and adjust permissions as desired for experimentation.
{% endhint %}

## Introduction

User roles are the primary authorization mechanism. There are three user roles:

* User
* Manager
* Admin

A user's role is set by a Ghostwriter administrator in the admin panel. All accounts are assigned the `user` role by default.

{% hint style="info" %}
If you look in Hasura you will also see a `public` role. This role is only used by Hasura. An unauthenticated request (i.e., any request that lacks a valid JWT in an `Authorization` header) is considered to have the `public` role. Certain webhook endpoints (e.g., `login`) are accessible by this role.
{% endhint %}

The roles carry the following privileges:

### User Role

The `user` role can only access client and project data if they:

* Have been assigned to the project
* Have been granted access to the client
* Have been granted access to the project

Otherwise, this role has the standard permissions you might expect. They can edit or delete their own comments, update their personal profiles, and view the shared information in the various libraries (e.g., findings, domains).

### Manager Role

The `manager` role has the ability to view all clients and projects and invite other users to view a client or project. This role can also edit report templates flagged as protected.

If an account is flagged as a Django _Superuser_ that account will automatically inherit the `manager` role.

### Admin Role

The `admin` role has complete access to everything available via the API. This role is even capable of creating and managing users and modifying fields not exposed to other roles.

{% hint style="danger" %}
Use great care when assigning this role to an account or acting as an administrator with the Hasura admin secret and `x-hasura-admin-secret` header.
{% endhint %}

## Roles in Tokens

The user's JSON Web Token (JWT) contains the user's allowed roles in two locations.

### X-Hasura-Default-Role

This claim includes the user's role set on their profile. A Ghostwriter administrator can change an account's role in the admin panel.

### X-Hasura-Allowed-Roles

This claim is a list that may contain additional roles allowed for the user and token. Ghostwriter will add roles to this list during authentication.

For example, if a user is flagged as a Django _Superuser_, tokens for that account will also include the `manager` role even if the user's role is not `manager`.
