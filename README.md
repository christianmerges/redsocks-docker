# Redsocks Docker image

[![Image size](https://img.shields.io/docker/image-size/myts2/redsocks-docker/latest.svg?label=docker%20image%20size)](https://hub.docker.com/r/myts2/redsocks-docker/)
[![Docker pulls](https://img.shields.io/docker/pulls/myts2/redsocks-docker.svg)](https://hub.docker.com/r/myts2/redsocks-docker/)

## Difference with ncarlier/redsocks
1. Apply with socks5 proxy
2. Add anti-dns-poisoning server(pdnsd)
3. Set `DOCKER_NET` to `""` variable and you can apply global proxy without -e DOCKER_NET by
```
docker run --privileged=true --net=host -d christianmerges/redsocks-docker 1.2.3.4 3128 user passwd
``` 
4. Smaller and smaller!

## Description

This docker image allows you to use docker on a host without being bored by your corporate proxy.

You have just to run this container and all your other containers will be able to access directly to internet (without any proxy configuration).

## Usage

Start the container like this:

```
docker run --privileged=true --net=host -d christianmerges/redsocks 1.2.3.4 3128 user passwd
```

Replace the IP and the port by those of your proxy.

The container will start redsocks and automatically configure iptable to forward **all** the TCP traffic of the `$DOCKER_NET` interface (`docker0` by default) through the proxy.

You can forward all the TCP traffic regardless the interface by unset the `DOCKER_NET` variable: `-e DOCKER_NET`.

If you want to add exception for an IP or a range of IP you can edit the whitelist file.
Once edited you can replace this file into the container by mounting it:

```
docker run --privileged=true --net=host \
  -v whitelist.txt:/etc/redsocks-whitelist.txt \
  -d christianmerges/redsocks 1.2.3.4 3128 user passwd
```

Use docker stop to halt the container. The iptables rules should be reversed. If not, you can execute this command:

```
iptables-save | grep -v REDSOCKS | iptables-restore
```

## Build

Build the image with `make`.

> Use `make help` to see available commands for this image.
