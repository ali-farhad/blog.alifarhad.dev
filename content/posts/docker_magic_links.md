---
author: ["alifarhad"]

title: "how to access docker containers with pretty urls locally?"

date: "2024-09-08"

description: "when running multiple Docker containers, you can access them all using pretty URLS"

summary: "hen running multiple Docker containers, you can access them all using pretty URLS"

tags: ["docker", "node", "dockerize", "Dockerfile", "docker networking']

categories: ["docker", "node", "DockerFile]

series: ["docker guide"]

ShowToc: true

TocOpen: false
---

### What are pretty URLS?

when we are running multiple docker containers or let's just call them "services", one problem becomes pretty obvious that accessing them all becomes really complicated and you need to remember different ports if you want to access them.

the trouble starts when you implement a rather large number of services, since our docker engine is running locally on a single host, we have to use the same IP to access all the active/running services. so for example, if you are running 3 different containers exposing 3 different PORTS such as `5050`, `8080`, `8070' etc then in order to access these, you will need to remember which port belongs to which container. This is stuff of the nightmares!

### Enter Pretty URLS

pretty URLs is a concept where we can use random but easy-to-remember URLS and assign them to each container so next time we need to access this container, we can access it by going to the assigned pretty URL. .e.g. if you have an `nginx` container running, you can simply access it on the following url : `mywebserver.localhost` this will work as long as the container and the host machine is on the same network.

### YML code for getting pretty URLS!

```sh

version: '2.4'

services:
  nginx-proxy:
    image: nginxproxy/nginx-proxy:1.6-alpine
    ports:
      - "80:80"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock

  nginx:
    image: nginx:1.27.0-alpine
    environment:
      - VIRTUAL_HOST=dude.localhost

  nginx2:
    image: nginx:1.27.0-alpine
    environment:
      - VIRTUAL_HOST=ghost.localhost

```

### Explanation

so this `docker compose` uses a 3rd party image that's built on top of `nginx` to automtically configure pretty URLS. when this script runs, it starts 3 containers, where the 1st is the setup container and the rest of the 2 containers can be accessed using their respective `Virtual Host` adresses. this uses reverse proxy configruation built on top of `nginx` webserver.

that's it for now! Happy Pretty URL-ing your containers!
