Forked from https://github.com/heroku/heroku-confluent-buildpack

# Heroku Confluent REST Proxy Buildpack

Installs the [Confluent](https://confluent.io) distribution on
Heroku. While you won't be able to run ZooKeeper or Kafka on it, you
could in theory run Kafka Rest on it, which is the primary purpose of
this buildpack.

The original buildpack was modified to support only the REST proxy.

## Setup

**NOTE:** The build script depends on the `CONFLUENT_*` config variables outlined below. If they change, you must re-build your Heroku app.

```bash
$ heroku create -b https://github.com/pharosjournals/heroku-confluent-restproxy-buildpack.git
```

Now that you have an app, you can download and "install" a version
by setting both `CONFLUENT_PACKAGE` and `CONFLUENT_VERSION=5.3.1`, and deploying an app that
makes use of confluent.

```bash
$ heroku config:set CONFLUENT_PACKAGE=http://packages.confluent.io/archive/5.3/confluent-community-5.3.1-2.12.tar.gz
$ heroku config:set CONFLUENT_VERSION=5.3.1
```

**NOTE**: Be sure to use the community version of confluent as the full version is likely too big for Heroku to build and deploy.

Lastly, set the `CONFLUENT_PROPERTIES` environment variable. This should be the contents you want in the confluent properties  file as a base64-encoded string. Must include an entry for the port to listen on with a value of `%PORT`:

```bash
port=%PORT%
```

A `bin/run-confluent-restproxy` script will be created which will perform the following
steps:

* Will generate a `confluent.properties` file from the `CONFLUENT_PROPERTIES` environment variable.

* It will setup a trap to run `bin/kafka-rest-stop` on
  SIGINT, or SIGTERM.

* It will then substitute `%PORT%` for the value of the environment
  variable `$PORT` (this enables static configuration).

* It will then launch `bin/kafka-rest-start confluent.properties`

This provides ultimate flexibility in generating your properties file
from the heroku config.

## Your app's Procfile

```bash
$ cat Procfile
web: bin/run-confluent-restproxy
```
