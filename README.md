# Apprl Sentry On-Premise

Installation / upgrade

We had to do a little custimization of the sentry onpremise installation, so we forked their project.

make build
tag [image] 169325977222.dkr.ecr.us-east-1.amazonaws.com/sentry-onpremise:[bump tag] docker push 169325977222.dkr.ecr.us-east-1.amazonaws.com/sentry-onpremise:[tag]

I installed the chart like this (it takes a surprisingly long time)
helm upgrade --install —-wait -f sentry.yml --namespace tools sentry stable/sentry

```
sentry.yml:
image:
  repository: 169325977222.dkr.ecr.us-east-1.amazonaws.com/sentry-onpremise
  tag: 8.22.2
ingress:
  enabled: true
  hostname: sentry.dev.apprl.com
web:
  env:
    - name: GITHUB_APP_ID
      value: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    - name: GITHUB_API_SECRET
      value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
```


After I installed Sentry I hade to setup Github Auth / Slack etc via web admin.

If you want to upgrade the release in the future, refresh the fork, rebuild and push new image. Redeploy chart with new image.
I think they're going to bundle the  github auth plugin in the future so we might not need a custom image.

## TODO
* Get public DNS entry. Should probably be sentry.apprl.com
* Connect commits to releases:
https://docs.sentry.io/learn/releases/#releases-are-better-with-commits
* Add support for frontend
* Add to other projects - finance etc.




# Sentry On-Premise

Official bootstrap for running your own [Sentry](https://sentry.io/) with [Docker](https://www.docker.com/).

## Requirements

 * Docker 1.10.0+
 * Compose 1.6.0+ _(optional)_

## Up and Running

Assuming you've just cloned this repository, the following steps
will get you up and running in no time!

There may need to be modifications to the included `docker-compose.yml` file to accommodate your needs or your environment. These instructions are a guideline for what you should generally do.

1. `mkdir -p data/{sentry,postgres}` - Make our local database and sentry config directories.
    This directory is bind-mounted with postgres so you don't lose state!
2. `docker-compose run --rm web config generate-secret-key` - Generate a secret key.
    Add it to `docker-compose.yml` in `base` as `SENTRY_SECRET_KEY`.
3. `docker-compose run --rm web upgrade` - Build the database.
    Use the interactive prompts to create a user account.
4. `docker-compose up -d` - Lift all services (detached/background mode).
5. Access your instance at `localhost:9000`!

Note that as long as you have your database bind-mounted, you should
be fine stopping and removing the containers without worry.

## Securing Sentry with SSL/TLS

If you'd like to protect your Sentry install with SSL/TLS, there are
fantastic SSL/TLS proxies like [HAProxy](http://www.haproxy.org/)
and [Nginx](http://nginx.org/).

## Resources

 * [Documentation](https://docs.sentry.io/server/installation/docker/)
 * [Bug Tracker](https://github.com/getsentry/onpremise)
 * [Forums](https://forum.sentry.io/c/on-premise)
 * [IRC](irc://chat.freenode.net/sentry) (chat.freenode.net, #sentry)
