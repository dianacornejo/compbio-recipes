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
            
## Creating a container for annovar in statgen-courses

Being in the ~/statgen-course/docker folder run the following command:

1. Create the DockerFile as the example below

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

2. Build the docker image

`docker build -t statisticalgenetics/annovar -f ./annovar.dockerfile .`

3. Push it into dockerhub

`docker push statisticalgenetics/annovar`

`statgen-setup login --tutorial annovar --my-name diana`

4. Test the command line

Start the tutorial as is written in the exercise

## Creating a docker container for the Regression exercise. 

This is based on Carl's deb packages

1. Create the docker file

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

2. Build the docker image

`docker build -t statisticalgenetics/regression -f ./regression.dockerfile .`

3. Push the image into dockerhub

`docker push statisticalgenetics/regression`

`statgen-setup login --tutorial regression --my-name diana`

4. Test the command line

Based on the exercise test that it runs

### Create a docker container for the population drift and selection exercises

This is based on Carl's deb packages

1. Create de dockerfile

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

2. Build the docker image

`docker build -t statisticalgenetics/popgen -f ./popgen.dockerfile .`

3. Push the image into dockerhub

`docker push statisticalgenetics/popgen`

`statgen-setup login --tutorial popgen --my-name diana`

4. Test the command line

Based on the exercise test that it runs






