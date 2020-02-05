### Running Elastic on Docker
###### The below guide aims to address how to run ELK on Docker.

##### STEP#0: Fulfill the Pre-requisites:
  - Installation of Git 
      - yum install git
  - Installation of Docker & Docker Compose
      - sudo apt-get install docker.io
      - sudo apt install docker-compose
      - docker --version
      - sudo systemctl enable docker
      - sudo systemctl status docker
      - sudo systemctl start docker 
      - sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
      - sudo chmod +x /usr/local/bin/docker-compose
      - docker-compose --version
##### STEP#1: Clone the  Docker ELK repository:
```
git clone https://github.com/caas/docker-elk.git
```
```
cd docker-elk
```
```
$ docker-compose up
```
##### STEP#2: Launch Elastic Search:
![image](https://user-images.githubusercontent.com/689226/73813078-f1adf500-4804-11ea-9e53-3000370bc93b.png)


##### STEP#3: Launch Kibana:
![image](https://user-images.githubusercontent.com/689226/73813189-3cc80800-4805-11ea-9ec3-d153691d5179.png)


<hr>

##### NOTE: In te above approach if the container is lost, the data inside the container is also lost. Let's see an another approach to address this problem, which is using Docker Volumes:

```
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 \
  -v $PWD/elasticsearch/data:/root/esvolume/data \
  -e "discovery.type=single-node" \
  --name elasticsearch \
  docker.elastic.co/elasticsearch/elasticsearch-oss:6.2.4
```
###### *Now, using the above command, the Volume is mounted on /root/esvolume/data*
<hr>
