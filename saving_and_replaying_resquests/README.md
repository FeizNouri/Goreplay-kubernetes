# Goreplay-kubernetes

## Saving requests to file and replaying them later

in this demo we will use persistent volume.

first create the persistent volume :

```kubectl create -f persistentvolume.yaml```

then the persistent volume claim :

```kubectl create -f persistentvolumeclaim.yaml```

to save requests to file, create the pod with the python server that we want to capture it's traffic, this pod also contain the goeplay app :

```kubectl create -f deployment-goreplay.yaml```

then create the service for this pod :

```kubectl create -f service-goreplay.yaml```

make a requests to the first server, then check the file by executing : 

``` kubectl exec -it <name_of_the_pod> -c goreplay -- /bin/sh```

you will find the file in the dir files.

stop the pod, and recreate it but this time change the goreplay arguments in the deployment file to read the requests from the existing file and replay them to the server.

replace the existing arguments with : 

```
        - "--input-file"
        - "files/requests_docker_0.gor"
        - "--output-http=http://localhost:3000"
```

you can check the goreplay logs indicating the reading of the file 

```[PPID 0 and PID 1] Version:1.2.0  FileInput: end of file 'files/requests_docker_0.gor' ```

by executing : 

```kubectl logs -f <name_of_the_pod> -c goreplay```

and check the requests coming to the server : 

```kubectl logs -f <name_of_the_pod> -c myserver```






