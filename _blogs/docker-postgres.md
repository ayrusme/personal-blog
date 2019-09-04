---
title:  "Persist Postgres Data with Docker"
date:   2019-09-04
permalink: docker-postgres-volume
categories: ["docker"]
layout: blog_post
---
# How to persist data in Docker 🐳

---

NOTE: This post is more of a `How I learnt to persist data in docker`

Like every other person using postgis on postgres docker, I pulled the mdillon postgis image and started it.

My initial command was really simple

`
docker run -d -t -p 5432:5432 -e POSTGRES_USER=suryaraman -e POSTGRES_DB=test mdillon/postgis
`

Great! That got the postgres DB up and I was able to connect to it 🍾

### Restart Always ♻️

However, there was a catch. Whenever I restarted the docker daemon or my computer, the container running postgres went missing. After going through the [docker docs](https://docs.docker.com/config/containers/start-containers-automatically/), I learnt that whenever we are running a container, the default policy is to not restart it unless explicitly specified. 

Upon adding the flag to restart, the docker command looked like 

`
docker run -d -t -p 5432:5432 -e POSTGRES_USER=suryaraman -e POSTGRES_DB=test --restart always mdillon/postgis
`

This made sure that whenever my computer/docker daemon/container restarted, the container was running. 

Now, Onto the real problem!

### Persisting Data to Local Machine 💻

I wanted to save the postgres data to my local machine so that I didn't have to create the same data over and over. In a production scenario, this would mean that the data would be lost if Server/Docker/Container restarts or an update is applied.

The solution would be to make the container store the data in a different place. Whenever we initialise a container without a volume, a temporary volume is created for the container where the data is stored.

By default, postgres stores the data into `/var/lib/postgresql/data` unless explicitly specified with PGDATA. We just need to move this data elsewhere into the host machine so that the data persists.

### Enter Docker Volume 💿

We can pass a flag -v to the docker run command to specify a location into your host machine for the data. This can be any place that you want the data to be in. As long as sufficient permissions are available for the docker container, it can persist the data onto the speicified location. 

I chose to keep the data in my Desktop. The -v flag is twofold, you can specify where you want to keep the data, and you can speicfy which data to keep. ( the file path, of course ) seperated by a colon

This would mean that the argument for the -v flag would be `host/system/path:docker/container/path`

My docker command now looks like 

`
docker run -d -t -p 5432:5432 -e POSTGRES_USER=suryaraman -e POSTGRES_DB=test --restart always --name test -v ~/Desktop/postgres/local_data/:/var/lib/postgresql/data mdillon/postgis
`

This did write the data onto my local machine, and it PERSISTED!

## Final Remarks 🍻

I am quite new to Docker, and still learning about it. If you feel some of the information in this blog is not accurate, [tell me about it](mailto:hello@suryaraman.blog)