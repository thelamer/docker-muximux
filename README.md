[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)](https://linuxserver.io)

The [LinuxServer.io](https://linuxserver.io) team brings you another container release featuring :-

 * regular and timely application updates
 * easy user mappings (PGID, PUID)
 * custom base image with s6 overlay
 * weekly base OS updates with common layers across the entire LinuxServer.io ecosystem to minimise space usage, down time and bandwidth
 * regular security updates

Find us at:
* [Discord](https://discord.gg/YWrKVTn) - realtime support / chat with the community and the team.
* [IRC](https://irc.linuxserver.io) - on freenode at `#linuxserver.io`. Our primary support channel is Discord.
* [Blog](https://blog.linuxserver.io) - all the things you can do with our containers including How-To guides, opinions and much more!

# [linuxserver/muximux](https://github.com/linuxserver/docker-muximux)
[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/muximux.svg)](https://microbadger.com/images/linuxserver/muximux "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/muximux.svg)](https://microbadger.com/images/linuxserver/muximux "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/muximux.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/muximux.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-muximux/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-muximux/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/muximux/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/muximux/latest/index.html)

[Muximux](https://github.com/mescon/Muximux) is a lightweight portal to view & manage your HTPC apps without having to run anything more than a PHP enabled webserver. With Muximux you don't need to keep multiple tabs open, or bookmark the URL to all of your apps.

[![muximux](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/muximux-icon.png)](https://github.com/mescon/Muximux)

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/muximux` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v7-latest |


## Usage

Here are some example snippets to help you get started creating a container.

### docker

```
docker create \
  --name=muximux \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -p 80:80 \
  -v <path to data>:/config \
  --restart unless-stopped \
  linuxserver/muximux
```


### docker-compose

Compatible with docker-compose v2 schemas.

```
---
version: "2"
services:
  muximux:
    image: linuxserver/muximux
    container_name: muximux
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    volumes:
      - <path to data>:/config
    ports:
      - 80:80
    restart: unless-stopped
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `-p 80` | WebUI |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Europe/London` | Specify a timezone to use EG Europe/London. |
| `-v /config` | Where muximux should store its files. |

## User / Group Identifiers

When using volumes (`-v` flags) permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id user` as below:

```
  $ id username
    uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```


&nbsp;
## Application Setup

Find the web interface at `<your-ip>:80` , set apps you wish to use with muximux via the webui.
More info:- [Muximux](https://github.com/mescon/Muximux)



## Support Info

* Shell access whilst the container is running: `docker exec -it muximux /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f muximux`
* container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' muximux`
* image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/muximux`

## Updating Info

Most of our images are static, versioned, and require an image update and container recreation to update the app inside. With some exceptions (ie. nextcloud, plex), we do not recommend or support updating apps inside the container. Please consult the [Application Setup](#application-setup) section above to see if it is recommended for the image.  
  
Below are the instructions for updating containers:  
  
### Via Docker Run/Create
* Update the image: `docker pull linuxserver/muximux`
* Stop the running container: `docker stop muximux`
* Delete the container: `docker rm muximux`
* Recreate a new container with the same docker create parameters as instructed above (if mapped correctly to a host folder, your `/config` folder and settings will be preserved)
* Start the new container: `docker start muximux`
* You can also remove the old dangling images: `docker image prune`

### Via Taisun auto-updater (especially useful if you don't remember the original parameters)
* Pull the latest image at its tag and replace it with the same env variables in one shot:
  ```
  docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock taisun/updater \
  --oneshot muximux
  ```
* You can also remove the old dangling images: `docker image prune`

### Via Docker Compose
* Update all images: `docker-compose pull`
  * or update a single image: `docker-compose pull muximux`
* Let compose update all containers as necessary: `docker-compose up -d`
  * or update a single container: `docker-compose up -d muximux`
* You can also remove the old dangling images: `docker image prune`

## Versions

* **23.03.19:** - Switching to new Base images, shift to arm32v7 tag.
* **22.02.19:** - Rebasing to alpine 3.9.
* **16.01.19:** - Add pipeline logic and multi arch.
* **13.09.18:** - Rebase to alpine 3.8.
* **09.01.18:** - Rebase to alpine 3.7.
* **25.05.17:** - Rebase to alpine 3.6.
* **12.02.17:** - Rebase to alpine 3.5.
* **14.10.16:** - Add version layer information.
* **30.09.16:** - Rebase to alpine linux.
* **09.09.16:** - Add badges to README.
* **22.02.16:** - Initial release date.
