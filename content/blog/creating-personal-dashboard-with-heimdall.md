---
title: "Creating a personal dashboard with Heimdall"
description: "meta description"
image: "images/post/creating-personal-dashboard-with-heimdall/creating-personal-dashboard-with-heimdall.png"
date: 2022-03-29T12:00:00+02:00
categories: ["infraestructure"]
tags: ["dashboard","docker"]
type: "regular" # available types: [featured/regular]
draft: false
mermaid: false
---
In the last months I have been building a personal infraestrucure to make my day to day faster. To make it also simpler, I have decided to give a try to a dashboard called Heimdall. It is a self-hosted site that can have any number of links that can be either static or dynamic.

#### Installation

For the installation, I opted for the Docker version. I created a named volume for the service:
```sh
$ docker volume create heimdall
```

Then modified the default docker-compose configuration to use the volume:
```yaml
version: "2.1"
services:
  heimdall:
    image: linuxserver/heimdall
    container_name: heimdall
    volumes:
      - heimdall:/config
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    ports:
      - 80:80
      - 443:443
    restart: unless-stopped
volumes:
  heimdall:
    external: true
```

Launching the service is as easy as running `docker-compose up -d`. Service is available via https at the ip address of your host.

#### Creating the first links

Heimdall supports both websites and application links. Setting those up is as easy as:

- Selecting your favorite app in the drop down menu
- Configuring the URL of the app
- Selecting the color and icon

If you want it to appear on your dashboard, don't forget to mark it as pinned before saving.

This is my sample dashboard with a link to this blog and a link to my Gitea instance:
![Initial Heimdall dashboard](images/post/heimdall-dashboard-small.png)

Total setup time (including writting this post): less than 20 minutes!

_Photo by Luke Chesser on Unsplash_
