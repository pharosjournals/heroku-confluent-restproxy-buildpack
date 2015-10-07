# Heroku Confluent Buildpack

Installs the [Confluent](https://confluent.io) distribution on
Heroku. While you won't be able to run ZooKeeper or Kafka on it, you
could in theory run Kafka Rest on it, which is the primary purpose of
this buildpack.

## Setup

```bash
$ heroku create -b https://github.com/heroku/heroku-confluent-buildpack.git
Creating vast-woodland-9430... done, stack is cedar-14
Buildpack set. Next release on vast-woodland-9430 will use https://github.com/heroku/heroku-confluent-buildpack.git.
https://vast-woodland-9430.herokuapp.com/ | https://git.heroku.com/vast-woodland-9430.git
Git remote heroku added
```

Of course, this doesn't solve the problem of setting up a proper
config file, which is left as an exercise to the reader, for now.