Let us look at how to create a Dockerfile which set up a simple webpage for us.

First, we will build a Docker Image shown below:
```bash
$ cd nginx-demo
$ docker build -t nginx-demo .

Sending build context to Docker daemon 30.21 kB
Step 1/4 : FROM nginx
latest: Pulling from library/nginx
693502eb7dfb: Already exists
6decb850d2bc: Pull complete
c3e19f087ed6: Pull complete
Digest: sha256:52a189e49c0c797cfc5cbfe578c68c225d160fb13a42954144b29af3fe4fe335
Status: Downloaded newer image for nginx:latest
 ---> 6b914bbcb89e
Step 2/4 : COPY wrapper.sh /
 ---> 70297381661d
Removing intermediate container 4e5d9e435617
Step 3/4 : COPY html /usr/share/nginx/html
 ---> 25adcbc67425
Removing intermediate container 8eb8ee64131f
Step 4/4 : CMD ./wrapper.sh
Step 4/4 : CMD ./wrapper.sh
 ---> Running in f4721b3f6421
 ---> c6c187264fad
Removing intermediate container f4721b3f6421
Successfully built c6c187264fad
```

Let us verify if Docker Image is built or NOT:
```bash
$ docker images
```
```
REPOSITORY         TAG                 IMAGE ID            CREATED             SIZE
nginx-demo         latest              c6c187264fad        7 seconds ago       182 MB
```

Running the Container:
```bash
$ docker run -d -P --name myweb nginx-demo
d30904aa94a4615045a2962c3fd15f02bcd82c1b7371f927f77923d60f014645
```

Verifying if Container is running or not:
```bash
$ docker ps
CONTAINER ID        IMAGE        COMMAND             CREATED             STATUS              PORTS                           
                NAMES
d30904aa94a4        nginx-demo   "./wrapper.sh"      3 seconds ago       Up 2 seconds        0.0.0.0:32782->80/tcp, 0.0.0.0:3
2781->443/tcp   myweb
```

**Note:**

Did you encounter this error while cloning and building the Docker Image:

```
docker: Error response from daemon: oci runtime error: container_linux.go:247: starting container process caused "exec: \"./wrapper.sh\": pe
rmission denied".
```

This is due to permission issue. The wrapper.sh script possibly doesn't have executable permission. Run chmod +x wrapper.sh and re-build the Docker Image.

Displaying Container Ports:\
```bash
$ docker port < container id >
  80/tcp -> 0.0.0.0:32782
  443/tcp -> 0.0.0.0:32781
```

You can open http://localhost:32769 in your browser.

If you want to run Nginx in your desired port, here is the way:

```bash
$ docker run -d -p   8888:80 ajeetraina/nginx-demo-105
beb4fa77b033fb46e9f772110cfcd65e9656af0212931e674a1f2bd34422d478

$ docker ps
CONTAINER ID        IMAGE        COMMAND             CREATED             STATUS              PORTS                           
NAMES
beb4fa77b033        nginx-demo   "./wrapper.sh"      7 seconds ago       Up 5 seconds        443/tcp, 0.0.0.0:8888->80/tcp   
gifted_almeida
```
