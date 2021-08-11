---
description: How to install Ghostwriter locally and in production
---

# Installation

{% hint style="danger" %}
**STOP!** Ghostwriter is installed using [Docker Compose](https://docs.docker.com/compose/). Install Docker before proceeding.
{% endhint %}

## Ghostwriter Environment Variables

First, copy the examples in the `.envs_template` directory into a new `.envs` directory within the Ghostwriter directory. These files control the environment variables for the Docker containers.

Run this command to create the directory and then recursively copy the files into your new directory:

`mkdir .envs && cp -r .envs_template/.* .envs`

{% hint style="warning" %}
You may notice this command does not work as described on your server. Some OS and shell flavors interpret the `.` characters in different ways. This is discussed in this thread and issue:

[https://unix.stackexchange.com/questions/40662/what-is-the-setting-in-bash-for-globbing-to-control-whether-matches-dot-files](https://unix.stackexchange.com/questions/40662/what-is-the-setting-in-bash-for-globbing-to-control-whether-matches-dot-files)

[https://github.com/GhostManager/Ghostwriter/issues/49](https://github.com/GhostManager/Ghostwriter/issues/49)

Try this sequence if you encounter the recursive copy issues:

```bash
shopt -s dotglob
mkdir .envs && cp -r .envs_template/* .envs/ 
```

If you are using `zsh`, use this command: `setopt dotglob`
{% endhint %}

The `.envs` directory contains `.local` and `.production` directories. Each of these directories contains the same two files, `.django` and `.postgres`. These files manage the environment variables for the Docker containers.

{% hint style="info" %}
Ghostwriter does not allow users to sign-up for an account by default. If you intend to have users sign-up for accounts, edit the `.django` file to re-enable `DJANGO_ACCOUNT_ALLOW_REGISTRATION`. Also, enable `ACCOUNT_EMAIL_VERIFICATION` if you want to require email verification. 
{% endhint %}

## Building the Container

Once you have the `.envs` directory setup, check Docker to make sure the service is running. Then build and start the Docker containers using the `local.yml` file \(_not_ `production.yml`\):

`docker-compose -f local.yml stop; docker-compose -f local.yml rm -f; docker-compose -f local.yml build; docker-compose -f local.yml up -d`

{% hint style="info" %}
The `-d` at the end means you will detach from the containers. If you'd like to monitor live console output, remove `-d` to remain attached to the containers. This is helpful during development and testing.
{% endhint %}

{% hint style="warning" %}
Resist any temptation to jump straight to using the production settings with `production.yml`. Production is geared towards actual production use. Therefore, it does not perform certain operations required for the initial setup that might be undesirable in production.
{% endhint %}

## Next Steps

Proceed to Database Setup.

