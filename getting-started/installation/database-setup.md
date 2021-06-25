---
description: Required database prep once Ghostwriter is installed
---

# Database Setup

## Seeding the Database

{% hint style="danger" %}
Before Ghostwriter can be used, you must populate the models with the provided Django fixtures. Without this data nothing will really work and you **will encounter errors.**
{% endhint %}

Once Docker has built your containers, you are ready to prepare the database. You know you're ready to begin when you see:

```javascript
Creating ghostwriter_postgres_1 ... done
Creating ghostwriter_redis_1    ... done
Creating ghostwriter_queue_1    ... done
Creating ghostwriter_django_1   ... done
```

Run this command:

`docker-compose -f local.yml run --rm django /seed_data`

{% hint style="info" %}
If you remained attached to the containers in the previous step, you will need to do this from another terminal.
{% endhint %}

That command populates models like the domain status and project types. This data must be inserted into the models before proceeding. If you try to use Ghostwriter without these values you will encounter database migration errors and the application **will be unusable**.

```javascript
# docker-compose -f local.yml run --rm django /seed_data
Starting ghostwriter_postgres_1 ... done
PostgreSQL is available
Loading /app/ghostwriter/reporting/fixtures/initial.json...
Installed 12 object(s) from 1 fixture(s)
Loading /app/ghostwriter/rolodex/fixtures/initial.json...
Installed 10 object(s) from 1 fixture(s)
Loading /app/ghostwriter/shepherd/fixtures/initial.json...
Installed 27 object(s) from 1 fixture(s)
```

### Customizing Fixtures

{% hint style="danger" %}
Editing fixtures may have some unexpected consequences. In general, please **do** **not** **edit** the fixture files not mentioned below.
{% endhint %}

Each sub-application has a _`fixtures`_ folder with an _`initial.json`_ file where the fixtures live. 

You can add or edit the finding types in the **Reporting** fixture file. The initial values include Network, Wireless, Mobile, Web, Physical, and Host.

The **Rolodex** fixture file has multiple types that you can change. Feel free to add or edit the project types and project roles. Project types represent the types of projects \(e.g. red team, penetration test\). The project roles apply to humans assigned to projects \(e.g. assessment lead\).

The **Shepherd** fixture file contains server providers and server roles that you can change. The domain status values should be left alone.

{% hint style="warning" %}
The severity ratings in Ghostwriter are customizable; however, some of the templates expect to find ratings that match the **Critical**, **High**, **Medium**, **Low**, and **Informational** ratings in the provided fixture file. If the ratings are changed there are several HTML template files that will also need to be edited, and it may impact some JavaScript.
{% endhint %}

### Fixture Layout

All fixtures should look something like this:

```javascript
{
    "model": "ghostwriter.findingtype",
    "pk": 5,
    "fields": {
        "finding_type": "Mobile"
    }
}
```

{% hint style="warning" %}
If you add a fixture, be careful to increment the _`pk`_ value \(the primary key\). If you do not, your addition will just overwrite the preceding fixture with the matching `pk`. 
{% endhint %}

## Next Steps

Once the fixtures have been loaded successfully, you are ready to move forward.

