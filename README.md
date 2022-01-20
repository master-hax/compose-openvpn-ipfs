# compose-openvpn-ipfs

a multi-container Docker application to run an [IPFS node](https://hub.docker.com/r/linuxserver/ipfs) behind an OpenVPN client

## how to set it up

1. download [docker-compose.yml](/docker-compose.yml)
1. put your `*.ovpn` file into `./openvpn`
1. run `docker-compose up`

if everything works correctly, go-ipfs should be running behind your VPN!

the IPFS web UI should be accessible at http://localhost:5001/webui

the IPFS gateway should be accessible at http://localhost:8080 e.g. http://localhost:8080/ipfs/QmVmtux8UCk8553R2qVa7CBYJbQ11hfyswqEJmTLYCugPx?.png

if you want to use this persistently, you should probably
1. change the locations of the `ipfs-node-data-volume` & `downloads-volume`
1. forward port 4001 with your VPN provider (or pick a different port)

## how it works

the `ipfs-node` service shares the network stack of the `vpn-sidecar` service (OpenVPN), which is tunneled through your VPN provider. to maintain local connectivity to the `ipfs-node` container's web UI, we proxy to it to through the `web-proxy` service (Nginx) using [Docker container links](https://docs.docker.com/network/links/).

## note: a [Wireguard](https://github.com/master-hax/compose-wireguard-ipfs) version is also available
