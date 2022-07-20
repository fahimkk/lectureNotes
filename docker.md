# Docker

## Basic commands

To list all the running containers
`$ docker ps`

To list all the containers that we have ever created
`$ docker ps --all`

`$ docker images`

To list running containers
`$ docker container ls`

Eg to use a redis docker without installing redis
`$ docker run -it redis`

Docker run command first check for the image locally, if not it takes the
image form docker hub and create a container with that image.
`$ docker run image_name`
this will execute the default command inside the image_name

To override the default commant, the overriding command should exists inside the file system.
`$ docker run image_name command`

To create a container
`$ docker create image_name`

If we want to override default command, do that when creating container, we can not override when starting or restarting.
overriding command only possible if the file accept it.
`$ docker create image_name command`

To start a container
`$ docker start -a container_id`

`-a` command used when starting a container to print the output of the docker contianer to the terminal,

When forgot `-a` command when starting the container, and need to see the logs we can use the `logs` command
`$ docker logs container_id`

To restart a stoped container we can use `start` command, but when restarting we can not override the default command

To remove all the containers
`$ docker system prune`

To remove a single container
`$ docker rm container_id`

To stop a container
`$ docker stop container_id`
`$ docker kill container_id`
`stop` will `kill` if it can'nt shut safely within 10sec

To Excecute commands in running container
`$ docker exec -it container_id command`
(run this in seperate terminal)

To get a linux command prompt in a container
`$ docker exec -it container_id sh`
This will provide a shell, here we can type any linux cammands like cd, ls etc
to exit form this shell `ctrl+d` or `exit`

we can also start a shell immedietly docker runs, in
`$ docker run -it image_name sh`

## Creating Docker Images

Creating a Dockerfile

1. Specify a base image
2. Run some commands to install additional programs
3. Specify a command to run on container startup

To build docker image, crete a Dockerfile and in terminal cd to the folder containing Dockerfile
`$ docker build .`
copy container_id from the result of the above command
`$ docker run container_id`

Instead of using container_id to run a docker, we can use a name. For that we have to make changes when docker build command
`your_docker_id/repo_or_project_name:version`
you will get docker_id when sign up in docker
example
`$ docker build -t stephengrider/redis:latest .` .
after `-t` we can also specify container name so that instead of id we can use this name for docker run

to user a dockerfile of another name use `-f` tag

## Without Dockerfile

When building a image, in each step it creates a temp contianer and from this temp container it creates an image(snapshot), this image is used by the next step.
So this means we can create an container from an image, and also we can create an image from a container.
start a conatainer of base_image with sh command
`$ docker run -it alpine sh`
This will open a shell inside the continer, in this shell we can install the dependencies that we mentioned in Dockerfile after RUN command manually
`apk add --update redis`
Then open another shell, and ps command will give the id of the running container
Below command will take a snapshot of the running container and assign the default command to it and generate an image of it.
`$ docker commit -c 'CMD ["redis-server"]' container_id`
This will return an id
`$ docker run above_id`

---

Copy fiels to docker container, add below line to Dockerfile before the section where we need to access the file

`COPY path_to_folder_to_copy_from_on_your_machine_realative_to_build_context place_to_copy_stuff_inside_the_container`

Docker run with port mapping
`$ docker run -p route_incoming_request_to_this_port_on_local_hot_to:this_post_inside_the_container image_id`

## Docker Componse

create a file docker-compose.yml
`$ docker-compose up` - to run a docker container
`$ docker-compose up --build` - to build and run a docker container
`$ docker-compose down` - to stop a docker container
`$ docker-compose ps` - to get status of running containers (run this command where docker-compose.yml file exists)

### Docker Volume

To reflect changes in files directly to the docker container without rebuild,
start the container with the following command
`$ docker run -p localPort:dockerPort -v /app/node_modules -v $(pwd):/app container_id`
here `/app` is the working directory in Dockerfile
`app/node_modules` is a placeholder that normally we dont have node modules locally, we create that when docker build. so we can not place
(this method is only working with react.)
