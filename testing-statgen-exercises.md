# This is a guide to keep on track how did I tested the statgen exercises
Specifically when the were ran in the cloud

First login as root in the virtual machine

```bash
ssh root@104.156.251.54
password:diana_test
curl -fsSL https://raw.githubusercontent.com/statgenetics/statgen-courses/master/src/vm-setup.sh -o /tmp/vm-setup.sh
bash /tmp/vm-setup.sh
```

To *start* tutorial on server

`statgen-setup login --tutorial "annovar vat pseq" --my-name <your-name>`

To *restart* a tutorial

`statgen-setup login --tutorial annovar --restart`

To build an image using statgen-setup

`statgen-setup build --tutorial <tutorial-name>`


## Testing IGV 

First of all to create docker images you have to login into dockerhub. To do so run the following commands and then follow Gao's Tutorial.

### Login to dockerhub

`docker log in`

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

## Testing Annovar

```
statgen-setup -h
docker build --build-arg DUMMY=`date +%s` -t statisticalgenetics/annovar -f docker/annovar.dockerfile docker 
docker push statisticalgenetics/annovar
```

The two commands above where replaced by `statgen-setup build --tutorial annovar`

Now login into the VM and run the exercise in the cloud

```
statgen-setup login --tutorial annovar --my-name statisticalgenetics

```

## Testing Regression

## Testing GEMINI

```
statgen-setup build --tutorial gemini
statgen-setup login --tutorial gemini --my-name statisticalgenetics

```
## Testing MLINK

```
statgen-setup build --tutorial mlink
statgen-setup login --tutorial mlink --my-name statisticalgenetics

```

