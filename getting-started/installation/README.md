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

The `.envs` directory contains `.local` and `.production` directories. Each of these directories contains the same two files, `.django` and `.postgres`. These files manage the environment variables for the Docker containers.

{% hint style="info" %}
Ghostwriter does not allow users to sign-up for an account by default. If you intend to have users sign-up for accounts, edit the `.django` file to re-enable `DJANGO_ACCOUNT_ALLOW_REGISTRATION`. Also, enable `ACCOUNT_EMAIL_VERIFICATION` if you want to require email verification.&#x20;
{% endhint %}

### Notes on DATE\_FORMAT

Be aware that Django offers some proprietary date format strings that may cause problems with date formatting in reports. Use the standard Python `strftime` values to make things easier later when you may want to use date filters in your report templates.

For example, Django has its own `N` format string that formats the month in the Associated Press style. This style has no Python `strftime` counterpart. It abbreviates months like the standard `b` format string, but it also adds a period and does not abbreviate some months (e.g., March, April).

If you use `N` for your `DATE_FORMAT` value and then use the `date_format` filter in a report template, this may become a problem when your filter expects `%b.` and gets `March` instead of `Mar.` when March rolls around.

Some of Django's proprietary values are acceptable, but you'll need to translate them if you want to format them later. For example, Django will read `-d` (the `strftime` value that represents a decimal day without a leading zero) literally and return a zero-padded day value with a leading `-`. Django's custom `j` value returns a day value without a leading zero. If you use `j`, you just need to remember that's `-d` if you try formatting it later with `date_format` in a report.

Further, there will be no errors if you provide an invalid value. For example, if you set `DATE_FORMAT=d M x` where `x` is an invalid `strftime` value, your dates will look like `7 March x`.

The most common values you might use are in the following table alongside their Python translations.

| Django | Python |                     Result                     |
| :----: | :----: | :--------------------------------------------: |
|    d   |    d   |         Day of the month (zero-padded)         |
|    j   |   -d   |       Day of the month (not zero-padded)       |
|    M   |    b   | Abbreviated textual month name (three-letters) |
|    F   |    B   |             Full textual month name            |
|    m   |    m   |        Month as a decimal (zero padded)        |
|    n   |   -m   |      Month as a decimal (not zero padded)      |
|    y   |    y   |       Year without century (zero padded)       |
|    Y   |    Y   |                Year with century               |

Further reference:

{% embed url="https://docs.djangoproject.com/en/4.0/ref/templates/builtins#std:templatefilter-date" %}

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
