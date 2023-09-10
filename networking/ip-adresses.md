# IP adresses

Every location or device on a network must be addressable. In the normal TCP/IP model of network layering, this is handled on a few different layers, but usually when we refer to an address on a network we are talking about an IP address.

**Each IP address must be unique on its own network.** Networks can be isolated from one another, and they can be bridged and translated to provide access between distinct networks. A system called **Network Address Translation**, allows the addresses to be rewritten when packets traverse network borders to allow them to continue on to their correct destination. This allows the same IP address to be used on multiple, isolated networks while still allowing these to communicate with each other if configured correctly.

## IPv4 Addresses Classes and Reserved Ranges

- Class A  

0--- : If the first bit of an IPv4 address is “0”, this means that the address is part of class A. This means that any address from 0.0.0.0 to 127.255.255.255 is in class A.

- Class B  

10-- : Class B includes any address from 128.0.0.0 to 191.255.255.255. This represents the addresses that have a “1” for their first bit, but don’t have a “1” for their second bit.

- Class C  

110- : Class C is defined as the addresses ranging from 192.0.0.0 to 223.255.255.255. This represents all of the addresses with a “1” for their first two bits, but without a “1” for their third bit.

- Class D  

1110 : This class includes addresses that have “111” as their first three bits, but a “0” for the next bit. This address range includes addresses from 224.0.0.0 to 239.255.255.255.

- Class E  

1111 : This class defines addresses between 240.0.0.0 and 255.255.255.255. Any address that begins with four “1” bits is included in this class.

> Class D addresses are reserved for multi-casting protocols, which allow a packet to be sent to a group of hosts in one movement.
>
> Class E addresses are reserved for future and experimental use, and are largely not used.

Traditionally, each of the regular classes (A–C) divided the networking and host portions of the address differently to accommodate different sized networks.  
Class A addresses used the remainder of the first octet to represent the network and the rest of the address to define hosts. This was good for defining a few networks with a lot of hosts each.  
The class B addresses used the first two octets (the remainder of the first, and the entire second) to define the network and the rest to define the hosts on each network.  
The class C addresses used the first three octets to define the network and the last octet to define hosts within that network.

## Reserved Private Ranges

One of the most useful reserved ranges is the loopback range specified by addresses from 127.0.0.0 to 127.255.255.255. This range is used by each host to test networking to itself. Typically, this is expressed by the first address in this range: 127.0.0.1.

Each of the normal classes also have a range within them that is used to designate private network addresses.  
For class A addresses, the addresses from 10.0.0.0 to 10.255.255.255 are reserved for private network assignment.  
For class B, this range is 172.16.0.0 to 172.31.255.255.  
For class C, the range of 192.168.0.0 to 192.168.255.255 is reserved for private usage.

Any computer that is not hooked up to the internet directly (any computer that goes through a router or other NAT system) can use these addresses at will.

## Netmasks and Subnets

The process of dividing a network into smaller network sections is called **subnetting**.

By default, each network has only one subnet, which contains all of the host addresses defined within. A **netmask** is basically a specification of the amount of address bits that are used for the network portion. A subnet mask is another netmask within used to further divide the network.

For instance, a netmask of `255.255.255.0` leaves us with 254 hosts in the network (you cannot end in 0 or 255 because these are reserved).

## CIDR Notation

A system called **Classless Inter-Domain Routing, or CIDR**, was developed as an alternative to traditional subnetting. The idea is that you can add a specification in the IP address itself as to the number of significant bits that make up the routing or networking portion.

For example, we could express the idea that the IP address `192.168.0.15` is associated with the netmask `255.255.255.0` by using the CIDR notation of `192.168.0.15/24`. This means that the first 24 bits of the IP address given are considered significant for the network routing.
