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

Create a file named `litecore-node.json` in that directory with the following contents:

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

Now run the container, publishing ports 3001 (for Litecore) and 18333 (for litecoind on Testnet):

```
docker run -d --restart=always --name litecore \
   -p 3001:3001 -p 18333:18333 \
   -v litecore_data:/root/.litecoin \
   mambix/litecore
```


[Dockerfile]: <https://github.com/mambix/litecore/blob/master/Dockerfile>
[GitHub repo]: <https://github.com/litecoin-project/litecore>
[Litecoin site]: <https://litecoin.com/>
