---
title: 1. Getting Started.
---

# Getting Started #

This section details everything you need to know to get Penpot up and
running in production environments. Although it can be installed in
many ways, the recommended approach is using **docker** and
**docker-compose**.


## Install Docker ##

**Skip this section if you already have docker installed, up and running.**


Probably the best approach to install docker is following the official docker
installation guide: https://docs.docker.com/engine/install/

Optionally, after installing docker, you can tweak your system for
avoid constanly using of **sudo**. The easy, single step way is
executing the following command that will add the current user to the
docker group:

```bash
sudo usermod -aG docker $USER
```

*NOTE*: probably you will need to re-login again to make this change take effect.


For more advanced setup, docker already has a guide for [rootless docker][1]

[1]: https://docs.docker.com/engine/security/rootless/



## Start Penpot ##

As first step you will need to obtain the `docker-compose.yaml`
file. You can create it from scratch or start with the [default
one][2] from penpot repository:

[2]: https://raw.githubusercontent.com/penpot/penpot/main/docker/images/docker-compose.yaml

```bash
wget https://raw.githubusercontent.com/penpot/penpot/main/docker/images/docker-compose.yaml
wget https://raw.githubusercontent.com/penpot/penpot/main/docker/images/config.env
```

The configuration is defined using environment variables in the
`config.env` file and the downloaded file already comes with the
essential variables already set and other commented with appropriate
comments.

```bash
docker-compose -p penpot -f docker-compose.yaml up -d
```

This will start listening on http://localhost:9001


## Configuration ##

If you started the application without configuring SMTP, you probably
need to create a user in order be able login into the application. You
can create an additional, already activated user using this command:

```bash
docker exec -ti penpot-penpot-backend-1 ./manage.sh create-profile
```

In general, the application is ready to be used without email
configuration; for example when no smtp configuration is found you
should be able to change the user email (in profile section) without
the email validation step.

For more advanced setups, look a the [Configuration][3] section.

[3]: /technical-guide/configuration/


## Update Penpot ##

To get the latest version of Penpot in your local installation, you just
need to execute:

```bash
docker-compose -f docker-compose.yaml pull
```

This will fetch the latest images. When you do docker-compose up
again, the containers will be recreated with the latest version.

