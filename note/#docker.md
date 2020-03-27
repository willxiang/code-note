docker 命令

[TOC]



##### mysql
```commonlisp
docker run --name mysql_name -e MYSQL_ROOT_PASSWORD=root -d -p 3309:3306 mysql:latest
```

---

##### elasticsearch
es和kibana结合使用，需要使用同一个`network`
```commonlisp
docker network create somenetwork

docker run -d --name es2 --net elastic2 -p 9201:9200 -p 9301:9300 -e "discovery.type=single-node" elasticsearch:latest

```


##### kibana
```commonlisp
docker run -d --name kibana2 --net elastic2 -e ELASTICSEARCH_URL=http://192.168.137.150:9201 -p 5602:5601 kibana:latest
```



##### docker 镜像加速配置

```commonlisp
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://dockerhub.azk8s.cn","https://reg-mirror.qiniu.com","https://tjmi15yr.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload
sudo systemctl restart docker
```



