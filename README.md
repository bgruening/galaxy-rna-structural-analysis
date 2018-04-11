[![Build Status](https://travis-ci.org/bgruening/galaxy-rna-structural-analysis.svg?branch=master)](https://travis-ci.org/bgruening/galaxy-rna-structural-analysis)
[![CircleCI](https://circleci.com/gh/bgruening/galaxy-rna-structural-analysis/tree/master.svg?style=svg)](https://circleci.com/gh/bgruening/galaxy-rna-structural-analysis/tree/master)
[![Docker Repository on Quay](https://quay.io/repository/bgruening/galaxy-rna-structural-analysis/status "Docker Repository on Quay")](https://quay.io/repository/bgruening/galaxy-rna-structural-analysis)

Galaxy for RNA structural analysis
==================================

The Galaxy flavor for RNA structural analysis is developed by the RNA Bioinformatics
Center (RBC). This center is one of the eight service units of the
[German Network for Bioinformatics Infrastructure](http://www.denbi.de), running the German [ELIXIR Node](https://www.elixir-europe.org/).

[<img src="assets/img/deNBI_logo.jpg" height="35px" alt="de.NBI"
valign="middle">](http://www.denbi.de)
[<img src="assets/img/elixir_germany.png" height="55px" alt="ELIXIR Germany"
valign="middle">](https://www.elixir-europe.org)

This webserver is based on the [Galaxy Docker](https://github.com/bgruening/docker-galaxy-stable) for advanced local deploymentes we recommend the upstream [documentation](http://bgruening.github.io/docker-galaxy-stable).

# Usage

The Galaxy RNA workbench is based on a dedicated Galaxy instance wrapped into a Docker container. It is based on the [Galaxy Docker Image](http://bgruening.github.io/docker-galaxy-stable/)

## Requirement

To use the Galaxy RNA workbench, you will need [Docker](https://www.docker.com/products/overview#h_installation).

Non-Linux users are encouraged to use [Kitematic](https://kitematic.com) for [OSX](https://github.com/bgruening/galaxy-rna-workbench/blob/master/howto_kitematic_osx.md) or [Windows](https://github.com/bgruening/galaxy-rna-workbench/blob/master/howto_kitematic_win.md), a graphical User-Interface for managing Docker containers.

Linux users and people familiar with the command line can follow the instruction on installing Docker from [Docker website](https://docs.docker.com/installation).

## Docker configuration

The RNA workbench docker container is rather large and expected to grow when further tools and workflows are contributed. So for users new to docker, we list here some tweaks that can help to work around issues when first using docker.
After successful installation of docker, it is recommended to configure some settings, dealing for example with the storage space required by containers. You can find more information [here](howtodocker.md).

## RNA workbench launch

Kitematic users can launch the RNA workbench directly from its interface, browsing all publicly available images from the Docker Hub.

The following video shows the launch of the RNA workbench from Kitematic:

[![Galaxy RNA workbench launch through Kitematic](https://i.imgur.com/qjQlRxJ.png)](https://www.youtube.com/watch?v=fYer4Xdw_h4 "Kitematic galaxy-rna-workbench launch")


For non-Kitematic users, starting the RNA workbench is analogous to start the generic Galaxy Docker image:

```
$ docker run -d -p 8080:80 quay.io/bgruening/galaxy-rna-structural-analysis
```

A detailed discussion of Docker's parameters is given in the [Docker manual](http://docs.docker.io). It is really worth reading.

Nevertheless, here is a quick rundown:

- `docker run` starts the Image/Container

   In case the Container is not already stored locally, docker downloads it automatically

- The argument `-p 8080:80` makes the port 80 (inside of the container) available on port 8080 on your host

    Inside the container a Apache web server is running on port 80 and that port can be bound to a local port on your host computer.
    With this parameter you can access your Galaxy instance via `http://localhost:8080` immediately after executing the command above

- `quay.io/bgruening/galaxy-rna-workbench` is the Image/Container name, that directs docker to the correct path in the [docker index](https://quay.io/repository/bgruening/galaxy-rna-workbench)
- `-d` will start the docker container in Daemon mode.

  For an interactive session, one executes:

  ```
  $ docker run -i -t -p 8080:80 quay.io/bgruening/galaxy-rna-structural-analysis /bin/bash
  ```

  and manually invokes the `startup` script to start PostgreSQL, Apache and Galaxy.

Docker images are "read-only". All changes during one session are lost after restart. This mode is useful to present Galaxy to your colleagues or to run workshops with it.

To install Tool Shed repositories or to save your data, you need to export the calculated data to the host computer. Fortunately, this is as easy as:

```
$ docker run -d -p 8080:80 -v /home/user/galaxy_storage/:/export/ quay.io/bgruening/galaxy-rna-structural-analysis
```

Given the additional `-v /home/user/galaxy_storage/:/export/` parameter, docker will mount the folder `/home/user/galaxy_storage` into the Container under `/export/`. A `startup.sh` script, that is usually starting Apache, PostgreSQL and Galaxy, will recognize the export directory with one of the following outcomes:

  - In case of an empty `/export/` directory, it will move the [PostgreSQL](http://www.postgresql.org/) database, the Galaxy database directory, Shed Tools and Tool Dependencies and various configure scripts to /export/ and symlink back to the original location.
  - In case of a non-empty `/export/`, for example if you continue a previous session within the same folder, nothing will be moved, but the symlinks will be created.

This enables you to have different export folders for different sessions - meaning real separation of your different projects.

It will start the Galaxy RNA workbench with the configuration and launch of a Galaxy instance and its population with the needed tools. The instance will be accessible at [http://localhost:8080](http://localhost:8080).

For a more specific configuration, you can have a look at the [documentation of the Galaxy Docker Image](http://bgruening.github.io/docker-galaxy-stable).

## Users & Passwords

The Galaxy Admin User has the username `admin@galaxy.org` and the password `admin`.

The PostgreSQL username is `galaxy`, the password `galaxy` and the database name `galaxy`.
If you want to create new users, please make sure to use the `/export/` volume. Otherwise your user will be removed after your docker session is finished.

## Tours

The RNA workbench provides interactive tours that illustrate how the main interface works in relation to real-life user tasks.

These show many common operations, such as searching, parametrizing, and running tools, or saving a history of operations in a sharable workflow.

The following video demonstrates the main elements that compose the Galaxy user interface:

[![Galaxy RNA workbench UI tour](https://i.imgur.com/c06O3I0.png)](https://www.youtube.com/watch?v=rP59wYIxWcI "Kitematic galaxy-rna-workbench launch")
