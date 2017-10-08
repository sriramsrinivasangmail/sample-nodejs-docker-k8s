

This is a simple example  nodejs project  that you can use to understand how to dockerize your node application as well as deploy into kubernetes (such as minikube)

**Eclipse source project**:

This folder container nodeclipse assets - so you should be able to import into your eclipse workspace directly & test out against port 3000. 

To run it with debug from the command line, using a unique port to avoid conflicts:

node --inspect=4000 --debug-brk app.js

 Dockerfile  describes how to build the docker image
k8s/  has the kubernetes artifacts

**Try out the Dockerized node app**:

1). To build the docker image

docker build -t nodejs-helloworld:v1 .

2). Start up the container,  *map* port 8000 on the host to port 3000 inside the container

docker run -p 8000:3000 -d nodejs-helloworld:v1

3).  Try  it out

http://localhost:8000/

**Testing with kubernetes/minikube**

**minikube** notes: 
if you are using minikube - its usually much more convenient to use the docker daemon/registry ***inside*** of minikube. 

`$ eval $(minikube docker-env)`

- would up your env variables to allow your host's docker client to talk to the docker daemon inside minikube

However you may see a version mismatch issue such as: 

    $ docker ps
    Error response from daemon: client is newer than server (client API version: 1.24, server API version: 1.23)

If thats the case, then set the API version env variable appropriately. For example.
 

    export DOCKER_API_VERSION=1.23

Then you can do a docker **build** etc. against the docker daemon *inside* minikube.

**Important note**  Just make sure you tag your Docker image with something *other* than ‘latest’ and use that tag while you pull the image. 
See: http://kubernetes.io/docs/getting-started-guides/minikube/#reusing-the-docker-daemon


**Deploy** in minikube:

	look at k8s/README.md about changing the ymls and scripts on the location of the docker image to an external registry if you so decide to.
    kubectl create -f k8s/deployment.yml

**Expose**:  (using NodePort only for minikube)

    kubectl expose deployment  nodejs-helloworl-deployment --type=NodePort

**Access**:

    minikube service nodejs-helloworld-deployment

