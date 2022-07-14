---
description: How to check for updates and what to do when one is available
---

# Updating Ghostwriter

## Checking for Updates

You can check for updates with Ghostwriter CLI and the `update` command. The output will look something like this:

```
$ ./ghostwriter-cli update

Installed version: Ghostwriter v2.2.3 ( 22 February 2022 )
Latest stable version: Ghostwriter v2.2.3 ( 22 February 2022 )
```

The command will take a moment to run as Ghostwriter CLI requests the latest release number from GitHub. If you do not have a network connection the latest version number will not be displayed.

If your version number and release date are older than the reported latest release you may want to update. Check the [Ghostwriter CHANGELOG](https://github.com/GhostManager/Ghostwriter/blob/master/CHANGELOG.md) to see what has changed to determine if now is the right time to update for you.

## Installing Updates

Updating Ghostwriter is as easy as pulling the latest code and building the updated containers. Your data will be unaffected by the build process.

{% hint style="warning" %}
Updates are generally easy, but you should **strongly** consider taking a snapshot of your host server in case anything goes wrong. There is always a chance something like a Python library may not install properly and you don't have the time to address it on the spot. You will thank yourself if you are able to restore a snapshot and try again later.
{% endhint %}

To perform an update:

```
git pull
./ghostwriter-cli containers down
./ghostwriter-cli containers build
./ghostwriter-cli containers up
```

These commands pull the latest code, stop any running production containers, build the new containers, and then bring Ghostwriter back online.
