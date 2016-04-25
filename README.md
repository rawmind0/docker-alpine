docker-alpine
=============

Environment images hierarchy to build Docker containers running [Alpine linux][alpinelinux], [monit][monit] as process management and [confd][confd] as config management.

## Containers

The following describes the containers that are available and the inheritance chain:


* [alpine-base](https://github.com/rawmind0/alpine-base)
  * [alpine-go-builder](https://github.com/rawmind0/alpine-go-builder)
  * [alpine-monit](https://github.com/rawmind0/alpine-monit)
    * [alpine-jvm8](https://github.com/rawmind0/alpine-jvm8)
      * [alpine-sbt-builder](https://github.com/rawmind0/alpine-sbt-builder)
    * [alpine-skydns]
  * [alpine-tools](https://github.com/rawmind0/alpine-tools)
    * [rancher-tools](https://github.com/rawmind0/rancher-tools)


### alpine-base

This image is the base for all containers. It contains Alpine Linux with bash, openssl and curl. It clocks in at a tiny 4 MB.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-base/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-base/tags/)).

### alpine-go-builder

A base image to build any golang app. It's based in rawmind/alpine-base, adding git, mercurial, bzr, make and go, to make able to build golang app's.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-go-builder/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-go-builder/tags/)).

### alpine-monit

A base image to run anything. It's based in rawmind/alpine-base, adding monit as process management.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-monit/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-monit/tags/)).

### alpine-jvm8

A base image to run any jvm8 app. It's based in rawmind/alpine-monit, adding jvm8 to make able to build jvm8 app's.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-jvm8/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-jvm8/tags/)).

### alpine-sbt-builder

A base image to build any jvm8 app with sbt. It's based in rawmind/alpine-jvm8, adding sbt to make able to build jvm8 app's.

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-sbt-builder/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-sbt-builder/tags/)).

### alpine-tools

A base image to expose tools to services. It's based in rawmind/alpine-base, adding confd as config management and shraing volume /opt/tools

* Latest version ([Dockerfile](https://github.com/rawmind0/alpine-tools/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/alpine-tools/tags/)).

### rancher-tools

A base image to expose tools to rancher services. It's based in rawmind/alpine-tools, adding confd and monit scripts to the image.

* Latest version ([Dockerfile](https://github.com/rawmind0/rancher-tools/blob/master/Dockerfile)).
* Image versions ([Tags](https://hub.docker.com/r/rawmind/rancher-tools/tags/)).


[alpinelinux]: https://www.alpinelinux.org/
[monit]: https://mmonit.com/monit/
[confd]: https://github.com/kelseyhightower/confd
