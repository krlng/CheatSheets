#Docker

```sh
docker ps -a | awk '{if(NR>1) print $NF}'
```

##Docker-Machine
```sh
docker-machine create -d virtualbox --virtualbox-memory 4096 default

# Change settings afterwards
docker-machine stop
VBoxManage modifyvm default --cpus 2
VBoxManage modifyvm default --memory 4096
docker-machine start
```

##Docker-Compose
```sh
docker ps -a | awk '{if(NR>1) print $NF}'
```
