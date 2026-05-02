# 1. What is an IP Address ?

An `IP (Internet Protocol)` address is a unique numerical label assigned to every device on a network. Think of it as a postal address for machines: it tells the network where to deliver data.
- Without `IP addresses`, there's no way for your laptop to tell a router "send this packet to that web server." Every device that sends or receives data needs one.

<p>There are two versions in use today:</p>

- <strong>IPv4</strong>: uses 32 bit addresses like <code>192.168.1.3</code>. It ran out decades ago. 
- <strong>IPv6</strong>: is the replacement for ipv4 using 128-bit addresses like <code>2001:0db8:85a3::8a2e:0370:7334</code>.

<P>IP addresses operate at Layer 3 (Network layer) of the OSI model. This is the layer responsible for routing, for getting packets from one network to another across the internet. Layer 2 (Data Link) handles local delivery using MAC addresses, but once a packet needs to leave your local network, IP takes over.</P>

![ip](https://www.computernetworkingnotes.com/wp-content/uploads/ccna-study-guide/images/csg107-03-ip-routing.png)