# docker-zipkin
reference :
https://github.com/openzipkin/docker-zipkin


## Usage

```
# If docker is running on your host machine, adjust the kernel setting directly
$ sudo sysctl -w vm.max_map_count=262144

# If using docker-machine/Docker Toolbox/Boot2Docker, remotely adjust the same
$ docker-machine ssh default "sudo sysctl -w vm.max_map_count=262144"

$ docker-compose -f docker-compose.yml -f docker-compose-elasticsearch.yml up
```

## clean volume 
List:
```
$ docker volume ls -qf dangling=true
```

Cleanup:
```
$ docker volume rm $(docker volume ls -qf dangling=true)
```


## max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
切换到root用户，然后
```
vim /etc/security/limits.conf
* soft nofile 300000
* hard nofile 300000
* soft nproc 102400
* soft memlock unlimited
* hard memlock unlimited
 
```



## max virtual memory areas vm.max_map_count [65530] is too low, 

increase to at least [262144]
先要切换到root用户；
然后可以执行以下命令，设置 vm.max_map_count ，但是重启后又会恢复为原值。

```
sysctl -w vm.max_map_count=262144
```

持久性的做法是在 /etc/sysctl.conf 文件中修改 vm.max_map_count 参数：
```
echo "vm.max_map_count=262144" > /etc/sysctl.conf
sysctl -p
```

## Export Or Load

Save all images with name:tag to one tar file:

```bash
docker save $(docker images | sed '1d' | awk '{print $1 ":" $2 }') -o allinone.tar
```

Then, load all images:

```bash
docker load -i allinone.tar
```
