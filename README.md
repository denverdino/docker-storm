# docker-storm

Apache Storm in Docker

## Deploy in local environment

You can deploy the storm and application in local Docker environment

#### Create the custom test network for Apache Storm

```
docker network create multi-host-network
```

#### Build Docker Image from storm-starter example topology  

Example from [https://github.com/apache/storm/tree/master/examples/storm-starter](https://github.com/apache/storm/tree/master/examples/storm-starter)

```
cd local
docker-compose build
```

#### Create the docker-compose project for Apache Storm


```
docker-compose up -d
docker-compose ps
```

When the "ui" container is started, launch the Storm UI with browser "http://192.168.99.100:8080/index.html"

**NOTE**: Please use the IP address from ```docker-machine ip default``` to replace the "192.168.99.100" according to your local docker configuration

#### Scale out supervisors

You can start more than one supervisors with following command, e.g. for 3 instances.

```
docker-compose scale supervisor=3
```

#### Submit topology deployment

For the "topology" container must start after "nimbus" is fully started. It maybe failed in the initial deployment. And you can run the following command to submit topology deployment with 

```
docker-compose start topology
```

And you can check the topology status with Storm UI 

#### Clean up

```
docker-compose stop
docker-compose rm -f
```

## Deploy in Aliyun Container Service

You can deploy the storm and application in Aliyun Container Service with docker-compose template in "/aliyun". Such template has some extension on the one for local environment.

**Prerequestite:** You need to create a custer with at least 3 nodes.
 

**NOTE:** The cool stuff in Aliyun Container Extension

* ```aliyun.routing.port_8080: storm-ui``` Specify the Storm UI endpoint for internet access
* ```aliyun.scale: "3"``` Specify the "Supervisor" status automatically
* ```aliyun.probe.url: tcp://container:6627``` Detect the health status of "Nimbus"
* ```aliyun.depends: nimbus``` Make topology container start after "nimbus"" is ready for business


## Optional: build docker images

```
./build.sh
```


## References

* [Tutorial: Deploying Apache Storm on Docker Swarm](http://blog.baqend.com/post/142795871760/tutorial-deploying-apache-storm-on-docker-swarm)
	* https://github.com/Baqend/docker-storm
* [学习Docker容器网络模型 - 搭建分布式Zookeeper集群](https://yq.aliyun.com/articles/30328)
	* https://github.com/denverdino/docker-zookeeper
