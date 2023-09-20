---
description: Configuring the API endpoint for automatic activity logging
---

# Setting up Automated Logging

Currently, two different C2 frameworks can easily integrate with Ghostwriter's GraphQL API: Mythic and Cobalt Strike. These utilities automatically create and update log entries.

You can also write scripts to integrate other frameworks and tools. All you need to get started if an API token.

## Obtaining an API Token

To get started logging you need an API token. To use the utilities mentioned below you will want to generate an API token with an expiration date. For custom logging tools, you can consider using the `login` action with the API.

Read more about this process here:

{% content-ref url="../graphql-api/authentication.md" %}
[authentication.md](../graphql-api/authentication.md)
{% endcontent-ref %}

## Setting up Syncing with Cobalt Strike

{% embed url="https://github.com/GhostManager/cobalt_sync" %}

Clone the [cobalt\_sync project](https://github.com/GhostManager/cobalt\_sync) to your Cobalt Strike team server and follow the instructions contained in the [README](https://github.com/GhostManager/cobalt\_sync/blob/main/README.md) to enable syncing for each Cobalt Strike team server you deploy.

{% hint style="info" %}
**Note**: Cobalt Strike does not associate console output with the original command. Therefore, _cobalt\_sync_ cannot automatically complete the output fields for log entries. Job IDs may be available for CObalt Strike in the future.
{% endhint %}

## Setting up Syncing with Mythic

{% embed url="https://github.com/GhostManager/mythic_sync" %}

Clone the [mythic\_sync project](https://github.com/GhostManager/mythic\_sync) to your Mythic C2 server and follow the instructions contained in the README to enable syncing for each Mythic server you deploy.

{% hint style="info" %}
**Note**: Since Mythic associates output with the original command, the _mythic\_sync_ project will retroactively update previous log entries when output is received. _This will overwrite any additional context added to the original entry within Ghostwriter before the new output was received._
{% endhint %}

