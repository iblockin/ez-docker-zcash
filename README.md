# docker-zcash

Docker configuration for building & running Zcash node & client in Docker. By default does not expose RPC interface outside of the container.

Requires multi-stage support from Docker, so version >= 17.05 is required.

# Building your own image

Clone this repository:

```
$ git clone https://github.com/vtorhonen/docker-zcash.git
$ cd docker-zcash
$ sudo docker build -t docker-zcash .
```
It takes quite a while to build the app. Grab a beer or two while waiting.

# Using pre-built images

If you trust me (note: you shouldn't) you can use pre-built images from my Dockerhub.

```
$ sudo docker pull vtorhonen/docker-zcash:1.0.10-1
```

Compressed image size is about about 990 MB.

# Running a Zcash node

This container uses `/zcash/data` by default as the data directory for Zcash. Needless to say, you should use some sort of persistent storage to store the blockchain and your wallet.

Container supports the following environment variables for configuration:

| Environment variable | Default value | Description |
-----------------------|---------------|--------------
`ZCASH_RPCUSER`     | 32 char long random string | Username for RPC access. Randomized on startup, if not set.
`ZCASH_RPCPASSWORD` | 32 chra long random string | Password for RPC access. Randomized on startup, if not set.
`ZCASH_ADDNODE`     | `mainnet.z.cash` | Target network. Uses zcash main network by default.
`ZCASH_DATADIR`     | `/zcash/data`| Data directory for storing blockchain and wallet.

Run the following command to run a Zcash node by using local volume mounts in interactive mode:

```
$ mkdir data
$ sudo docker run \
--name zcash-node -dit \
-v $(pwd)/data:/zcash/data \
vtorhonen/docker-zcash
```
If you want to attach to the process to see where blockchaing sync is at you can run:

```
$ sudo docker attach zcash-node
```

Use `^C-p-^C-q` to detach. Do not use `^C-c` or you will exit the shell and kill your node.


# Running Zcash CLI

You can interact with your wallet by using `zcash-cli` as follows:

```
$ docker exec -ti zcash-node zcash-cli getbalance
0.00000000
```

# Backing up your wallet

Your data directory is super important. Take backups by following [instructions from Zcash documentation](https://github.com/zcash/zcash/blob/master/doc/wallet-backup.md).