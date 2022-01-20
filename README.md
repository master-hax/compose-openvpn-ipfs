# compose-vpn-ipfs

a multi-container Docker application to run an [IPFS node](https://hub.docker.com/r/linuxserver/ipfs) behind a VPN

## how to set it up

1. download [docker-compose.yml](/docker-compose.yml)
1. put your `*.ovpn` file into `./openvpn`
1. (optional) change the locations of the `ipfs-node-data-volume` & `downloads-volume`
1. (optional) add port forwarding rules using the `vpn-sidecar` environment variables
1. run `docker-compose up`

if everything works correctly, go-ipfs should be running behind your VPN & you should be able to access its web UI at http://localhost:80/webui & https://localhost:443/webui,
and its IPFS gateway at http://localhost:8080 e.g. http://localhost:8080/ipfs/QmVmtux8UCk8553R2qVa7CBYJbQ11hfyswqEJmTLYCugPx?.png

## how it works

the `ipfs-node` service shares the network stack of the `vpn-sidecar` service (OpenVPN), which is tunneled through your VPN provider. to maintain local connectivity to the `ipfs-node` container's web UI, we proxy to it to through the `web-proxy` service (Nginx) using [Docker container links](https://docs.docker.com/network/links/).

## note: a [Wireguard](https://github.com/master-hax/compose-wireguard-ipfs) version is also available
