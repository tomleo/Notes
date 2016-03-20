sudo docker	-d -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375
docker run --rm -ti ubuntu:latest /bin/bash

git clone https://github.com/spkane/docker-node-hello.git
docker run -d -p 8080:8080 example/docker-node-hello:latest

