# docker-blurrily-uuid
Simple docker image for running the awesomeness that is [Blurrily](https://github.com/mezis/blurrily) as a container.

> Note: This fork is the [UUID based version](https://github.com/StitchedCode/blurrily), see [mrmattwright/docker-blurrily](https://github.com/mrmattwright/docker-blurrily) for the docker of the original integer based version.

> Note: Support restricted to UUID v4, beginning with a non-zero (Line 66 in [uuid.parse](http://fossies.org/dox/e2fsprogs-1.42.13/uuid_2parse_8c_source.html) seems to be at fault?).

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
> client.put('London', '10000000-0000-4000-A000-000000001337')
```
Recover a reference from the haystack:
```
> client.find('lonndon')
```
