---
description: Common problems and questions
---

# FAQ

### Ghostwriter CLI Reports an Issue with PostgreSQL

You may encounter an issue with PostgreSQL while upgrading an existing installation. Ghostwriter v2.x.x and lower used environment variables stored in files under _.envs/.production/.postgres_. Beginning with v3.0.0, Ghostwriter no longer uses the _.envs/_ files and you must transfer your PostgreSQL username and password to the new configuration file.

By default, Ghostwriter CLI generates a random password for a default `postgres` user. Update the configuration with your Postgres credentials by running these commands:

```
./ghostwriter-cli config set POSTGRES_USER <YOUR USER>
./ghostwriter-cli config set POSTGRES_PASSWORD <YOUR PASSWORD>
```

If that does not work or you need to change the credentials, you may do so with the `psql` tool. First, start the containers even if initialization fails due to the bad password.

```
docker exec -it ghostwriter_postgres_1 bash
psql -U postgres
```

Use the `\password` command to set a new password for the `postgres` user:

```
postgres=# \password postgres
Enter new password: 
Enter it again: 
postgres=# \q
```

{% hint style="success" %}
If `postgres` is not your username, change the command to use your chosen username. If you are not sure what the username is, run the `\du` command:

```
postgres=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```
{% endhint %}

That sets the new password and `\q` quits the `psql` console. Set your new password in Ghostwriter's config, and then bring the containers down and back up.

```
./ghostwriter-cli containers down
./ghostwriter-cli config set POSTGRES_PASSWORD <NEW PASSWORD>
./ghostwriter-cli containers up
```

If the passwords still do not match, you may need to try again. Copy/pasting the password may not work. If you try to paste the new password, you might not be setting the password you expect. It is best to type the password manually.



