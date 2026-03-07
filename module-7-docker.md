# DevOpsBootCamp_TWN_2026
Preparation for DIgital Badge Certificate


# Use Docker for local development

* Starting code - to run js-app without mongodb configuration: 
https://gitlab.com/twn-devops-bootcamp/latest/07-docker/js-app/-/tree/feature/starting-code
* Final code - to run js-app with mongodb configuration: 
https://gitlab.com/twn-devops-bootcamp/latest/07-docker/js-app


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
--name mongo \
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
# Docker-copose up
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
[+] Building 19.6s (9/9) FINISHED                                                                                           docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                        0.4s
 => => transferring dockerfile: 193B                                                                                                        0.1s
 => [internal] load metadata for docker.io/library/node:20-alpine                                                                           3.1s
 => [auth] library/node:pull token for registry-1.docker.io                                                                                 0.0s
 => [internal] load .dockerignore                                                                                                           0.1s
 => => transferring context: 2B                                                                                                             0.0s 
 => [1/3] FROM docker.io/library/node:20-alpine@sha256:b88333c42c23fbd91596ebd7fd10de239cedab9617de04142dde7315e3bc0afa                    11.9s
 => => resolve docker.io/library/node:20-alpine@sha256:b88333c42c23fbd91596ebd7fd10de239cedab9617de04142dde7315e3bc0afa                     0.1s 
 => => sha256:7ad115895a6aadccf7f98a05c33873c04df2933ba557c5bcdbedd3de612803da 1.26MB / 1.26MB                                              0.7s 
 => => sha256:9d10d4687fae6e13c052431ea3221ccda835c3262df3564310345ffe173146d9 445B / 445B                                                  0.6s 
 => => sha256:ed2fdcee5269a06b0b8b77c69867176443364a72a9626e9aa2e66ab1ebda35a7 43.22MB / 43.22MB                                            8.0s
 => => extracting sha256:ed2fdcee5269a06b0b8b77c69867176443364a72a9626e9aa2e66ab1ebda35a7                                                   3.3s
 => => extracting sha256:7ad115895a6aadccf7f98a05c33873c04df2933ba557c5bcdbedd3de612803da                                                   0.1s 
 => => extracting sha256:9d10d4687fae6e13c052431ea3221ccda835c3262df3564310345ffe173146d9                                                   0.0s
 => [internal] load build context                                                                                                           0.1s
 => => transferring context: 225B                                                                                                           0.0s
 => [2/3] RUN mkdir -p /home/app                                                                                                            2.2s 
 => [3/3] COPY . /home/app                                                                                                                  0.1s
 => exporting to image                                                                                                                      1.0s
 => => exporting layers                                                                                                                     0.3s 
 => => exporting manifest sha256:255644274c742c9ecb777adcaabd78077b44d7ee90b71006657d3ff206d9fad3                                           0.1s
 => => exporting config sha256:18a8a67e67a2a5d60f28205a558e47ff24a5813b6cfe846fe4cb412482be2427                                             0.0s 
 => => exporting attestation manifest sha256:802f796c008a80d9b4dda9c67af3ceae22a83e33a5579effe54353a9a45a9c11                               0.1s 
 => => exporting manifest list sha256:163bbfff13703486aa10dd69a4040410d393f1e3acd5d749d4ba05c91ebaecbf                                      0.1s
 => => naming to docker.io/library/my-app:1.0                                                                                               0.0s 
 => => unpacking to docker.io/library/my-app:1.0                                                                                            0.2s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/wpbc6qcoe8px6r97tv92h0ljt
```






