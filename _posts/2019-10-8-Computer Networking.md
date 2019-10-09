---
title: Computer Networking A Top Down Approach
layout: posts
tags: computer_network
use_math: true
---

1. ## Computer Networks and the Internet

    1. ~~**What Is The Internet?**~~

    2. ~~**The Network Edge**~~

    3. **The Network Core**

        1. **Packet Switching**

            Store-and-Forward Transmission

            * In a network application, end systems exchange **messages** with each other.
            * To send a message from a source end system to a destination end system, the source breaks long messages into smaller chunks of data known as **packets**.
            * if a source end system or a packet switch is sending a packet of **L** bits over a link with transmission rate **R** bits/sec, then the time to transmit the packet is **L/R** seconds.
            * Most packet switches use **store-and-forward** transmission at the inputs to the links.
            * Only after the router has received **all** of the packet’s **bits** can it begin to transmit (i.e., “forward”) the packet onto the outbound link.
            * sending one packet from source to destination over a path consisting of **N** links each of rate **R** (thus, there are *N-1* routers between source and destination). Applying the same logic as above, we see that the end-to-end delay is : $d_\mbox{end-to-end} = {N\times{L}\over{R}}$

            Queuing Delays and Packet Loss

            * Each packet switch has multiple links attached to it. For each attached link, the packet switch has an **output buffer** (also called an output queue), Since the amount of buffer space is finite, an arriving packet may find that the buffer is completely full with other packets waiting for transmission. In this case, **packet loss** will occur.

            Forwarding Tables And Routing Protocols

            * each router has a forwarding table that maps destination addresses (or portions of the destination addresses) to that router’s outbound links.
            * the Internet has a number of special routing protocols that are used to automatically set the forwarding tables.
            * We now invite you to get your hands dirty by interacting with the Traceroute program. Simply visit the site www.traceroute.org, choose a source in a particular country, and trace the route from that source to your computer.

        2. **Circuit Switching**

            * In circuit-switched networks, the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are reserved for the duration of the communication session between the end systems.

            Multiplexing in Circuit-Switched Networks

            * the link dedicates a frequency band to each connection for the duration of the connection. In telephone networks, this frequency band typically has a width of 4 kHz (that is, 4,000 hertz or 4,000 cycles per second). The width of the band is called, not surprisingly, the bandwidth.

            Packet Switching Versus Circuit Switching

            * packet switching is more efficient.
            * packet switching is not suitable for real-time services due to end-to-end delays.
            * Circuit switching pre-allocates use of the transmission link regardless of demand, with allocated but unneeded link time going unused.
            * unused. Packet switching on the other hand allocates link use on demand.

        3. **A Network of Networks**

            * Connecting end users and content providers into an access ISP is only a small piece of solving the puzzle of connecting the billions of end systems that make up the Internet. To complete this puzzle, *the access ISPs themselves must be interconnected*. This is done by creating a **network of networks**—understanding this phrase is the key to understanding the Internet.

            * Our first network structure, Network Structure 1, *interconnects all of the access ISPs with a single global transit ISP*. Our (imaginary) global transit ISP is a network of routers and communication links that not only spans the globe, but also *has at least one router near each of the hundreds of thousands of access ISPs*.To be profitable, it would naturally charge each of the access ISPs for connectivity, with the pricing reflecting (but not necessarily directly proportional to) the amount of traffic an access ISP exchanges with the global ISP. Since the access ISP pays the global transit ISP, the access ISP is said to be a *customer* and the global transit ISP is said to be a *provider*.

            * Network Structure 2, which consists of the hundreds of thousands of access ISPs and multiple global transit ISPs. The access ISPs certainly prefer Network Structure 2 over Network Structure 1 since they can now choose among the competing global transit providers as a function of their pricing and services.Note, however, that the global transit ISPs themselves must interconnect: Otherwise access ISPs connected to one of the global transit providers would not be able to communicate with access ISPs connected to the other global transit providers.

            * In reality, although some ISPs do have impressive global coverage and do directly connect with many access ISPs, no ISP has presence in each and every city in the world. Instead, in any given region, there may be a regional ISP to which the access ISPs in the region connect. Each regional ISP then connects to tier-1 ISPs. Tier-1 ISPs are similar to our (imaginary) global transit ISP; but tier-1 ISPs, which actually do exist, do not have a presence in every city in the world. There are approximately a dozen tier-1 ISPs, including Level 3 Communications, AT&T, Sprint, and NTT. Interestingly, no group officially sanctions tier-1 status; as the saying goes—if you have to ask if you’re a member of a group, you’re probably not.

            * Returning to this network of networks, not only are there multiple competing tier- 1 ISPs, there may be multiple competing regional ISPs in a region. In such a hierarchy, each access ISP pays the regional ISP to which it connects, and each regional ISP pays the tier-1 ISP to which it connects. (An access ISP can also connect directly to a tier-1 ISP, in which case it pays the tier-1 ISP). Thus, there is customer-provider relationship at each level of the hierarchy. Note that the tier-1 ISPs do not pay anyone as they are at the top of the hierarchy. To further complicate matters, in some regions, there may be a larger regional ISP (possibly spanning an entire country) to which the smaller regional ISPs in that region connect; the larger regional ISP then connects to a tier-1 ISP. For example, in China, there are access ISPs in each city, which connect to provincial ISPs, which in turn connect to national ISPs, which finally connect to tier-1 ISPs [Tian 2012]. We refer to this multi-tier hierarchy, which is still only a crude approximation of today’s Internet, as Network Structure 3.

            * As we just learned, customer ISPs pay their provider ISPs to obtain global Internet interconnectivity. The amount that a customer ISP pays a provider ISP reflects the amount of traffic it exchanges with the provider. To reduce these costs, a pair of nearby ISPs at the same level of the hierarchy can peer, that is, they can directly connect their networks together so that all the traffic between them passes over the direct connection rather than through upstream intermediaries. When two ISPs peer, it is typically settlement-free, that is, neither ISP pays the other. As noted earlier, tier-1 ISPs also peer with one another, settlement-free. For a readable discussion of peering and customer-provider relationships, see [Van der Berg 2008]. Along these same lines, a third-party company can create an Internet Exchange Point (IXP) (typically in a stand-alone building with its own switches), which is a meeting point where multiple ISPs can peer together. There are roughly 300 IXPs in the Internet today [Augustin 2009]. We refer to this ecosystem—consisting of access ISPs, regional ISPs, tier-1 ISPs, PoPs, multi-homing, peering, and IXPs—as Network Structure 4.

            * Network Structure 5, which describes the Internet of 2012. builds on top of Network Structure 4 by adding **content provider networks**.

    4. Delay Loss And Throughput in Packet-Switched Networks

        1. Overview of Delay in Packet-Switched Networks

            * The most important of these delays are the nodal processing delay, queuing delay, transmission delay, and propagation delay; together, these delays accumulate to give a total nodal delay.

            Types of Delay

            * **Processing Delay** The time required to examine the packet’s header and determine where to direct the packet is part of the processing delay.The processing delay can also include other factors, such as the time needed to check for bit-level errors in the packet.Processing delays in high-speed routers are typically on the order of microseconds or less.less. After this nodal processing, the router directs the packet to the **queue** that precedes the link to router B.

            * **Queuing Delay** At the queue, the packet experiences a queuing delay as it waits to be transmitted onto the link. The length of the queuing delay of a specific packet will depend on the number of earlier-arriving packets that are queued and waiting for transmission onto the link. Queuing delays can be on the order of microseconds to milliseconds in practice.

            * **Transmission Delay** 

        2. Queuing Dealy And Packet Loss

        3. End-to-End Delay

        4. Throughput in Computer Networks
    5. Protocol Layers And Their Service Models
        1. Layered Architecture
        2. Encapsulation
    6. Network Under Attack
    7. History of Compuer Networking and The History
        1. The Development of Packet Switching 1961-1972
        2. Proprietary Networks and Internetworking 1972-1980
        3. A Proliferation of Networks 1980-1990
        4. The Internet Explosion The 1990s
        5. The New Millennium
    8. Summary

    Homework Problems and Questions

    Wireshark Lab

    Interview: Leonard Kleinrock

