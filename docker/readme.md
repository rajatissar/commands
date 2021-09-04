# Docker

## Commands

```bash
$ docker run hello-world
run docker repo
```

```bash
$ docker ps
list of process running
```

```bash
$ docker ps -a
list of images running
```

```bash
$ docker start name_of_image
start docker image
```

```bash
$ docker stop name_of_image
stop docker image
```

```bash
$ docker rm name_of_image
remove docker image
```

```bash
$ docker images
list of Docker images
```

```bash
$ docker run -p 8080:80 --name hello -d hello-world
TODO
```

```bash
$ docker logs name_of_image
see docker logs
```

```bash
$ docker logs -f hello
see realtime logs
```

```bash
$ docker tag hello-world pmcckee/hello-world
TODO
```

```bash
$ docker push pmckee/hello-world
push docker image
```

```bash
$ docker-compose up -d
TODO
```

```bash
$ docker-compose down
TODO
```

```bash
$ docker volume create todo-db
TODO Create a volume
```

```bash
$ docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
TODO volume mount
```

```bash
$ docker volume inspect
where docker is storing data
```

```bash
$ docker run -dp 3000:3000 \
     -w /app -v "$(pwd):/app" \
     node:12-alpine \
     sh -c "yarn install && yarn run dev"
TODO
```

```bash
$ docker network create todo-app
create network
```

```bash
$ docker run -d \
     --network todo-app --network-alias mysql \
     -v todo-mysql-data:/var/lib/mysql \
     -e MYSQL_ROOT_PASSWORD=secret \
     -e MYSQL_DATABASE=todos \
     mysql:5.7
TODO
```

```bash
$ docker exec -it <mysql-container-id> mysql -u root -p
to confirm datadase up and running
```

```bash
$ docker run -it --network todo-app nicolaka/netshoot
TODO
```

```bash
$ dig mysql
TODO
```

```bash
docker run -dp 3000:3000 \
   -w /app -v "$(pwd):/app" \
   --network todo-app \
   -e MYSQL_HOST=mysql \
   -e MYSQL_USER=root \
   -e MYSQL_PASSWORD=secret \
   -e MYSQL_DB=todos \
   node:12-alpine \
   sh -c "yarn install && yarn run dev"
```

```bash
docker-compose version
```
