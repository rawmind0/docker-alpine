docker-alpine
=============

Environment images hierarchy to build Docker containers running [Alpine linux][alpinelinux], [monit][monit] as process management and [confd][confd] as config management.

## Users

The following describe the different users that run the services.

- nexus - 10000 - nexus - 10000
- traefik - 10001 - traefik - 10001
- zookeeper - 10002 - zookeeper - 10002
- kafka - 10003 - kafka - 10003
- nginx - 10004 - nginx - 10004
- etcd - 10005 - etcd - 10005
- vamp - 10006 - vamp - 10006
- haproxy - 10007 - haproxy - 10007

## Catalog

The following repo provides rancher catalog package to deploy services. There are packages for cattle and for k8s.

* [service-catalog](https://github.com/rawmind0/service-catalog)

## Containers

The following describes the containers that are available and the inheritance chain:

![Alt text](docker-hierarchy.png?raw=true "Title")


* [alpine-base](https://github.com/rawmind0/alpine-base)
  * [alpine-volume](https://github.com/rawmind0/alpine-volume)
  * [alpine-go-builder](https://github.com/rawmind0/alpine-go-builder)
  * [alpine-monit (executor)](https://github.com/rawmind0/alpine-monit)
    * [alpine-etcd](https://github.com/rawmind0/alpine-etcd)
    * [alpine-haproxy](https://github.com/rawmind0/alpine-haproxy)
    * [alpine-jvm8](https://github.com/rawmind0/alpine-jvm8)
      * [alpine-sbt-builder](https://github.com/rawmind0/alpine-sbt-builder)
      * [alpine-nexus](https://github.com/rawmind0/alpine-nexus)
      * [alpine-zk](https://github.com/rawmind0/alpine-zk)
      * [alpine-kafka](https://github.com/rawmind0/alpine-kafka)
      * [alpine-vamp](https://github.com/rawmind0/alpine-vamp)
    * [alpine-nginx](https://github.com/rawmind0/alpine-nginx.git)
      * [alpine-vamp-ui](https://github.com/rawmind0/alpine-vamp-ui)
    * [alpine-traefik](https://github.com/rawmind0/alpine-traefik.git)
  * [alpine-tools (configurator)](https://github.com/rawmind0/alpine-tools)
    * [rancher-tools](https://github.com/rawmind0/rancher-tools)
      * [rancher-etcd](https://github.com/rawmind0/rancher-etcd.git)
      * [rancher-kafka](https://github.com/rawmind0/rancher-kafka.git)
      * [rancher-traefik](https://github.com/rawmind0/rancher-traefik.git)
      * [rancher-zk](https://github.com/rawmind0/rancher-zk.git)
    * [k8s-tools](https://github.com/rawmind0/k8s-tools)
      * [k8s-kafka](https://github.com/rawmind0/k8s-kafka.git)
      * [k8s-zk](https://github.com/rawmind0/k8s-zk.git)
  * [alpine-redis](https://github.com/rawmind0/alpine-redis)
  * [alpine-skydns](https://github.com/rawmind0/alpine-skydns)
  * [web-test](https://github.com/rawmind0/web-test)

### alpine-base

This image is the base for all containers. It contains Alpine Linux with bash, openssl and curl. It clocks in at a tiny 4 MB.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-base/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-base/tags/)).

### alpine-volume

A base image to run any storage volume for a service. It's based in rawmind/alpine-base.

It creates and prepares $SERVICE_VOLUME volume with $SERVICE_UID and $SERVICE_GID owners. 

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-volume/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-volume/tags/)).

### alpine-go-builder

A base image to build any golang app. It's based in rawmind/alpine-base, adding git, mercurial, bzr, make and go, to make able to build golang app's.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-go-builder/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-go-builder/tags/)).

### alpine-monit

A base image to run anything. It's based in rawmind/alpine-base, adding monit as process management.

- It compiles and install monit under /opt/monit. 
- Monit will be used as process management to execute microservices tasks. 
- From this image, you could build language specific images or microservices images directly.
- Entrypoint is stablished to execute monit and read configuration from 2 directories, /opt/monit/etc/cond.d and /opt/tools/monit/cond.d if exists (this folder doesn’t exists in this container, it would be provided by configurator).

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-monit/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-monit/tags/)).

### alpine-haproxy

A base image to run any haproxy base. It's based in rawmind/alpine-monit, building and installing haproxy.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-haproxy/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-haproxy/tags/)).

### alpine-jvm8

A base image to run any jvm8 app. It's based in rawmind/alpine-monit, adding jvm8 to make able to build jvm8 app's.

- Install oracle jre8 under /opt/jre
- From this image, you could build your jvm microservices.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-jvm8/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-jvm8/tags/)).

### alpine-sbt-builder

A base image to build any jvm8 app with sbt. It's based in rawmind/alpine-jvm8, adding sbt to make able to build jvm8 app's.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-sbt-builder/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-sbt-builder/tags/)).

### alpine-nexus

A base image to run nexus server. It's based in rawmind/alpine-jvm8.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-nexus/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-nexus/tags/)).

### alpine-kafka

A base image to run kafka server. It's based in rawmind/alpine-jvm8.

- Install and configure kafka under /opt/kafka.
- It provides a monit config to run kafka. /opt/monit/etc/conf.d/monit-service.cfg 
- It provides a script to start/stop/restart kafka.
- If it’s run alone, it gets a default configuration and run kafka. 
- If it’s run with rancher-kafka or with k8s-kafka in a rancher env, it runs kafka and confd to get dynamic config from cattle or kubernetes.
- Service is started with its own user and group. # kafka-10003 kafka-10003
- Default options could be overwritten through env variables.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-kafka/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-kafka/tags/)).

### alpine-zk

A base image to run zookeeper server. It's based in rawmind/alpine-jvm8.

- Install and configure zookeeper under /opt/zk.
- It provides a monit config to run zookeeper. /opt/monit/etc/conf.d/monit-service.cfg 
- It provides a script to start/stop/restart zookeeper.
- If it’s run alone, it gets a default configuration and run zookeeper. 
- If it’s run with rancher-zk or with k8s-zk in a rancher env, it runs zookeeper and confd to get dynamic config from cattle or kubernetes.
- Service is started with its own user and group. # zookeeper-10002 zookeeper-10002
- Default options could be overwritten through env variables.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-zk/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-zk/tags/)).

### alpine-nginx

A base image to run nginx. It's based in rawmind/alpine-monit, adding nginx to make able to run it.

- Compile and install nginx under /opt/nginx.
- It provides a monit config to run nginx. /opt/monit/etc/conf.d/monit-service.cfg 
- It provides a script to start/stop/restart nginx.
- From this image, you could build your html microservices. 
- It could be also used as loadbalancer/proxy base.
- Service is started with its own user and group.
- Default options could be overwritten through env variables.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-nginx/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-nginx/tags/)).

### alpine-traefik

A base image to run traefik. It's based in rawmind/alpine-monit, adding traefik and a basic config to make able to run it.

- Install and configure traefik under /opt/traefik.
- It provides a monit config to run traefik. /opt/monit/etc/conf.d/monit-service.cfg 
- It provides a script to start/stop/restart traefik.
- If it’s run alone, it gets a default configuration and run traefik. 
- If it’s run with rancher-traefik in a cattle env, it runs traefik and confd to get dynamic config.
- Service is started with its own user and group.
- Default options could be overwritten through env variables.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-traefik/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-traefik/tags/)).

### alpine-tools

A base image to expose tools to services. It's based in rawmind/alpine-base, adding confd as config management and shraing volume /opt/tools
 
- Creates and shared with the executors, $SERVICE_VOLUME /opt/tools with $SERVICE_UID and $SERVICE_GID owners.
- It compiles statically and install confd under $SERVICE_VOLUME/confd.
- The volume is used to share all software tools for configuration aids to the microservices. 

Initial volume structure:

```
/opt/tools/
  confd/          # Confd directory
    etc/
      templates/  # confd template directory
      conf.d/     # confd toml directory
    bin/          # confd bin directory
  monit/
    conf.d/       # Monit config directory
  scripts/        # Custom scripts directory (for future use)
```

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-tools/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-tools/tags/)).

### rancher-tools

A base image to expose tools to rancher services. It's based in rawmind/alpine-tools, adding confd and monit scripts to the image.

- Contextualize the configuration tools to talk with rancher-metadata. 
- Provides a monit config to run conf.d. $SERVICE_VOLUME/monit/conf.d/monit-conf.cfg
- Provides a script to start/stop/restart confd. 
- Confd is default configured to connect to rancher-metadata service. $SERVICE_VOLUME/confd/bin/service-conf.sh
- Default options could be overwritten through env variables. 

* Latest version ([Dockerfile](https://github.com/rawmind0/rancher-tools/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/rancher-tools/tags/)).

### rancher-traefik

A base image to expose tools to traefik service running in rancher. It's based in rawmind/rancher-tools, adding confd templates to generate traefik config.

- Sets rancher configuration tools to get config for traefik.
- Provides confd toml’s and templates to generate traefik dynamic config from rancher-metadata.
- To provide the volume, it has to run once as a alpine-traefik sidekick.

* Latest version ([Dockerfile](https://github.com/rawmind0/rancher-traefik/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/rancher-traefik/tags/)).

### rancher-kafka

A base image to expose tools to kafka service running in rancher. It's based in rawmind/rancher-tools, adding confd templates to generate kafka config.

- Sets rancher configuration tools to get config for kafka.
- Provides confd toml’s and templates to generate kafka dynamic config from rancher-metadata.
- To provide the volume, it has to run once as a alpine-kafka sidekick.

* Latest version ([Dockerfile](https://github.com/rawmind0/rancher-zk/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/rancher-zk/tags/)).

### rancher-zk

A base image to expose tools to zookeeper service running in rancher. It's based in rawmind/rancher-tools, adding confd templates to generate zookeeper config.

- Sets rancher configuration tools to get config for zookeeper.
- Provides confd toml’s and templates to generate zookeeper dynamic config from rancher-metadata.
- To provide the volume, it has to run once as a alpine-zk sidekick.

* Latest version ([Dockerfile](https://github.com/rawmind0/rancher-zk/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/rancher-zk/tags/)).

### k8s-tools

A base image to expose tools to kubernetes services. It's based in rawmind/alpine-tools, adding confd and monit scripts to the image.

- Contextualize the configuration tools to talk with kubernetes etcd. 
- Provides a monit config to run conf.d. $SERVICE_VOLUME/monit/conf.d/monit-conf.cfg
- Provides a script to start/stop/restart confd. 
- Confd is default configured to connect to kubernetes etcd service. $SERVICE_VOLUME/confd/bin/service-conf.sh
- Default options could be overwritten through env variables. 

* Latest version ([Dockerfile](https://github.com/rawmind0/k8s-tools/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/k8s-tools/tags/)).

### k8s-kafka

A base image to expose tools to kafka service running in kubernetes. It's based in rawmind/k8s-tools, adding confd templates to generate kafka config.

- Sets k8s configuration tools to get config for kafka.
- Provides confd toml’s and templates to generate kafka dynamic config from k8s etcd.

* Latest version ([Dockerfile](https://github.com/rawmind0/k8s-kafka/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/k8s-kafka/tags/)).

### k8s-zk

A base image to expose tools to zookeeper service running in kubernetes. It's based in rawmind/k8s-tools, adding confd templates to generate zookeeper config.

- Sets k8s configuration tools to get config for zookeeper.
- Provides confd toml’s and templates to generate zookeeper dynamic config from k8s etcd.

* Latest version ([Dockerfile](https://github.com/rawmind0/k8s-zk/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/k8s-zk/tags/)).

### alpine-redis

A base image to run redis. It's based in rawmind/alpine-base.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-redis/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-redis/tags/)).

### alpine-skydns

A base image to run skydns. It's based in rawmind/alpine-base.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-skydns/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-skydns/tags/)).

### web-test

This image runs a web service in 8080 port, used for testing. It comes from rawmind/alpine-base.

* Latest version ([Dockerfile](https://github.com/rawmind0/web-test/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/web-test/tags/)).

[alpinelinux]: https://www.alpinelinux.org/
[monit]: https://mmonit.com/monit/
[confd]: https://github.com/kelseyhightower/confd


