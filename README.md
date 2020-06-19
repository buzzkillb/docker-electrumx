
# docker-electrumx
[![Docker Automated build](https://img.shields.io/docker/automated/buzzkillb/docker-electrumx.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/buzzkillb/docker-electrumx/)
[![Docker Pulls](https://img.shields.io/docker/pulls/buzzkillb/docker-electrumx.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/buzzkillb/docker-electrumx/)
[![Docker Stars](https://img.shields.io/docker/stars/buzzkillb/docker-electrumx.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/buzzkillb/docker-electrumx/)

[![Alpine Version](https://img.shields.io/badge/Alpine%20version-v3.11-green.svg?style=for-the-badge)](https://alpinelinux.org/) 

> Run an Electrum server with one command

An easily configurable Docker image for running an Electrum server.

setup guide  
https://denariustalk.org/index.php?/topic/282-electrumx-server-setup-guide-docker/

## Usage
**Denarius**
```
docker run \
  --name=electrumx \
  --net=host \
  --ulimit nofile=5120:5120 \
  -t -d \
  -v ~/electrumx:/data \
  -e DAEMON_URL=http://user:pass@127.0.01:32369 \
  -e COIN=Denarius \
  -p 50002:50002 \
  buzzkillb/docker-electrumx:latest
```
```
docker run \
  --net=host \
  --name=denariusd \
  -t -d \
  -p 33369:33369 \
  -p 32369:32369 \
  -v ~/.denarius:/data \
  -P buzzkillb/denariusd:latest
```
## Run History Compaction on Denarius (stop electrumx server first)  
```
docker run \
  --name=electrumx-compact \
  --net=host \
  --ulimit nofile=5120:5120 \
  -t -d \
  -v ~/electrumx:/data \
  -e DAEMON_URL=http://user:pass@127.0.01:32369 \
  -e COIN=Denarius \
  -p 50002:50002 \
  buzzkillb/docker-electrumx:dcompact
  ```
crontab (daily compaction cronjob)  
```
0 0 * * * docker stop electrumx >/dev/null 2>&1
1 0 * * * docker start electrumx-compact >/dev/null 2>&1
6 0 * * * docker start electrumx >/dev/null 2>&1
```

Below is original readme
```
docker run \
  -v /home/username/electrumx:/data \
  -e DAEMON_URL=http://user:pass@host:port \
  -e COIN=BitcoinSegwit \
  -p 50002:50002 \
  lukechilds/electrumx
```

If there's an SSL certificate/key (`electrumx.crt`/`electrumx.key`) in the `/data` volume it'll be used. If not, one will be generated for you.

You can view all ElectrumX environment variables here: https://github.com/kyuupichan/electrumx/blob/master/docs/environment.rst

### TCP Port

By default only the SSL port is exposed. You can expose the unencrypted TCP port with `-p 50001:50001`, although this is strongly discouraged.

### WebSocket Port

You can expose the WebSocket port with `-p 50004:50004`.

### RPC Port

To access RPC from your host machine, you'll also need to expose port 8000. You probably only want this available to localhost: `-p 127.0.0.1:8000:8000`.

If you're only accessing RPC from within the container, there's no need to expose the RPC port.

### Version

You can also run a specific version of ElectrumX if you want.

```
docker run \
  -v /home/username/electrumx:/data \
  -e DAEMON_URL=http://user:pass@host:port \
  -e COIN=BitcoinSegwit \
  -p 50002:50002 \
  lukechilds/electrumx:v1.8.7
```

## License

MIT © Luke Childs
