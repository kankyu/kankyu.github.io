---
layout: post
title:  How to configure SSH from host to VirtualBox Ubuntu server
date:   2019-04-19
categories: ["Linux"]
---

To be able to ssh from the host onto the VirtualBox Ubuntu server.

## Solution 1

Using NAT mode[^networking-mode] (not the NAT network mode) and port forwarding ssh on the host to guest vm.

In the virtualbox settings > network > port forwarding:
HOSTPORT - port number that you would like
Guest IP - found on the virtual machine network card interface, e.g. through using `ifconfig` and look for eth, enp0. Refer to the inet IP value.
Why Guest Port 22?

```yaml
Protocol: TCP
Host IP: 127.0.0.1
Host Port: HOSTPORT
Guest IP: GUESTIP
Guest Port: 22
```

After this has been set up you should be able to ssh onto your ubuntu server from your host[^ssh-host-vm-guide]

```bash
ssh -p HOSTPORT LOGINNAME@127.0.0.1
```

You should have been able to connect at this point.

## Solution 2 WIP

Without the use of port forwarding, be able to ssh onto the ubuntu server using it's IP, more specifically the assigned IP of the network card (host-only network adapter) that supports communication between host to virtual machine.

* Allows other VirtualBox VMs and host to communicate the VM with Host-only adapter as if they are connected through a physical ethernet switch[^networking-mode].

Attach two adapters to the virtual machine:

1. NAT - for access to the internet
2. Host-only - for allowing communication between host to virtual machine

[Purpose of DHCP server]
A DHCP server provides and assigns IP addresses, default gateways and other network parameters to client devices[^dhcp]. Without a DHCP server in our case, every VM has to be manually set up for communicating with the host. The DHCP assigns each client with a unique dynamic IP address.

## Setting up Static IP on the Ubuntu server 18.04 WIP

WIP

[^networking-mode]: [https://www.virtualbox.org/manual/ch06.html#networkingmodes](https://www.virtualbox.org/manual/ch06.html#networkingmodes)
[^ssh-host-vm-guide]: [https://bobcares.com/blog/virtualbox-ssh-nat/](https://bobcares.com/blog/virtualbox-ssh-nat/)
[^dhcp]: [https://www.infoblox.com/glossary/dhcp-server/](https://www.infoblox.com/glossary/dhcp-server/)
