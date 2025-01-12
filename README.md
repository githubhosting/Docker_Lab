# Containerizarion Lab - Docker

1. Write Dockerfile and run docker image to execute java program
a. In interactive console mode
b. In Deamon mode

2. Write and run docker file to run java program and the java program should write logs to the host system which is outside the container using mount  (Ex: write logs to host system with path c://container/logs)

3. Log into docker container to see logs for debug purpose and locate jars path, locate logs path

4. Write and run docker file to run java program with environment variables like Port number (8081), or any logs location (c://container/logs) etc

5. Set up a MySQL database in a Docker container and connect it to a web application (Any database is fine)

6. Create docker compose file to run multiple containers like database and to run the Java program using the same docker compose file

---

To write Dockerfile and run docker image to execute java program

```Dockerfile
FROM openjdk:17-jdk-alpine3.13
WORKDIR /app
COPY demo-1.0.0-SNAPSHOT.jar /app/demo-1.0.0-SNAPSHOT.jar
ENTRYPOINT ["java", "-jar", "demo-1.0.0-SNAPSHOT.jar"]
```

First copy the jar and docker files to a folder and navigate to it

```bash
docker build -t workshop .
```

`-t` is used to tag the image

```bash
docker build -t <docker image name> <docker folder path>
```

For Interactive mode

```bash
docker run -p 8081:8081 workshop
```

`workshop` is the image name

`-p` is used to map the port from the host to the container

`8081:8081` is the port mapping from the host to the container

For Daemon mode

```bash
docker run -d -p 8081:8081 workshop
```

`-d` is used to run the container in the background

---

To see log and jar path

```bash
docker ps
```

`ps` stands for process status

```bash
docker logs -f <container id or name>
```

`-f` is used to follow the logs

```bash
docker exec -it <container id or name> /bin/sh
ls
```

`it` is used to run the container in interactive mode

`/bin/sh` is the shell

---

To set environment variables and upload the file to a container

```bash
docker run -d -p 8081:8081 -env SERVER_PORT=8081 --env UPLOAD_DIR=/tmp/fileupload/ -v /home/fileupload:/tmp/logs <docker image name>
```

`-e` is used to set the environment variable

`-v` is used to mount the volume

`/home/fileupload` is the host path

`/tmp/logs` is the container path

Create a file by `vim some_file.txt` and write something in it

To Upload the file to the container

```bash
curl --location 'http://localhost:8081/upload' --form 'file=@some_file.txt'
```

To check the file in the container

```bash
docker exec -it <container id or name> /bin/sh
ls /tmp/fileupload
```
