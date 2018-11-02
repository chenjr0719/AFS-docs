# Developer Guildlines

## How to use the private registry




## Using Docker to Install Inference Engine
The method of install inference engine by using docker is introduced in the section. Suppose the user have the basic knowledge about docker. About more commands of docker, please refer the [docker docs](https://docs.docker.com/engine/reference/commandline/cli/) 

Pre-condition:
* The docker image must be uploaded to the private registry.

1. Login the private registry.
	```
	$ docker login [IP of private registry: port] -u [username] -p [password]
	```

2. Download the docker image file from private registry.
	```
	$ docker pull [IP of private registry: port]/inference
	```

3. Run the image to derive the container from.
	```
	$ docker run -it -d -p 7500:7500 --name inference [IP of private registry: port]/inference python3 /root/inference_engine/inference.py
	```

4. Startup the container for inference engine.
	```
	$ docker start inference
	```