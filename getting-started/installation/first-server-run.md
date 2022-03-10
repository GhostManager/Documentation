---
description: Required steps for your first run of Ghostwriter
---

# First Server Run

## Creating Users

Your containers are built and running and the database is primed, so now it is time to create your first user.

The first time you use Ghostwriter, you must create a "superuser" to access the admin panel and create new users.

{% hint style="warning" %}
This is the administrator of all of Django, so set a strong password and document it in a password vault somewhere.
{% endhint %}

### Create a Superuser

To create a superuser, run:

`docker-compose -f local.yml run --rm django python manage.py createsuperuser`

Success looks like this:

```
$ docker-compose -f local.yml run --rm django python manage.py createsuperuser
Creating ghostwriter_django_run ... done
PostgreSQL is available
Username: benny
Email address: benny@ghostwriter.wiki
Password:
Password (again):
Superuser created successfully.
```

Visit _http://SERVER\_IP:8000/admin_ to view the admin panel and log in using your new superuser.

{% hint style="info" %}
The defaults for the local deployment have the server listening on localhost, 127.0.0.01, and 0.0.0.0 on Django's default development, 8000.
{% endhint %}

### Creating New Users

You may create users using the admin panel or ask users to sign-up using `/accounts/signup`. Filling out a complete profile is recommended. Ghostwriter can use full names for displaying user actions and filling-in report templates. Ghostwriter can also use email addresses for password recovery.

{% hint style="warning" %}
Django usernames are _case sensitive_, so all lowercase is recommended to avoid confusion later.
{% endhint %}

{% hint style="info" %}
Signups are disabled by default as it is assumed most deployments will not want it to be open. Signups are managed by the `DJANGO_ACCOUNT_ALLOW_REGISTRATION` in your `.envs` configuration. Set it to `True` in your `.django` file to re-enable it.
{% endhint %}

### Password Changes

Once you create an account, hand-off the username and password to the intended user. That person may then login and click their avatar icon in the upper-right corner to change their password and upload a custom avatar (if they so desire).

Users can request password resets using the email address linked to their account and the "forgot password" link on the login form.

{% hint style="info" %}
Unless you configure an email back-end for Django, the server will send the recovery emails to the console.
{% endhint %}

## Next Steps

Use Ghostwriter for some time and see if everything is working properly. If you intend to make modifications to the codebase now is the time to do that.

Visit [_**Command Center**_ ](../../configuring-global-settings/introduction-to-command-center.md)to personalize your server configuration.

{% content-ref url="../../configuring-global-settings/introduction-to-command-center.md" %}
[introduction-to-command-center.md](../../configuring-global-settings/introduction-to-command-center.md)
{% endcontent-ref %}
