---
title:  "Easily Setup OpenVPN Bastion Server with Docker"
---

`OpenVPN` is a full-featured open source Secure Socket Layer (SSL) VPN solution. While trying to secure amazon elasticsearch service, we decided that an openvpn bastion server would be the simplest way for us to securely access `kibana` and `elasticsearch service` from our home, office, or anywhere as long as we have proper client certificate.

# Setup
We use a small t2.micro amazon linux ami with `elastic ip` attached and `docker` installed on the machine, the `security group` is set to open udp on port `1194` to `0.0.0.0/32` cidr range. The server sit in a public subnet of the vpc.

OpenVPN itself is ran using the docker image `https://github.com/kylemanna/docker-openvpn`, this is an amazing image that really ease the setup and creation of client certificate of the `OpenVPN` server. 

Once we havethe OpenVPN server is running, and the client `.ovpn` files generated, the files can be distributed to the users that require access. We are ready to connect to the vpn.

I use [TunnelBlick](https://tunnelblick.net) as my openvpn client, which is free. Simply install it and drag the `.ovpn` file to `TunnelBlick` and everything is setup.

Once connected, we could securely access `amazon elasticsearch` and `kibana` using its `vpc endpoint`.

# Conclusion
With `docker`, the installation of `OpenVPN` is easy and simplified. Using `OpenVPN` as a bastion server is a good way to allow secure access to our aws's vpc.