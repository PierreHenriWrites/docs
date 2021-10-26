---
title: 'Configuring the network on Proxmox VE on the High Grade & SCALE ranges'
slug: proxmox-network-hg-scale
excerpt: 'Find out how to configure the network on Proxmox VE on the High Grade & SCALE ranges'
section: 'Advanced use'
order: 5
---

**Last updated 4th October 2021**

## Objective

On the High Grade & SCALE ranges, it is not possible to operate failover IPs in *bridged* mode (via virtual MACs). It is therefore necessary to configure failover IPs in routed mode or via the vRack.

**This guide explains how to configure the network in Proxmox VE.**

## Requirements

- an [OVHcloud dedicated server](https://www.ovhcloud.com/en-ie/bare-metal/)
- access to the [OVHcloud Control Panel](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.ie/&ovhSubsidiary=ie)
- a [failover IP](https://www.ovhcloud.com/en-ie/bare-metal/ip/)

> [!warning]
>
> No virtual MACs should be applied to failover IPs in the OVHcloud Control Panel.
>

## Instructions

> [!primary]
>
> On these server ranges, there are 4 network cards. The first two for the public, the last two for the private network. To get all the bandwidth, aggregates must be created.
>

### Failover IP in routed mode on public network interfaces

#### Target configuration schema

![route diagram](images/schema_route2022.png){.thumbnail}

#### Explanations

You need to:

- create an aggregate
- create a bridge
- allow forwarding and add routes.

#### Configure the hypervisor

The entire configuration is done in the `/etc/network/interfaces` file:

```bash
vi /etc/network/interfaces
```

```bash
auto lo
iface lo inet loopback

# public interface 1
auto ens33f0
iface ens33f0 inet manual

# public interface 2
auto ens33f1
iface ens33f1 inet manual

# private interface 1
auto ens35f0
iface ens35f0 inet manual

# private interface 2
auto ens35f1
iface ens35f1 inet manual

auto bond0
# LACP aggregate on public interfaces
# configured in DHCP mode on this example
# Has the server's public IP
iface bond0 inet dhcp
	bond-slaves ens33f0 ens33f1
        bond-miimon 100
	bond-mode 802.3ad
        post-up echo 1 > /proc/sys/net/ipv4/conf/bond0/proxy_arp
        post-up echo 1 > /proc/sys/net/ipv4/ip_forward

#Private

auto vmbr0
# Configure the bridge with a private address and add route(s) to send the failover IPs to it
# A.B.C.D/X => Subnet of failover IPs assigned to the server, this can be a host with /32
iface vmbr0 inet static
	address 192.168.0.1
        netmask 255.255.255.255
	bridge-ports none
	bridge-stp off
	bridge-fd 0
        post-up ip route add A.B.C.D/X dev vmbr0
```

At this point, restart the network services or restart the server.

#### Configuration example of a client VM on Debian

File contents `/etc/network/interfaces`:

```bash
auto lo ens18
iface lo inet loopback
iface ens18 inet static
    address IP_FO
    netmask 255.255.255.255
    gateway 192.168.0.1
```

### Failover IP via vRack

#### Requirements

- a public block of IP addresses in your account, with a minimum of four addresses
- your chosen private IP address range
- a [vRack compatible server](https://www.ovhcloud.com/en-ie/bare-metal/){.external}
- a [vRack](https://www.ovh.ie/solutions/vrack/){.external} service activated in your account
- access to the [OVHcloud Control Panel](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.ie/&ovhSubsidiary=ie)


#### Target configuration schema

![vrack diagram](images/schema_vrack2022.png){.thumbnail}

#### Explanations

You need to:

- create an aggregate
- create a bridge connected to the aggregate

#### Configure a usable IP address

For vRack, the first, penultimate, and last addresses in a given IP block are always reserved for the network address, network gateway, and network *broadcast* respectively. This means that the first usable address is the second address in the block, as shown below:

```sh
46.105.135.96 # Reserved: network address
46.105.135.97 # First usable IP
46.105.135.98
46.105.135.99
46.105.135.100
46.105.135.101
46.105.135.102
46.105.135.103
46.105.135.104
46.105.135.105
46.105.135.106
46.105.135.107
46.105.135.108
46.105.135.109 # Last usable IP
46.105.135.110 # Reserved: network gateway
46.105.135.111 # Reserved: network broadcast
```

To configure the first usable IP address, you must edit the network configuration file as shown below. In this example, we use a subnet mask of **255.255.255.240**.

> [!primary]
>
> The subnet mask used in this example is appropriate for our IP block. Your subnet mask may differ depending on the size of your block. When you purchase your IP block, you will receive an email notifying you of the subnet mask to use.
>

#### Configure the hypervisor

The entire configuration is done in the `/etc/network/interfaces` file:

```bash
vi /etc/network/interfaces
```

What matters here is the `bond1` and `vmbr1` configuration:

```bash
auto lo
iface lo inet loopback

# public interface 1
auto ens33f0
iface ens33f0 inet manual

# public interface 2
auto ens33f1
iface ens33f1 inet manual

# private interface 1
auto ens35f0
iface ens35f0 inet manual

# private interface 2
auto ens35f1
iface ens35f1 inet manual

auto bond0
iface bond0 inet dhcp
	bond-slaves ens33f0 ens33f1
        bond-miimon 100
	bond-mode 802.3ad
        post-up echo 1 > /proc/sys/net/ipv4/conf/bond0/proxy_arp
        post-up echo 1 > /proc/sys/net/ipv4/ip_forward

auto bond1
# LACP Aggregate on private interfaces
# No IPs on it
iface bond1 inet manual
	bond-slaves ens35f0 ens35f1
        bond-miimon 100
	bond-mode 802.3ad


#Private

auto vmbr1
# Bridge connected to bond1 aggregate
# No need for IP
iface vmbr1 inet manual
	bridge-ports bond1
	bridge-stp off
	bridge-fd 0

```

At this point, restart the network services or restart the server.

#### Configuration example of a client VM on Debian

Content of the file `/etc/network/interfaces`:

```bash
auto lo ens18
iface lo inet loopback
iface ens18 inet static
    address 46.105.135.97
    netmask 255.255.255.240
    gateway 46.105.135.110
```

## Go further

Join our community of users on <https://community.ovh.com>.
