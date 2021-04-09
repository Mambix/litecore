# Supported tags and respective `Dockerfile` links
* `latest` ([Dockerfile])

![Litecore v4.1.0](https://img.shields.io/badge/litecore-v4.1.10-green.svg)
[![](https://images.microbadger.com/badges/image/mambix/litecore.svg)](https://microbadger.com/images/mambix/litecore "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/mambix/litecore.svg)](https://microbadger.com/images/mambix/litecore "Get your own version badge on microbadger.com")

## Litecore
Infrastructure to build Litecoin and blockchain-based applications for the next generation of financial technology.

Links:
* [Litecoin site];
* [GitHub repo].

## Running the container

Create a volume where all data and blockchain data will be stored:

```
docker volume create --name litecore_data
```

Now run the container, publishing ports 3001 (for Bitcore) and 8333 (for bitcoind).

```
docker run -d --restart=always --name litecore \
   -p 3001:3001 -p 8333:8333 \
   -v litecore_data:/root/.litecoin \
   mambix/litecore
```

If you want to run Litecore on the Testnet, first you need to add a configuration file to the data volume indicating that. Locate the data volume mount location on the Docker host:

```
docker volume inspect litecore_data | grep Mountpoint
```

There is file named `litecore-node.json` created in that directory, update it with the following contents:

```
{
  "datadir": "/root/.litecoin/data",
  "network": "testnet",
  "port": 3001,
  "services": [
    "litecoind",
    "db",
    "address",
    "web",
    "insight-api",
    "insight-ui"
  ]
}
```

You can change settings here if needed and restart the container to take effect. In `bitcoin.conf` file
you wll find settings for the node daemon if you need to adjust parameters. I would suggest adding this line to the end:
```
maxconnections=500
```

__Also don't forget to change the user/password for RPC access if your insight is exposed to the internet!__

# Node sync
Initial sync of the data can take a long long long time. That is especially true on a low end hardweare.
But once the node is synced it does not need a lot of resources to run.  

I prepared a torrent file that shares a pre-synced data folder (__can be found under releases__). Once you download the archive you need
to decompressed it and copy the files to your litecore_data volume undex `_data` folder. On ubuntu
that is usually found under `/var/lib/docker/volumes/litecore_data/_data`.
Once all the files are copied start the container and it will start syncing from block 2M+ instead of block zero.

__Please also continue to seed the torrent data once you download it, especially if you are on a good connection. The more of us there is to share the faster other can DL and sync their nodes, thank you!__

[Dockerfile]: <https://github.com/mambix/litecore/blob/master/Dockerfile>
[GitHub repo]: <https://github.com/litecoin-project/litecore>
[Litecoin site]: <https://litecoin.com/>
