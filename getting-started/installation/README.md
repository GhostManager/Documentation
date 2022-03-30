---
description: How to install Ghostwriter locally and in production
---

# Installation

{% hint style="danger" %}
**STOP!** Ghostwriter is installed using [Docker Compose](https://docs.docker.com/compose/). Install Docker before proceeding.

You will need a Docker version >=20 for the Alpine Linux images used for Ghostwriter. Run `docker --version` to check your installation.
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

The `.envs` directory contains `.local` and `.production` directories. Each of these directories contains the same set of files: `.django`, `.postgres`, and `.hasura`. These files manage the environment variables for the Docker containers.

When configuring these files for your initial local deployment, the default options will be fine. You will want to change some of the values before deploying Ghostwriter into a production environment. This is covered in-depth in the [_**Switch to Production**_](../production.md) section.

If you wish to change any of the default values now, refer to the _**Adjusting Production Settings**_ section on that page for guidance.

{% hint style="info" %}
When you set `DATE_FORMAT` use Django's format string values. They are different than Python's standard values you might be familair with. Django also offers several additional options.

[https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#std:templatefilter-date](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#std:templatefilter-date)
{% endhint %}

## Building the Container

Once you have the `.envs` directory setup, check Docker to make sure the service is running. Then build and start the Docker containers using the `local.yml` file (_not_ `production.yml`):

`docker-compose -f local.yml stop; docker-compose -f local.yml rm -f; docker-compose -f local.yml build; docker-compose -f local.yml up -d`

{% hint style="info" %}
The `-d` at the end means you will detach from the containers. If you'd like to monitor live console output, remove `-d` to remain attached to the containers. This is helpful during development and testing.
{% endhint %}

{% hint style="warning" %}
Resist any temptation to jump straight to using the production settings with `production.yml`. Production is geared towards actual production use. Therefore, it does not perform certain operations required for the initial setup that might be undesirable in production.
{% endhint %}

## Next Steps

Proceed to Database Setup.
