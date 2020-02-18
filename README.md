# elk-docker-vps

## 1. Create new VPS [DigitalOcean]

## 2. Docker

https://logz.io/blog/elk-stack-on-docker/ - From here, it's pretty much complete on how to first time setup the ELK Docker.

But, for that, we need to install Docker and Docker-compose

From your Ubuntu machine, use  `apt install docker.io`

After that proceed to install Docker-compose. 

`git clone https://github.com/deviantony/docker-elk.git`

```
remote: Counting objects: 1112, done.
remote: Total 1112 (delta 0), reused 0 (delta 0), pack-reused 1112
Receiving objects: 100% (1112/1112), 234.87 KiB | 84.00 KiB/s, done.
Resolving deltas: 100% (414/414), done.
Checking connectivity... done.
```

Go into the newly created folder `cd  /docker/elk/`

Simply run `docker-compose up -d` to start install the Docker.

```
Creating docker-elk_elasticsearch_1 ... done
Creating docker-elk_logstash_1      ... done
Creating docker-elk_kibana_1        ... done
Attaching to docker-elk_elasticsearch_1, docker-elk_kibana_1, docker-elk_logstash_1
.
.
.
```

After a few minutes, depending on the network speed (usually VPS network speed are high and not consuming much time) the setup is complete!

`logstash_1       | [2020-02-18T05:02:00,421][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600}`

The logs keep coming but never mind that.

## 3. Filebeat

This installation is very tricky depending on what machine you work on.

If you intended on working only Ubuntu machine, simply refer to the original logz.io website https://www.elastic.co/guide/en/beats/filebeat/current/setup-repositories.html

But for me, installing on Raspberry Pi is a different story.

TLDR; I provide filebeat file tar.gz for you to be extracted and ready to use. Less hassle for compile from scratch




