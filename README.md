# docker-blurrily
Simple docker image for running Blurrily as a container

Welcome to the future. Go install docker. [Docker MacOS Instructions](https://docs.docker.com/installation/mac/)

You read them right? Ok, I know you didn't but let's just go for it...
```
boot2docker init
```
or if you have power:
```
boot2docker init --memory=4096
```
One time. Then:
```
boot2docker start
```
then this guy:
```
$(boot2docker shellinit)
```

Start blurrily as a docker container? 
```
docker run  --env DOCKER_HOST=$DOCKER_HOST --env DOCKER_TLS_VERIFY=$DOCKER_TLS_VERIFY --env DOCKER_CERT_PATH=/docker/cert -v $DOCKER_CERT_PATH:/docker/cert -p "12021:12021" -ti -d mrmattwright/docker-blurrily blurrily
```

Now to avoid port forwarding hell we will just look for the docker host:
```
echo $DOCKER_HOST
tcp://192.168.59.105:2376
```
take the IP address and then use that to connect to blurrily:
```
irb -rubygems
> require 'blurrily/client'
> config = { :host => "192.168.59.105", :port => "12021"}
> client = Blurrily::Client.new(config)
```
Store a needle with a reference:
```
> client.put('London', 1337)
```
Recover a reference form the haystack:
```
> client.find('lonndon')
```
