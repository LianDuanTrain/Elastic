# Install ELK (Elasticsearch, Kibana, and Filebeat) using Docker
## Topics
- Prerequisite Check 
- Target ELK version
- Install ELK
  - Elasticsearch
  - Kibana
  - kscarlett/nginx-log-generator
  - Filebeat
- Explore Nginx Access Log in Kibana

## Prerequisite Check 
- OS version - Oracle Linux Server release 8.4   
  -`cat /etc/oracle-release`
- Docker Version - 20.10.6  
  -`docker -v`   
- VSCode Plugin - Docker V 1.18.0  
## Target ELK version
- Version is 7.15.2     
- https://www.docker.elastic.co/  
## Install ELK
  ### Create a Docker Network
 -  `docker network create elastic`  
  ### Install Elasticsearch
  - `docker run -d --name es-node-01 --net elastic --rm -p 127.0.0.1:9200:9200 -p 127.0.0.1:9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.15.2`
  - `curl localhost:9200`
  ### Install Kibana
  - `docker run -d --name kibana-01 --net elastic --rm -p 127.0.0.1:5601:5601 -e "ELASTICSEARCH_HOSTS=http://es-node-01:9200" docker.elastic.co/kibana/kibana:7.15.2`
  ### Install kscarlett/nginx-log-generator
  - `docker run --name nginxLogGenerator --rm --label type=nginxLog -d -e "RATE=10" kscarlett/nginx-log-generator:sha-5416ec2`    
  ### Install Filebeat
  ```
    docker run -d \
      --name=filebeat \
      --net=elastic \
      --user=root \
      --rm \
      --volume="$(pwd)/filebeat.docker.yml:/usr/share/filebeat/filebeat.yml:ro" \
      --volume="/var/lib/docker/containers:/var/lib/docker/containers:ro" \
      --volume="/var/run/docker.sock:/var/run/docker.sock:ro" \
        docker.elastic.co/beats/filebeat:7.15.2  -e -strict.perms=false 
  ```  
- https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-reference-yml.html  
## Explore Nginx Access Log in Kibana
- Stack Management
  - Kibana => Index patterns
  - Data => Index Management
- Analytics
  - Discover
  - Dashboard
## Summary  
- Prerequisite Check 
- Target ELK version
- Install ELK
  - Elasticsearch
  - Kibana
  - kscarlett/nginx-log-generator
  - FileBeat
- Explore Nginx Access Log in Kibana










