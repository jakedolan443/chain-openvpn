# chain-openvpn

Gateway chaining tool for multiple OpenVPN configurations written in python

## Usage 

Each configuration connects chronologically, with the bottom line of the file representing the default gateway.
```
git clone https://github.com/jakedolan443/chain-openvpn
cd chain-openvpn
chmod +x chain-vpn
sudo ./chain-vpn
```
OpenVPN configuration files (.ovpn) should be placed into `connect.conf`, separated by one line, e.g.
```
client1.ovpn
client2.ovpn
client3.ovpn
```
Each file should contain authentication within via `auth-user-pass file.txt`, see `man openvpn` for help

Tested on Debian 10, Ubuntu 20.04

## How it works

Each VPN configuration file is connected chronologically, using a different `tun` interface each time. There is no hard limit to how many interfaces and configurations can be used but a greater amount may reduce performance and introduce security issues; it is recommended to have no more than 5.

Traffic is routed through each connected server, e.g.

`in > VPN1 > VPN2 > VPN3 > VPN4 > VPN5 > out`

The default gateway is set to the last configuration in the chain, and all traffic is routed through all `tun` interfaces to reach it.

