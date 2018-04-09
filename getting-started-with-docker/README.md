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