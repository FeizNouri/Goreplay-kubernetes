# Goreplay-kubernetes

## Mirroring

to forward web traffic and to another server in another pod, create the pod with the python server that we want to capture it's traffic, this pod also contain the goreplay app :

```kubectl create -f deployment-goreplay.yaml```

then create the service for this pod :

```kubectl create -f service-goreplay.yaml```

now create the pod with the second server that we will forward the traffic to :

```kubectl create -f deployment-server.yaml```

and create it's service :

```kubectl create -f service-server.yaml```

make a requests to the first server and watch them mirroring in the second server by executing : 
``` kubectl logs -f <name_of_the_second_pod>```