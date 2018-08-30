# Docker OpenVPN Client

Original idea borrowed from https://github.com/dperson/openvpn-client

1. You should add the generated openvpn client config to a directory, you can call it client.ovpn
2. You should add your username and password to `client.pwd`
3. Run the following, I recommend adding `--auth-nocache`

```
docker run -d --name vpn-client \
  --cap-add=NET_ADMIN \
  --device /dev/net/tun \
  -v /path/with/vpn/configs:/vpn \
  ekristen/openvpn-client --config /vpn/client.conf --auth-user-pass /vpn/client.pwd --auth-nocache
```

### Route container traffic

Use `--net=container:<container-id>` -- routes available by the VPN client will be made available to the container.

```
docker run -it --rm \
  --net=container:vpn-client
  ubuntu /bin/bash
```

### Restart on disconnect
If you want the container to restart on disconnect, add 
```
--inactive 3600 --ping 10 --ping-exit 60
```
after `--auth-nocache`.
