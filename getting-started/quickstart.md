---
description: >-
  Get started with Ghostwriter quickly and easily with Docker and Ghostwriter
  CLI
---

# Quickstart

{% hint style="danger" %}
**STOP!** Ghostwriter uses [Docker Compose](https://docs.docker.com/compose/). Install Docker before proceeding.

You will need a version >=20 for the Alpine Linux images used for Ghostwriter. Run `docker --version` to check your installation.
{% endhint %}

## Getting Started

Ghostwriter comes with the [Ghostwriter CLI](https://github.com/GhostManager/Ghostwriter\_CLI) tool. This tool makes it simple to manage the application. You will use it to install, reconfigure, and upgrade Ghostwriter.

Ghostwriter can run on Windows, macOS, and Linux, so there are multiple versions of Ghostwriter CLI. Pick the appropriate Ghostwriter CLI binary for your operating system.

* `ghostwriter-cli-macos` : macOS
* `ghostwriter-cli-linux` : Linux amd64
* `ghostwriter-cli.exe` : Windows 64-bit

{% hint style="success" %}
You can rename these binaries without causing any issues. This wiki will always refer to it simply as `ghostwriter-cli` for commands.
{% endhint %}

### Installing Ghostwriter

You can install Ghostwriter with three basic commands:

```
git clone https://github.com/GhostManager/Ghostwriter.git
cd Ghostwriter
./ghostwriter-cli install prod
```

{% hint style="info" %}
Ghostwriter will create self-signed TLS/SSL certificates. If you'd like to use your own signed certificates, do that now to make things easier. If you don't have them ready, you can always install them later.

There is more information below in [Customizing Your Installation](quickstart.md#customizing-your-installation).
{% endhint %}

The last command will take care of everything necessary to install a production (`prod`) environment for you. That command performs the following actions:

* Sets up the default server configuration
* Generates TLS certificates for the server
* Builds the Docker containers
* Creates a default _admin_ user with a randomly generated password

{% hint style="success" %}
If you'd prefer to install a development (`dev`) environment, you can run:&#x20;

`./ghostwriter-cli install dev`

A development environment is best if you want to change Ghostwriter's codebase or test functionality. Debug logging is enabled, which makes it easier to troubleshoot. The `dev` installation does not use TLS, so it skips creating certificates.
{% endhint %}

### Accessing Ghostwriter

The Ghostwriter server will now be accessible! Just visit _https://127.0.0.1_ and authenticate with the _admin_ user. The password is displayed in the Ghostwriter CLI output at the end of the installation.

You can also get the password by running this command:

`./ghostwriter-cli config get admin_password`

{% hint style="success" %}
This password is only used for creating the account. You can change it in the admin panel after logging into Ghostwriter. You can also change any other part of the user profile.
{% endhint %}

### Adding More Users

You may create users using the admin panel or ask users to sign-up using `/accounts/signup`. Filling out a complete profile is recommended. Ghostwriter can use full names for displaying user actions and filling-in report templates.

{% hint style="warning" %}
Django usernames are _case-sensitive_, so all lowercase is recommended to avoid confusion later.
{% endhint %}

### Customizing Your Installation

You may wish to change some of the configuration options. The following sections outline common customizations.

If you make changes to the configuration, restart Ghostwriter for the changes to take effect:

`./ghostwriter-cli restart {prod|dev}`

#### Using Your Own Certificates

You can use your own TLS/SSL certificates for Ghostwriter. To swap in your own certificate package:

1. Name the keypair files _ghostwriter.key_ and _ghostwriter.crt_
2. Name the Diffie-Helman Parameters file _dhparam.pem_
3. Place all three files inside the _ssl_ directory

#### Customizing the Date Format

The default format is `d M Y` which formats dates like so: _3 Jun 2022_

This format is the default used in parts of the user interface and reports. You can change the date format with this command:

`./ghostwriter-cli config set date_format "d M Y"`

{% hint style="info" %}
When you set `DATE_FORMAT` use Django's format string values:

[https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#std:templatefilter-date](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#std:templatefilter-date)
{% endhint %}

#### Customizing the Domain Name or IP Address

To avoid potential exposure to [HTTP Host header attacks](https://portswigger.net/web-security/host-header), Ghostwriter explicitly checks the hostname against a list of allowed hosts. To access Ghostwriter with your custom domain name or server IP address, you must tell the server to allow new IP addresses or hostnames.

To allow a new IP address or hostname, run this command:

`./ghostwriter-cli config allowhost <YOUR DOMAIN NAME OR IP>"`

You can use `config disallowhost` to remove a host you have added to the list.

{% hint style="warning" %}
While **not** recommended, a wildcard (`*`) will work, but only `*`.

A `*` will allow any hostname or IP address.

Anything like `*.myserver.local` or `192.168.10.*` will not work to allow a host.
{% endhint %}
