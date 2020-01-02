## Usage of SAIGE to analyze binary phenotypes

1. Install SAIGE

The most convenient way of running the software is through a docker image

* Pull the docker image
```
docker pull wzhou88/saige:0.36.2
docker run -it wzhou88/saige:0.36.2
```
The docker image is at https://github.com/weizhouUMICH/Docker/tree/master/SAIGE

Note: if you are compiling from the source code, take into account that the program is built for a Linux x86 machine

Try calling these functions to see if the program works

```
step1_fitNULLGLMM.R --help
step2_SPAtests.R --help
createSparseGRM.R --help
```

* Run the docker image and shared folders (container-data) with the host machine to upload input data

`mkdir container-data` (make sure the host has read-write permission to this folder)
`docker run --name test -dit -P -v  ~/container-data:/home wzhou88/saige:0.36.2` (*--name* followed by the name of the new container, -dit: *d* is detached mode, *it* ensures that bash or sh can be allocated to a pseudoterminal, *-P*: publishes the containers ports to the host, *-v* says that what follows is to be the volume, *wzhou88/saige:0.36.2* is the image to be used for the container). After using this command you will be given the container id, make sure you remember the first 4 characters.
You can also use `docker ps -a` to see a list of all running containers and get the container ID
Access the newly deployed container using `docker attach ID`

