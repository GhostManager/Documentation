---
description: Explanation of Ghostwriter's role-based access controls
---

# Authorization

{% hint style="success" %}
The role-based access controls described on this page apply _only to the GraphQL API_. These controls will apply to the web interface in a future version.
{% endhint %}

## Introduction

User roles are the primary authorization mechanism. There are three user roles:

* User
* Manager
* Admin

A user's role is set by a Ghostwriter administrator in the admin panel. All accounts are assigned the `user` role by default.

{% hint style="info" %}
If you look in Hasura you will also see a `public` role. This role is only used by Hasura. An unauthenticated request (i.e., any request that lacks a valid JWT in an `Authorization` header) is considered to have the `public` role. Certain webhook endpoints (e.g., `login`) are accessible to this role.
{% endhint %}

The roles carry the following privileges:

### User Role Privileges

The `user` role can only access client and project data if they:

* Have been assigned to the project
* Have been invited to access the client
* Have been invited to access the project

Otherwise, this role has the standard permissions you might expect. They can edit or delete their own comments, update their personal profiles, and view the shared information in the various libraries (e.g., findings, domains).

### Manager Role Privileges

The `manager` role has the ability to view all clients and projects. The role can also:

* Invite others to access client data
* Invite others to access project data
* Assign others to a project
* Edit report templates flagged as _protected_

If an account is flagged as a Django _Superuser_ that account will automatically inherit the `manager` role.

{% hint style="info" %}
In v3.0.0 web interface, client and project invitations must be created via the admin panel (_/admin/rolodex/clientinvite/_ and _/admin/rolodex/projecttinvite/_). Access to the admin panel requires an account with `superuser` status.

A future release will add an option to invite users from the client and project pages for the `manager` role.
{% endhint %}

### Admin Role Privileges

The `admin` role is only used by the GraphQL API. This role has complete access to everything available via the API. This role is even capable of creating and managing users and modifying fields not exposed to other roles.

{% hint style="danger" %}
Use great care when assigning this role to an account. In general, a user should only ever be a `manager` or a `user`.
{% endhint %}
