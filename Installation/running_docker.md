_Last updated: January 2026_

This guide explains how to run an **Abey (`gabey`) node** using Docker.

Abey provides an official Docker image containing recent snapshot builds from the `develop` branch, available on Docker Hub.

- Docker image: https://hub.docker.com/r/abeyfoundation/gabey
- Base image: Ubuntu (~158 MB)

---

## Prerequisites

Before you begin, ensure that Docker is installed and running on your system.

- Docker installation guide: https://docs.docker.com/get-docker

---

## Pull the Docker Image

Download the latest Abey Docker image:

```bash
docker pull abeyfoundation/gabey
```

---

## Run a Basic Node

Start an Abey node with default settings:

```bash
docker run -it -p 30313:30313 abeyfoundation/gabey
```

This command exposes the P2P port (`30313`) to the host.

---

## Run a Node with JSON-RPC Enabled

To enable the JSON-RPC interface on port **8545**, run:

```bash
docker run -it \
  -p 8545:8545 \
  -p 30313:30313 \
  abeyfoundation/gabey --rpc --rpcaddr "0.0.0.0"
```

⚠️ **Security Warning**

Binding RPC to `0.0.0.0` exposes the JSON-RPC interface to external connections.  
**Do not use this setting on public or untrusted networks.**

For improved security, restrict access using firewall rules or bind RPC to `127.0.0.1`.

---

## Run with Interactive Console

To start the node with the interactive JavaScript console:

```bash
docker run -it -p 30313:30313 abeyfoundation/gabey console
```

---

## Persist Blockchain Data with Volumes

By default, blockchain data is stored inside the container and will be lost when the container is removed.

To persist data between container restarts, use a Docker volume. Replace `/path/on/host` with a directory on your host system:

```bash
docker run -it \
  -p 30313:30313 \
  -v /path/on/host:/root/.abeyfoundation \
  abeyfoundation/gabey
```

This ensures that blockchain data and configuration files are preserved across container runs.

---

## Notes

- Ensure required ports are open on your firewall if running a public node
- Use Docker volumes for long-running or production nodes
- Avoid exposing RPC endpoints publicly without authentication
