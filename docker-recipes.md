# Creating Docker container

Docker is a service for finding and sharing container images with your team

## Docker installation

For instaling Docker Desktop for MacOs click the link [Docker](https://docs.docker.com/docker-for-mac/install/) and follow instructions
  
## Docker usage

1. Sign up for Docker
2. Create a new repository on the Docker webpage
3. Open the terminal and sign in to Docker Hub on your computer
    `$ docker login`
4. Build and push a container image to Docker Hub from your computer
    * Create a dockerfile with your application
        
            cat > Dockerfile <<EOF 
            from busybox 
            CMD echo "Hello world! This is my first docker image." 
            EOF
            
    * Build a docker image
          
          docker build -t <your_username>/my-first-repo .
          docker build -t dianacornejo/new-repo . 
          
## Useful docker commands to keep in mind

* `docker log in ` User:**statisticalgenetics** Password:**##Wombat19##**

* `docker build -t NAME[:TAG] -f [PATH/dockerfile name] .`. Example: `docker build -t statisticalgenetics/nps-1.1:1.1 -f ./nps.dockerfile .`

* `docker push OPTIONS NAME[:TAG]`. Example: `docker push statisticalgenetics/nps1.1` or `docker push statisticalgenetics/nps-1.1:1.1` to push an specific tagged version

You can build the docker containers if you have the statgen-setup script created by Gao simply writing:

* `statgen-setup build --tutorial nps --tag <version number>`. Using this script  the image is build and pushed to dockerhub so no further steps are necessary.

* `statgen-setup login --tutorial [tutorial name] --my-name [diana]`. Example: `statgen-setup login --tutorial nps --my-name diana` Use this command to run a specific tutorial. Don't forget to use a unique name.

* `statgen-setup clean`. To shutdown all containers and clean up the dangling ones. Run as a root. You should do this before building a new container and running it, so the changes in the dockerfile can take place. 

* `docker pull NAME[:TAG]`. Example: `docker pull statisticalgenetics/nps-1.1:1.1` This command pulls an image from a repository

* `docker run OPTIONS NAME[:TAG]`. Example: `docker run -it statisticalgenetics/nps-1.1:1.1` -i: interactive (Keep STDIN open even if not attached) and -t: Allocate a pseudo-TTY. 

* `docker images ls` To list all the created images

* `docker image rmi [image-number]` To delete the image you are specifying, option -f to force removal

* `docker ps -a` For a list of all running containers

* `docker stop [container id]` To stop a specific container

* `docker container stop $(docker container ls -aq)` To stop all running containers

            
## Creating a container for annovar in statgen-courses

Being in the ~/statgen-course/docker folder run the following command:

1. **Create the DockerFile as the example below:** `vim annovar.dockerfile`

`FROM ubuntu:latest` **This command specifies de base image to derive from**

`MAINTAINER Diana Cornejo <dmc2245@cumc.columbia.edu>` **This command indicates who created the image**

`WORKDIR /root` **This command is to specify the working directory**

`#Install dependency tools and deploy data-set package that Carl made
RUN echo "deb [trusted=yes] http://statgen.us/deb ./" | tee -a /etc/apt/sources.list.d/statgen.list && \
    apt-get update && \
    apt-get install -y annotation-tutorial && \
    apt-get clean`
    
**The above commands indicate the .deb package to install in this case annovar from what Carl had prepared earlier http://statgen.us/deb/**

`## Update the exercise text
ARG DUMMY=unknown
RUN DUMMY=${DUMMY} curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/FunctionalAnnotation.2019.docx`

2. **Build the docker image**: You have to specify the path where your dockerfile is located to be able to create the image

`docker build -t statisticalgenetics/annovar -f ./annovar.dockerfile .` -t indicates tag, -f the file from which you wish to creater the docker image and the . to create the image. 

`docker build -t statisticalgenetics/annovar:0.1.0 -f ./annovar.dockerfile .` If you want to tag the docker image add : and then the version number

`docker build --build-arg DUMMY='date +%s' -t statisticalgenetics/nps1.1 -f docker/nps.dockerfile docker` You can also set build-time varirables and specify them when creating the image. 

3. **Push built image into dockerhub**: That way anyone can pull the image and use it

`docker push statisticalgenetics/annovar` Without tags
`docker push statisticalgenetics/nps1.1:1.1` With tags

`statgen-setup login --tutorial annovar --my-name diana` This step will allow you to run the different tutorials on the cloud

4. **Test the command line**

Start the tutorial as is written in the exercise

## Creating a docker container for the Regression exercise. 

This is based on Carl's deb packages

1. **Create the docker file**

```
FROM gaow/base-notebook:latest

MAINTAINER Diana Cornejo <dmc2245@cumc.columbia.edu>

USER root

#Install dependency tools and install data-set

RUN echo "deb [trusted=yes] http://statgen.us/deb ./" | tee -a /etc/apt/sources.list.d/statgen.list && \
apt-get update && \
    apt-get install -y regression-tutorial && \
    apt-get clean && mv /home/shared/* /home/jovyan && chown jovyan.users -R /home/jovyan/*

#Download notebook script and clean out output

USER jovyan
ARG DUMMY=unknown
RUN DUMMY=${DUMMY} curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/regression.docx -o regression.docx
RUN curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/regression.pdf -o regression.pdf

```

2. **Build the docker image**

`docker build -t statisticalgenetics/regression -f ./regression.dockerfile .`

3. **Push the image into dockerhub**

`docker push statisticalgenetics/regression`

`statgen-setup login --tutorial regression --my-name diana`

4. **Test the command line**

Based on the exercise test that it runs

### Create a docker container for the population drift and selection exercises

This is based on Carl's deb packages

1. **Create de dockerfile**

```
ROM gaow/base-notebook:latest
  
MAINTAINER Diana Cornejo <dmc2245@cumc.columbia.edu>

USER root

#Install dependency tools and install data-set using Carl's deb packages

RUN echo "deb [trusted=yes] http://statgen.us/deb ./" | tee -a /etc/apt/sources.list.d/statgen.list && \
apt-get update && \
    apt-get install -y popgen-tutorial && \
    apt-get clean && mv /home/shared/* /home/jovyan && chown jovyan.users -R /home/jovyan/*

#Download scripts and tutorial files

USER jovyan
ARG DUMMY=unknown
RUN DUMMY=${DUMMY} curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/PopGen.docx -o PopGen.docx
RUN DUMMY=${DUMMY} curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/PopGen.pp.docx -o PopGen.pp.docx
RUN curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/PopGen.pp.pdf -o PopGen.pp.pdf
RUN curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/PopGen.sim.docx -o PopGen.sim.docx
RUN curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/PopGen.sim.noscript.docx -o PopGen.sim.noscript.docx
RUN curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/PopGen.sim.noscript.pdf -o PopGen.sim.noscript.pdf
RUN curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/handout/PopGen.sim.pdf -o PopGen.sim.pdf
RUN curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/code/popgen_drif.R -o popgen_drift.R
RUN curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/code/popgen_selection.R -o popgen_selection.R
```

2. **Build the docker image**

`docker build -t statisticalgenetics/popgen -f ./popgen.dockerfile .`

3. **Push the image into dockerhub**

`docker push statisticalgenetics/popgen`

`statgen-setup login --tutorial popgen --my-name diana`

4. **Test the command line**

Based on the exercise test that it runs






