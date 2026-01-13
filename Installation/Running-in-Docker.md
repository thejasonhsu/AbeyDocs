# Running in Docker

We keep a Docker image with recent snapshot builds from the `develop` branch [on DockerHub](https://hub.docker.com/r/abeyfoundation/gabey/). In addition to the container based on [Ubuntu](http://www.ubuntu.com) (158 MB).

To pull the image, run this command:

```shell
docker pull abeyfoundation/gabey
```

Start a node with:

```shell
docker run -it -p 30313:30313 abeyfoundation/gabey
```

To start a node that runs the JSON-RPC interface on port **8545**, run:

```shell
docker run -it -p 8545:8545 -p 30313:30313 abeyfoundation/gabey --rpc --rpcaddr "0.0.0.0"
```
**WARNING: This opens your container to external calls. "0.0.0.0" should _not_ be used when exposed to public networks**

To use the interactive JavaScript console, run:

```shell
docker run -it -p 30313:30313 abeyfoundation/gabey console
```

## Using Data Volumes

To persist downloaded blockchain data between container starts, use Docker [data volumes](https://docs.docker.com/engine/tutorials/dockervolumes/#/mount-a-host-directory-as-a-data-volume). Replace `/path/on/host` with the location you want to store the data in.

    docker run -it -p 30313:30313 -v /path/on/host:/root/.abeyfoundation abeyfoundation/gabey

