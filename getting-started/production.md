---
description: Moving Ghostwriter into production with Nginx and HTTPS
---

# Switch to Production

## Moving to Production

Eventually, you will want to move Ghostwriter from a development server to a production server.

Even though Ghostwriter _could_ be run in developer mode forever, it is much better to use HTTPS and take some of the workload off of Django when it comes to hosting files.

### Adjusting Production setting

Start your move to production by opening `.envs/.production/.django`. The `SECRET_KEY` variable is set to `changeme`. Generate a new value and drop in the new key. It is usually something like:

`cg#p$g+j9tax!#a3cup@1$8obt2_+&k3q+pmu)5%asj6yjpkag`.

The default configuration will host Ghostwriter on _0.0.0.0:443_ with HTTPS. 

If you would like to modify the listening port or anything else within the Nginx configuration, open `compose/production/nginx/nginx.conf`. You can make changes to this configuration file like you would for any Nginx configuration.

{% hint style="info" %}
Production deployments use separate settings from local deployments, so make changes to `.envs/.production/.django` and `.../.postgres` as desired.
{% endhint %}

{% hint style="success" %}
You can keep separate settings for your local/development and production deployments.
{% endhint %}

### Using a Domain Name

You may wish to use a domain name instead of an IP address once you move Ghostwriter into production. By default, Ghostwriter's production deployment only allows the server to be accessed using **ghostwriter.local** and **localhost**.

Open your `.envs/.production/.django` file and uncomment the `ALLOWED_HOSTS` line to make modifications.

{% hint style="info" %}
If you see 400 BAD REQUEST errors in your web browser, check the Ghostwriter console output. Chances are good you will see mention of your domain name not matching anything in `ALLOWED_HOSTS`.
{% endhint %}

### Using HTTPS

Before running in production, it is necessary to set up an SSL certificate. A self-signed certificate can be created using the following commands. Other options include purchasing a certificate or using [LetsEncrypt](https://letsencrypt.org/) for a free certificate.

Certificates should be placed in the `ssl/` folder. The files referenced in `compose/production/nginx/nginx.conf` use the following files names:

* ghostwriter.crt
* ghostwriter.key
* dhparam.pem

If different filenames are used, update the `nginx.conf` to reflect the correct filenames.

#### Creating a Self-Signed SSL Certificate

**With Prompts**

```text
openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -keyout ghostwriter.key -out ghostwriter.crt
```

**Without Prompts**

```text
openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=/ST=/L=/O=Ghostwriter/CN=ghostwriter.local" -keyout ghostwriter.key -out ghostwriter.crt
```

**Creating the dhparam.pem**

```text
openssl dhparam -out dhparam.pem 4096
```

## Deploy to Production

Deploy Ghostwriter in production mode using Docker Compose and the `production.yml` file.

`docker-compose -f production.yml stop; docker-compose -f production.yml rm -f; docker-compose -f production.yml build; docker-compose -f production.yml up -d`

