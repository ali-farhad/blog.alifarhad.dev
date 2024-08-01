---
author: ["alifarhad"]

title: "Dockerizing an old node app"

date: "2024-08-01"

description: "Dockerizing old node apps is quite fun when you know what you are doing!"

summary: "Dockerizing old node apps is quite fun when you know what you are doing!"

tags: ["docker", "node", "dockerize", "Dockerfile"]

categories: ["docker", "node", "DockerFile"]

series: ["docker guide"]

ShowToc: true

TocOpen: false
---

### what is up with dockerzing old node apps?

Hello good ppl!

so as you know, I have been dabling with docker a lot since lately so today I found a really (ancient) old node app, and thought it would be fun to dockerize it. a little fun and a little challenge should be enough to level up my bad docker skills?

### I won but it took a lot!

when I started, I had an idea that it would not be easy task, but thanks to mother AI age, and MS copilot pro, I was able to go through a few iterations and finally got the entire app working.

Truth be told, if it was not for CoPilot Pro, I might as well have wasted 10 more hours trying to get things rights. so here is the summary of most challenging things I encountered on this jounery:

### Elephant in the ~Room~ Docker?

1. This app takes a gif file and outputs a cartoonish version of the same gif using `GraphicsMagick` binary.

2. so I had to figur out a way to install that binary manually into the app using `apk` file manager as i was using alpine linux as base image.

3. the 2nd challenge was the fact that app was using two folders i.e /in and /out for scanning and outputting gifs. thanks to docker mount-paths, I was able to get them working with no truble!

4. the 3rd and biggest problem of them all was that this app was clearly made way before docker-age, so it was using `winston` to log all logs into files on disk instead of `stdout` and `stderr`. we need the logs to go there if we want to use `docker log` or see any log on the screen. thanks to AI, i was able to actually create those files / folders into the container using `mkdir` and then hard link them directly to stdout and stderror. the command to achieve that can be seen down below in Dockerfile code.

5. that's it folks! once i got the container compiled, i was able to run it just fine. you can check the dockerFile and run command down below

```sh

FROM node:8-alpine

# Install GraphicsMagick
RUN apk update && apk add --no-cache graphicsmagick

ENV CHARCOL_FACTOR=0.8

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN mkdir -p /app/logs \
    && touch /app/logs/combined.log /app/logs/error.log \
    && ln -sf /dev/stdout /app/logs/combined.log \
    && ln -sf /dev/stderr /app/logs/error.log


RUN ln -sf /dev/stdout /app/logs/combined.log \
    && ln -sf /dev/stderr /app/logs/error.log

CMD ["node", "index.js"]

```

### command to create the container

`docker build -t oldnodeapp .`

### command to run the container

`docker run -it --rm -v "$(pwd)/in:/app/in" -v "$(pwd)/out:/app/out" oldnodeapp`
