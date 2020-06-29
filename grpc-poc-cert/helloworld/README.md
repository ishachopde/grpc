this is a grpc-go poc helloworld application.

1. It contains client and server scripts
2. docker file to create container for trsting
3. Mapping grpc Services and deployment

Blockers: Ambassador test fails when hostname(ambassador) of the running service is added.


Below are the basic instructions for running this app:

1. create the container using docker file and test it:
 Dockerfile is present inside example/grpc-poc-cert/helloworld/greeter_server/Dockerfile
 
 $ docker build -t grpc_demo .

  e.g docker build -t grpc-demo/server (i used this since my yaml file already has the imag etagged in it. You can change it if you would like)

$ docker run --net=host -it grpc_demo   ||  docker run -p 50054:50054 grpc_demo
$ docker push grpc_demo

2. Switch to another terminal and go to greeter_client directory
 $ go run main.go  

On the server terminal run:
$ kubectl apply -f grpc-poc-cert.yaml
$ kubectl get service ambassador -o wide || kubectl get service ambassador -n ambassador (to get the external-ip and port)

3. Go to server and client scripts and make the changes written below:

 O/p: Ideally it displays Hello World, but after making ambassador it doesn't work

 **Note:
 Client side changes for ambassador are: 

 const (
	address     = "$AMBASSADOR:$PORT"
	defaultName = "world" 
)

server side changes :

const (
	port = "localhost:$PORT"
)


