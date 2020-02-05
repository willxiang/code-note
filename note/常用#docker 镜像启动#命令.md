一些常用docker 镜像启动命令

##### mysql
```
docker run --name mysql_name -e MYSQL_ROOT_PASSWORD=root -d -p 3309:3306 mysql:latest
```

---

##### elasticsearch
es和kibana结合使用，需要使用同一个`network`
```
docker network create somenetwork

docker run -d --name es2 --net elastic2 -p 9201:9200 -p 9301:9300 -e "discovery.type=single-node" elasticsearch:latest

```


##### kibana
```
docker run -d --name kibana2 --net elastic2 -e ELASTICSEARCH_URL=http://192.168.137.150:9201 -p 5602:5601 kibana:latest
```