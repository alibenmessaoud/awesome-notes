# Most Docker daily commands for developers

### docker build

```bash
docker build --rm -t --no-cache docker-examples:latest .
```

`--rm` is used in order to remove an intermediate container after the successful build.

`-t` tag images you build, otherwise it becomes difficult to know at a glance what it contains.

`--no-cache ` force Docker to rebuild the image from scratch.

### docker run

```bash
docker run -d -p 4000:3000 docker-examples:latest
```

`-d` runs the container in the background and prints the container ID.
`-p` publishes the container port to the local machine port 
`host_port:docker_port` maps ports

```bash
docker run docker-examples:latest
```

Run a Docker image and see/control the launched process in your terminal. 
Press `Ctrl + C` to to exit your container as well.

```bash
docker run -t -i docker-examples:latest node
```

Helps to run a specific command/tool inside your container.

### docker stop

```bash
docker stop container_name
```

Stop an already launched container.

`docker ps -a` to get `container_name` 

```bash
docker stop $(docker ps -a -q)
```

Stop all containers

### docker ps

```bash
docker ps -a
```

list of running containers.

### docker logs

```bash
docker logs -f container_name
```

Watch for logs of the specific container; 
This works for running and stopped containers. 
`-f` turns on following for the log output.

### docker kill

```bash
docker kill $(docker ps -q)
```

Kills all running containers.

### docker rm

```bash
docker rm $(docker ps -a -q)
```

Deletes all stopped container

### docker system

```bash
docker system prune
```

Remove all unused containers, networks, images (both dangling and unreferenced), and optionally, volumes.

### docker rmi

```bash
docker rmi $(docker images -q)
```

Deletes all images

### docker exec

```bash
docker exec -it container_name /bin/sh
```

Runs a specific command/tool inside your container and allows you to use it inside a container.

### docker-compose up

```bash
docker-compose up
```

Build a docker-compose file with some services, and run them all and see their output.

```bash
docker-compose up -d
```

You want to build and run all services in the background and see their statuses later.

```bash
docker-compose up -d example-service-1
```

Run a specific service (name as in docker-compose file) with all declared ports and volumes.

### docker-compose down

```bash
docker-compose down
```

Stops & removes containers, networks, volumes, and images declared in a docker-compose file.

### docker-compose run

```bash
docker-compose run example_app rails db:migrate
```

Run a specific task that your service provides, such as migrations or tests. You can do this in a similar way as docker run, but you can run the task by service name from your docker-compose file. Keep in mind that when you use “run” ports, you declarations in docker-compose will not be published without using --service-ports.

### docker-compose exec

```bash
docker-compose exec example_app node
```

Access inside the launched container and execute something, similar with docker exec. The difference is that you could get access by the service name. Useful for the debug purpose, test network, check that all data exist inside the Docker container.

### docker-compose logs

```bash
docker-compose logs -f container_name
```

Watches for logs of the specific container, for both running and stopped containers.

`-f` turns on following for the log output.

### docker-compose stop

```bash
docker-compose stop example_app 
```

Stop your container by service name or container name.

### docker-compose restart

```bash
docker-compose restart example_app 
```

Restart a container with the service name or container name.