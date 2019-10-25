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
            
