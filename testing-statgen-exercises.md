# This is a guide to keep on track how did I checked the statgen exercises

First login as root in the virtual machine

ssh root@104.156.251.54
password: diana_test

`curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/src/vm-setup.sh -o /tmp/vm-setup.sh
bash /tmp/vm-setup.sh`

To start tutorial on server

`statgen-setup launch --tutorial vat pseq`


## Testing IGV 

First run this commands and follow Gao's Tutorial

### Login to dockerhub

`docker log in`

> User: statisticalgenetics

> Password: ##Wombat19##

```
statgen-setup -h
docker build --build-arg DUMMY=`date +%s` -t statisticalgenetics/igv -f docker/igv.dockerfile docker 
docker push statisticalgenetics/igv
statgen-setup login --tutorial igv --my-name statisticalgenetics

```
Hint: use the email address obtained after running the command line and add the ip of your computer.
For MacOS use ip=0.0.0.0
Question: How can I add the files to a local directory so that can people can upload them to the online IGV program?

## Testing VAT

```
statgen-setup -h
docker build --build-arg DUMMY=`date +%s` -t statisticalgenetics/vat -f docker/vat.dockerfile docker 
docker push statisticalgenetics/vat
statgen-setup login --tutorial vat --my-name statisticalgenetics

```

Now start running the exercise by reading the tutorial and adding the commands one by one in the terminal


## Testing PSEQ

```
statgen-setup -h
docker build --build-arg DUMMY=`date +%s` -t statisticalgenetics/pseq -f docker/pseq.dockerfile docker 
docker push statisticalgenetics/pseq
statgen-setup login --tutorial pseq --my-name statisticalgenetics

```

## Testing Popgen

```
statgen-setup -h
docker build --build-arg DUMMY=`date +%s` -t statisticalgenetics/popgen -f docker/popgen.dockerfile docker 
docker push statisticalgenetics/popgen
statgen-setup login --tutorial popgen --my-name statisticalgenetics

```

## Testing Regression

## Useful commands in docker

```statgen-setup clean
   docker pull <image>
   docker images
   docker stop <container id>


