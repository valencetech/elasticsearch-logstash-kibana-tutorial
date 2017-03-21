Amazon Linux AMI 2015.09

Elasticsearch 5.2.2
===================

Commands
--------
sudo su
yum update -y
cd /root
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.2.rpm
yum install elasticsearch-5.2.2.rpm -y
rm -f elasticsearch-5.2.2.rpm
cd /usr/share/elasticsearch/
./bin/elasticsearch-plugin -install mobz/elasticsearch-head
./bin/elasticsearch-plugin -install lukas-vlcek/bigdesk
./bin/elasticsearch-plugin install https://artifacts.elastic.co/downloads/elasticsearch-plugins/discovery-ec2/discovery-ec2-5.2.2.zip
./bin/plugin --install lmenezes/elasticsearch-kopf/2.1.1
cd /etc/elasticsearch
nano elasticsearch.yml

Config
cluster.name: ELK
cloud.aws.access_key: ACCESS_KEY
cloud.aws.secret_key: SECRET
cloud.aws.region: eu-west-2
discovery.type: ec2
discovery.ec2.tag.Name: "ELK"
http.cors.enabled: true
http.cors.allow-origin: "*"

path.data: /tmp/elasticsearch/workdir 
path.work: /tmp/elasticsearch/workdir
path.logs: /var/log/elasticsearch 
path.conf: /etc/elasticsearch 

chown -R elasticsearch:elasticsearch /tmp/elasticsearch/data
chown -R elasticsearch:elasticsearch /tmp/elasticsearch/workdir

Commands
service elasticsearch start

Logstash 5.2.2
===============
Commands
sudo su
yum update -y
cd /root
wget https://artifacts.elastic.co/downloads/logstash/logstash-5.2.2.rpm
yum install logstash-5.2.2.rpm -y
rm -f logstash-5.2.2.rpm
nano /etc/logstash/conf.d/logstash.conf
Config
input { file { path => "/tmp/logstash.txt" } } output { elasticsearch { host => "ELASTICSEARCH_URL_HERE" protocol => "http" } }
Commands
service logstash start

Kibana 5.2.2
============
Commands
sudo su
yum update -y
cd /root
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.2.2-linux-x86_64.tar.gz
tar xzf kibana-5.2.2-linux-x86_64.tar.gz
rm -f kibana-5.2.2-linux-x86_64.tar.gz
cd kibana-5.2.2-linux-x86_64
nano config/kibana.yml
Config
elasticsearch_url: "ELASTICSEARCH_URL_HERE"
Commands
nohup ./bin/kibana &
Navigate In Browser
http://KIBANA_URL:5601/