2. Application Layer
    1. Principles of Network Applications
    2. The Web and HTTP
    3. File Transfer:FTP
    4. Electronic Mail in the internet
    5. DNS-The internet's Directory Service
    6. Peer-to-Peer Applications
    7. Socket Programing:Creating Network Applications
    8. Summary

    Homework Problems and Questions

    Socket Programing Assignments

    Wireshark Labs: HTTP,DNS.

    Interview: Marc Andreessen

3. Transport Layer
    1. Introduction and Transport-Layer Services
    2. Multiplexing and Demultiplexing
    3. Connectionless Transport:UDP
    4. Principles of Reliable Data Transfer
    5. Connection-Oriented Transport : TCP
    6. Principles of Congestion Control
    7. TCP Congestion Control
    8. Summary

    Homework Problems and Questions

    Programing Assignments

    Wireshark Labs:TCP,UDP

    Interview: Van Jacobson

4. The Network Layer
    1. Introduction
    2. Virtual Circuit and Datagram Networks
    3. What's inside a Router?
    4. The Internet Protocol(IP):Forwarding an Addressing in internet
    5. Routing Algorithms
    6. Routing in the Internet
    7. Broadcast and Multicast Routing
    8. Summary

    Homework Problems and Questions

    Programing Assignments

    Wireshark Labs: IP,ICMP

    Interview:Vinton G. Cerf

5. The Link Layer: Links,Access Networks,and LANs
    1. Introduction to the Link Layer
    2. Error-Detection and -Correction Techniques
    3. Multiple Access Links and Protocols
    4. Switched Local Area Networks
    5. Link Virtualization:A Network as a Link Layer
    6. Data Center Networking
    7. Retrospective: A Day in the Life of a Web Page Request
    8. Summary
    Programing Assignments

    Wireshark Labs: Ethernet and ARP,DHCP

    Interview: Simon S.Lam

6. Wirless And Mobile Networks

7. Multimedia Networking

8. Security in Computer Networks

9. Network Management
