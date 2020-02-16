# elk-docker-vps

## 1. Create new VPS [DigitalOcean]

## 2. Docker

https://elk-docker.readthedocs.io/

Full docker [kibana elasticsearch logstash filebeat] `sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk `

Half docker [kibana elasticsearch logstash] `sudo docker run -p 5601:5601 -p 9200:9200 -it --name elk sebp/elk `
