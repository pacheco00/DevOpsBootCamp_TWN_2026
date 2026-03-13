# DevOpsBootCamp_TWN_2026
Preparation for DIgital Badge Certificate


# Use Docker for local development

* Starting code - to run js-app without mongodb configuration: 
https://gitlab.com/twn-devops-bootcamp/latest/07-docker/js-app/-/tree/feature/starting-code
* Final code - to run js-app with mongodb configuration: 
https://gitlab.com/twn-devops-bootcamp/latest/07-docker/js-app


# Create Dockerfile for Nodejs application and build Docker image

## Download Images
```
# docker pull mongo
# docker pull mongo-express
$ docker images
IMAGE                                                                                   ID             DISK USAGE   CONTENT SIZE   EXTRA                               
mongo-express:latest                                                                    1b23d7976f02        286MB         58.9MB    U 
mongo:latest                                                                            9e52bf8768ae       1.27GB          329MB    U 
```

## Create network
```
# docker create network mongo-network
$ docker network ls
NETWORK ID     NAME            DRIVER    SCOPE
989c58857c5a   bridge          bridge    local
3f5f5bfa9223   host            host      local
1478d341d8b7   mongo-network   bridge    local
151cc9544c97   none            null      local
```

## Run containers
```
# Run docker mongo
docker run -d \
-p 27017:27017 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--name mongodb \
--net mongo-network \
mongo
```

```
# Run docker mongo-express
docker run -d \
-p 8081:8081 \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
-e ME_CONFIG_BASICAUTH_USERNAME=user \
-e ME_CONFIG_BASICAUTH_PASSWORD=pass \
--name mongo-express \
--net mongo-network \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
-e ME_CONFIG_MONGODB_URL=mongodb://mongodb:27017 \
mongo-express
```

```
$ docker ps
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                                             NAMES
914b6fa74e59   mongo-express   "/sbin/tini -- /dock…"   2 minutes ago   Up 2 minutes   0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp       mongo-express
45457b345040   mongo           "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   0.0.0.0:27017->27017/tcp, [::]:27017->27017/tcp   mongodb
```

## Docker logs
```
{"t":{"$date":"2026-03-03T02:11:58.053+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"172.19.0.1:34682","isLoadBalanced":false,"uuid":{"uuid":{"$uuid":"f819cb7a-6d46-4416-b78e-9caf6e78af0b"}},"connectionId":7,"connectionCount":5}}
{"t":{"$date":"2026-03-03T02:11:58.055+00:00"},"s":"I",  "c":"NETWORK",  "id":51800,   "ctx":"conn7","msg":"client metadata","attr":{"remote":"172.19.0.1:34682","client":"conn7","negotiatedCompressors":[],"doc":{"driver":{"name":"nodejs","version":"4.16.0"},"platform":"Node.js v24.13.1, LE","os":{"name":"win32","architecture":"x64","version":"10.0.26200","type":"Windows_NT"}}}}
```

# Docker Compose - Run multiple Docker containers

# Docker compose file
```
version '3'
services:
  mongodb:
    image: mongo
    ports:
    - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
  mongo-express:
    image: mongo-express
    ports:
    - 8081:8081
    restart: always
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_BASICAUTH_USERNAME=user
      - ME_CONFIG_BASICAUTH_PASSWORD=pass
      - ME_CONFIG_MONGODB_SERVER=mongodb
      - ME_CONFIG_MONGODB_URL=mongodb://mongodb:27017
```

```
# Docker-compose up
$ docker ps
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                                             NAMES
88e3601bbe83   mongo           "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:27017->27017/tcp, [::]:27017->27017/tcp   m7-mongodb-1
5f105e8569aa   mongo-express   "/sbin/tini -- /dock…"   2 minutes ago   Up 2 minutes   0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp       m7-mongo-express-1

# Check network (m7_default)
Oscar@Lenovo-7i MINGW64 ~/Downloads/nana
$ docker network ls
NETWORK ID     NAME            DRIVER    SCOPE
4e40afa63916   bridge          bridge    local
fb5ad9bb43a6   diveinto.io     bridge    local
70dd87897056   host            host      local
06834bc29d19   m7_default      bridge    local
4562e8caa1cd   minikube        bridge    local
2d714e6481f4   mongo-network   bridge    local
10cf9dfab457   none            null      local
```

```
# Docker compose down
$ docker-compose -f mongo.yaml down
time="2026-03-04T21:09:46-05:00" level=warning msg="C:\\Users\\Oscar\\Downloads\\nana\\m7\\mongo.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
[+] down 3/3
 ✔ Container m7-mongo-express-1 Removed                                                                                           0.8s
 ✔ Container m7-mongodb-1       Removed                                                                                           1.0s
 ✔ Network m7_default           Removed    

$ docker network ls
NETWORK ID     NAME            DRIVER    SCOPE
4e40afa63916   bridge          bridge    local
fb5ad9bb43a6   diveinto.io     bridge    local
70dd87897056   host            host      local
4562e8caa1cd   minikube        bridge    local
2d714e6481f4   mongo-network   bridge    local
10cf9dfab457   none            null      local
```

```
# Docker compose up
$ docker ps 
CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS          PORTS                                        
     NAMES
dd01d784aa76   mongo           "docker-entrypoint.s…"   45 seconds ago   Up 43 seconds   0.0.0.0:27017->27017/tcp, [::]:27017->27017/tcp   m7-mongodb-1
e958833d8136   mongo-express   "/sbin/tini -- /dock…"   45 seconds ago   Up 43 seconds   0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp       m7-mongo-express-1
```

# Dockerize Nodejs application

## Docker file
```
FROM node:20-alpine
ENV MONGO_DB_USERNAME=ADMIN \
    MONGO_DB_PWD=password
RUN mkdir -p /home/app
COPY . /home/app
CMD ["node", "server.js"]
```

```
# Docker build
$ docker build -t my-app:1.0 .
$ docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED         STATUS         PORTS     NAMES
14f360fa2eaa   my-app:1.0   "docker-entrypoint.s…"   7 minutes ago   Up 7 minutes             confident_allen
```
```
$ docker exec -it 14f360fa2eaa sh
/home/app # ls
images             index.html         node_modules       package-lock.json  package.json       server.js
/home/app # ls /
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
```

```
/home/app # env
NODE_VERSION=20.20.1
HOSTNAME=14f360fa2eaa
YARN_VERSION=1.22.22
SHLVL=1
HOME=/root
MONGO_DB_USERNAME=admin
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MONGO_DB_PWD=password
PWD=/home/app
/home/app #
```

# Create Docker repository on Nexus and push to it

## Docker file

