# Node Operator

Welcome to the Swan Mainnet Node Operator Guide! This page provides a Docker image for running a Swan Mainnet node. The image is built on the official `nebulablock/swan-node-mainnet` image and is designed to be easy to use and highly configurable.

### Getting Started

To start a new container, use the following command:

```sh
docker run -d \
  -v /opt/swan:/opt/swan \
  -p 8545:8545 \
  -e L1RPC=$L1_RPC \
  -e L1BEACON=$L1_Beacon_RPC \
  --name swan-node \
  nebulablock/swan-node-mainnet
```

**Tip**: Looking to speed up your node initialization? Check out our[ Snapshots guide](swan-node-snapshots.md) for a faster synchronization method that can significantly reduce your initial setup time.

This command will start a new container in detached mode, mapping the container's port 8545 to the host's port 8545 and persisting data in the `/opt/swan` directory.

### Environment Variables

The image supports the following environment variables:

* `L1RPC`: The URL of the L1 RPC provider.
* `L1BEACON`: The URL of the L1 Beacon RPC provider.

These variables can be set using the `-e` flag when running the container, as shown in the example command above.

### Volumes

The image uses a volume at `/opt/swan` to persist data between container restarts. This volume can be mounted using the `-v` flag when running the container, as shown in the example command above.

### Ports

The image exposes port 8545 for RPC connections. This port can be mapped to a host port using the `-p` flag when running the container, as shown in the example command above.

