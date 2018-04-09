# Getting Started with Docker

`docker version` - Shows client & server information
`docker info` - Shows in depth information such as `Containers`, `Images`, `Server Version` etc. This is a good command to see how things are on your `Docker` host.

## Run your first container

`docker run hello-world`

```
Unable to find image 'hello-world:latest' locally
```
The daemon will try and find a copy of `hello-world` on your local machine. But since it's the first time, it won't find it as above.

```
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete 
Digest: sha256:97ce6fa4b6cdc0790cda65fe7290b74cfebd9fa0c9b8c38e979330d547d22ce1
Status: Downloaded newer image for hello-world:latest
```
The docker daemon will pull the container image from a `Docker` repository that is remote and store it locally.

```
Hello from Docker!
```

After download, Docker will run the image and in our case, our image will print out the text on the screen and exit.

`docker ps` - Shows the current `running` images.

And in our case it will not show anything as there are no images currently running.

`docker ps -a` - If we want to see images that were running but `EXITED`/stopped.
`docker images` - Shows you a list of all images on your local machine.

## Containers and Images

`Images` - Stopped containers.
`Containers` - Running images.

`docker pull alpine` - This will pull the latest `Alpine` image from `DockerHub` without the need to execute the `docker run` command.

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              3fd9065eaf02        2 months ago        4.15MB
hello-world         latest              f2a91732366c        4 months ago        1.85kB
```

`docker pull alpine:3.6` - This will pull the `Alpine` image version 3.6 from `DockerHub`.

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              3fd9065eaf02        2 months ago        4.15MB
alpine              3.6                 77144d8c6bdc        2 months ago        3.97MB
hello-world         latest              f2a91732366c        4 months ago        1.85kB
```

Suppose you wanted to remove the image `Alpine` version `3.6`.

`docker rmi alpine:3.6` - Removes an image by the image ID. But can also be used to remove an image by the image name & tag.

```bash
MacBook-Pro-26:docker amuzanenhamo$ docker rmi alpine:3.6
Untagged: alpine:3.6
Untagged: alpine@sha256:3d44fa76c2c83ed9296e4508b436ff583397cac0f4bad85c2b4ecc193ddb5106
Deleted: sha256:77144d8c6bdce9b97b6d5a900f1ab85da325fe8a0d1b0ba0bbff2609befa2dda
Deleted: sha256:9dfa40a0da3b1a8a7c34abc596d81ede2dba4ecd5c0a7211086d6685da1ce6ef
```

## Container Lifecycle

`docker start <container>` -> `docker stop <container>` -> `docker rm <container>`

`Ctrl P+Q` - gets you out of a container without stopping the container.

`docker rmi $(docker images -q)` - `-q` means quietly and returns only `IDs`. This will delete all the images stored locally given the `IDs` returned.

`docker rm $(docker ps -aq)` - Removes all the images and containers.