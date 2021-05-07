# MNP

MNP is a mesh routing protocol for Linux based operating systems.
The academic paper with more theoretical details can be found [here](http://dsg.ac.upc.edu/node/843).

## Content

*   [Installation](#installation)
    *   [Installing in MeshWRT](#installing-in-meshwrt)
    *   [Packages](#Packages)
*   [FAQ](#faq)
*   [Usage](doc/Usage.md)
*   [Concepts](doc/Concepts.md)
    *   [Autoconfiguration](doc/Usage.md#address-auto-and-manual-configuration)
    *   [Unicast Host Network Announcements (UHNA)](doc/Usage.md#unicast-host-network-announcements-uhna)
*   [Tunnel Announcements](doc/Tunneling.md)
*   [MNP Plugins](doc/Plugins.md)
*   [Debugging](doc/Debugging.md)

## Installation ##

### Requirements ###

The following tools and libraries are needed to obtain, compile, and install MNP:
* git (debian package: git-core)
* gcc
* make
* build-essential
* libjson-c-dev zlib1g-dev libiw-dev
* libmbedtls-dev ( or mbedtls-2.4.0 from https://tls.mbed.org/download/mbedtls-2.4.0-gpl.tgz)

Optional for static configuration:
* uci-0.7.5 from http://downloads.openwrt.org/sources/uci-0.7.5.tar.gz

The following Linux-kernel modules are needed (depending on used MNP features)
* ipv6
* tunnel6
* ip6_tunnel

The mbed TLS or PolarSSL crypto library is needed for cryptographic operations:
Most tested with debian or mbedtls-2.4.0:
```
wget https://tls.mbed.org/download/mbedtls-2.4.0-gpl.tgz
tar xzvf mbedtls-2.4.0-gpl.tgz
cd mbedtls-2.4.0
make
sudo make install
# compile MNP with: make EXTRA_CFLAGS="-DCRYPTLIB=MBEDTLS_2_4_0"
```

### Downloading

Latest development sources are available from MNP git repository:

```
git clone https://github.com/meshcoin-project/MNP.git
cd MNP
```

### Compile and Install

To only compile the main MNP daemon (no MNP plugins):
```
make EXTRA_CFLAGS="-DCRYPTLIB=MBEDTLS_2_4_0"
sudo make install
```

## Installing in OpenWRT

MNP is currently in the official OpenWRT-routing feed, so to install it from a existing system you can use opkg:
```
opkg install MNP MNP-uci-config
```

If you are compiling your own MeshWRT, you can add the routing feed (already enabled by default) which can be found here: https://github.com/meshcoin-project/packages

Then run "make menuconfig" and select the MNP package in Networking -> Routing and redirection

It is recommended to select also, at least, the uci plugin (MNP-uci-config)

You can select "luci-app-MNP" to have a nice web interface for manage and monitorize the routing daemon.

Finally type "make" to build the image.

## Packages
Available packages exist for the following distributions:
- Arch Linux package(AUR): https://aur.archlinux.org/packages/MNP/
- Debian Linux package(deb): **Coming soon**

## FAQ
1. How does MNP work and on which OSI layer?
- MNP is a routing protocol that operates on layer 3 of the OSI layer; it
    extends the concept of **receiver-driven routing**  and the principles of
    DSDV routing. The routing update of MNP (in contrast to traditional DSDV)
    contains a single and verifiable heartbeat value which unambiguously
    identifies a particular node of the network and a specific version of this
    nodes' self-defined description and routing-update version.

2. The goal of MNP/SEMTOR?
- The goal of MNP is to provide secure mechanisms to ensure that
	non-trusted nodes in an open network are effectively prevented from
	disrupting the routing between trusted nodes.
- It's achieved by enforcing the exclusion of a given set of identified faulty nodes.

3. Differences with bmx6
- TBD

4. Similar Software
- AODV,
- Babel,
- BMX6,
- OLSR,
- batman-adv
