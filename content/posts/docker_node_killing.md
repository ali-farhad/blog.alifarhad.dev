---
author: ["alifarhad"]

title: "A Case of Immortal Node Container?"

date: "2024-07-27"

description: "Containers running node images need special syntax and / or commands to gracefully shutdown"

summary: "Containers running node images need special syntax and / or commands to gracefully shutdown"

tags: ["docker", "node", "tini"]

categories: ["docker", "node"]

series: ["docker guide"]

ShowToc: true

TocOpen: false
---

### node process is imortal inside docker containers

today, when I was going through my docker adventures, I learned a rather curious things. it's that when running node inside a docker container, you need special syntax or a binary if you want it to gracefully close and shutdown properly.

Now, I know that it's pretty typical for us to simply `ctrl + c` out of container, when we are done experimenting, and that **usually** shut downs the container, but this is different with `node` containers. Pressing `ctrl + c` on a node container only makes it look like you closed / killed the container, but in reality it's actually still running in docker engine setup. you can confirm it by running `docker ps` inside your terminal or from docker GUI. so what's going on with node containers?

### 3 Methods to kill them all

1.  the simplest and easiest of them all is to attach a `- - init` flag into your docker run command. this shims a small binary called `tini` into container, so that when you press `ctrl + c` it makes sure that container is indeeded closed / shut down
2.  if you can control DockerFile or docker image generation setup for your node app, you can install `tini` image right into your apline or other underlying OS and use `entrypoint` instruction to scaffold entire app via this `tini` process. this will again make sure that closing container will in fact kill the app. (down below you can find actual instrutions in a sample DockerFile)
3.  Edit your node app so that it can detect closing signal from cotainer and handle graceful shutdown. This is by far the most approporite and practical method for any practical/ real-world node app.You can use this function to properly cleanup and or do other essential tasks before the app is killed forever.

### Sample DockerFile

```{hl_lines=[5,15]}

FROM node:10.15-alpine

EXPOSE 3000

RUN apk add --no-cache tini

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install && npm cache clean --force

COPY . .

ENTRYPOINT ["tini", "--"]

CMD ["node", "app.js"]
```

### Closing Remarks

That's it for today. happy killing node apps! 🫡
