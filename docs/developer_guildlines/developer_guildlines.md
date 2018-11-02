# Developer Guildlines

## How to use the private registry




## Using Docker to Install Inference Engine
The method of install inference engine by using docker is introduced in the section. Suppose the user have the basic knowledge about docker. About more commands of docker, please refer the [docker offical website](https://docs.docker.com/engine/reference/commandline/cli/) 



$ docker login [IP of private registry: port] -u [username] -p [password]
$ docker pull [IP of private registry: port]/inference
$ docker run -it -d -p 7500:7500 --name inference [IP of private registry: port]/inference python3 /root/inference_engine/inference.py
$ docker start inference