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

`git clone https://github.com/maismuka/elk-docker-vps.git`

`cd /elk-docker-vps`

 `tar -xvzf filebeat-6.5.4-linux-arm.tar.gz`
 
 After successfully extract the file, proceed to the folder `cd /filebeat-6.5.4-linux-arm`
 
 Change the file `filebeat.yml` with `nano filebeat.yml`
 
```
#=========================== Filebeat inputs =============================

filebeat.inputs:

# Each - is an input. Most options can be set at the input level, so
# you can use different inputs for various configurations.
# Below are the input specific configurations.

- type: log

  # Change to true to enable this input configuration.
  enabled: true

  # Paths that should be crawled and fetched. Glob based paths.
  paths:
    - /var/log/suricata/eve.json
    - /var/log/suricata/fast.log
    #- c:\programdata\elasticsearch\logs\*
.
.
.
    #----------------------------- Logstash output --------------------------------
output.logstash:
  # The Logstash hosts
  hosts: ["128.199.159.207:5000"]
.
.
.
```
 
 ######You may use whatever input file that you want, as long as it is shipable by Filebeat.
 
 ######For Raspberry Pi, the log cannot be received by Elasticsearch, so I changed to Logstash instead.
 
 ######But in my VmWare Ubuntu, using Elasticsearch is no problem and more secured.
 
 Save and exit.
 
 Run the filebeat by command `./filebeat -c filebeat.yml -e`
 
 `-c` is configuration file
 `-e` is display the log
 
 Be remember that, next time to run the Filebeat is to change directory to the Filebeat current folder. It doesn't matter if change the folder name, but to execute `./filebeat ` is to be in the same directory.
 
Here, maybe Filebeat crawl into unexisted or empty log file and folder (depends on the `filebeat.yml`)

You just need to install some data log collection program. For me, Suricata is enough for producing `eve.json`

On the second tought, I will be exploring Metricbeat for shipping the localhost's device status like CPU, RAM and temperature.

For now, the Suricata it is!

## 4. Suricata

The main purpose for this Suricata installation is to provide log, you may use any other software as long as the produce `.log` or `.json` which is requireq for the Filebeat shipping mechanism.

Since this is ARM machine, no Suricata repository can be add.

Proceed with basic apt install

`apt install suricata suricata-update`

Clone this git `git clone https://github.com/401trg/utilities/blob/master/suricata_et_rule_update.py`

And run `python3 suricata_et_rule_update.py`

This will download rules and configuration files from Emerging Threat's open ruleset and move them to /etc/suricata/. It also enabled informational rules in EmergingThreat's included configuration file

Then update the rules, run `suricata-update`

View available sources suricata-update list-sources. The results is somewhat as below;

```
15/2/2020 -- 07:15:16 - <Info> -- Using data-directory /var/lib/suricata.
15/2/2020 -- 07:15:16 - <Info> -- Using Suricata configuration /etc/suricata/suricata.yaml
15/2/2020 -- 07:15:16 - <Info> -- Using /etc/suricata/rules for Suricata provided rules.
15/2/2020 -- 07:15:16 - <Info> -- Found Suricata version 4.1.2 at /usr/bin/suricata.
Name: et/open
  Vendor: Proofpoint
  Summary: Emerging Threats Open Ruleset
  License: MIT
Name: et/pro
  Vendor: Proofpoint
  Summary: Emerging Threats Pro Ruleset
  License: Commercial
  Replaces: et/open
  Parameters: secret-code
  Subscription: https://www.proofpoint.com/us/threat-insight/et-pro-ruleset
Name: oisf/trafficid
  Vendor: OISF
  Summary: Suricata Traffic ID ruleset
  License: MIT
  .
  .
  .
  ```
  
Proceed to enable anything you want, for example `suricata-update enable-source oisf/trafficid`

Redo untill satisfy.

Lastly, to rerun `suricata-update` to verify changes.

Restart the Suricata.

`cd /etc/suricata suricata -i eth0`

ITS DONE!
 
 
 
 
 
 
 
 


