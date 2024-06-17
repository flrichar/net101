### Network 101 Workshop

✅ Session 1, Network Intro 

* Introduction
   * Fred’s Personal Opinion sprinkled throughout. I was a hardware and infrastructure person and interested in SDN & NFV
* What are the purpose & goals of this presentation?
   * Outline networking generally, forge a foundation for higher complexity later. Keep it simple, SUSE. 
   * Context to Kubernetes, Containers and Linux, outlined similar to O'Reilly Book - https://www.oreilly.com/library/view/networking-and-kubernetes/9781492081647/
   * Generate more workshop sessions.

* What are the basics? Two classifications: Infrastructure/Mapping & Operations/How Things Work
   * Like an adventure video game. Map of the Realm/World, then How to play, the rules and standards
* What are OSI Layers?  
   * What are the main Layers we are concerned with?  It depends… 
* What is Subnetting? 
   * Network Fractions, Can you cheat with subnetting? Yes! … ipcalc … https://packetlife.net/library/cheat-sheets 
* Who cares about MTU? 
   * Its 1500 Globally, 9000 Locally/Privately, Encapsulation for tunnels or abstraction eats into MTU.
* What are some things Fred dislikes? 
   * Proxies, NAT, iptables. HTTP, IPv4 (to a degree). Simple is better, more efficient.  

* What are some general troubleshooting tips?
   * Direct terminology, use common words to describe specific contextual details.
   * Mock up a lab environment.  Diagrams, _“this is everyone’s typical home network”_
   * Establish a Reference Architecture, introduce my home lab diagram.
   * Zoom out to see context, big picture, then zoom in to see the specific minute details.
   * What is the Structure of a Packet?  Headers & Body, how does a Packet (L3) differ from a Frame (L2), or Segment (L4)? Diagram this out.
   * What are some Specific tools? - mtr, ping/traceroute, proving the current and lower layers are stable. 

* What is a TCP session? 
   * It is much like a phone conversation, 3-way Handshake. 
   * What do I think of TCP?  There are modern, better alternatives like QUIC (http/3) https://quicwg.org … more efficient than TCP + TLS
   * Are packet captures really necessary? 
   * When the payload or body of the packet is encrypted, you're really just looking into flows & headers.
   * What are some good tools? Tshark, wireshark, tcpdump, netflow, conntrack.

* Cloud Native Computing focuses on L3/L4 & L7 mostly.  
   * What does a router do? … decides how to send packets from Interface to Interface. Switch does this for frames & switchports.
   * What is the difference between Routed Protocols & Routing Protocols?  Routed = IP , Routing = BGP
* Who cares about L2 & L1? 
   * Cloud Native doesn’t, in fact it's abstracted away.  Keep this in mind when troubleshooting. 

* What are some References and resources?  Do we have Q&A discussion topics? Do we have future workshop ideas? 
   * https://packetlife.net/library/cheat-sheets
   * https://frrouting.org
   * https://www.openvswitch.org
   * `#discuss-ipnetworking` on SUSE Company Slack
---


✅ Session 2, Linux Networking, Kubernetes Containers & Pods

### Linux Networking
* What is a Network Interface?

   * What are Interfaces? They can be real hardware or virtual, logical, or derived devices, used to communicate to the outside world.
   * How do we make a mental map?  Analyze interfaces & route tables with iproute2;  `ip route show` ... `ip address show`

* What is a Bridge Interface - Virtual Switch!

   * Allows for multilple L2 interfaces on a single host.
   * L2 switches, brctl, OVS, vde2.  Network namespace & veth pairs mentioned initially, crucial for CNI, containers & Kubernetes.
   * `brctl show`

* What are some Linux network tools?

   * `brctl`, `iproute2`, `nmap`, `netcat`, `dig`, `ss`, `curl`, `ping`, `mtr`, `traceroute`, `conntrack`
   * Demo container networking with `systemd-cgls` and `nsenter` 

### Kubernetes Networking
* Where are the Kubernetes Boundaries?

   * Bridge between the containers in the pods' network namespace and the host's root namespace.  _(see diagram)_
   * Overlay Network. Encapsulation, a _“network-inside-a-network”_ ... "Fake" L2 network over a L3 physical network.
   * VXLAN Tunnels between nodes, can also be other protocols like IPIP or Wireguard.

* How do we describe the _Life of a Packet_ in the context of a cluster?

   * Concerned with either traffic inside or outside of the cluster, this boundary exists on the node itself.
   * DNAT in-to, SNAT out-of Cluster
   * Source -> Load Balancer -> Ingress FE -> BE Service FE -> BE Endpoints / Pods
 
