---
title: Computer Networking A Top Down Approach
layout: posts
tags: computer network
---

1. Computer Networks and the Internet
    1. ~~What Is The Internet?~~

    2. ~~The Network Edge~~

    3. The Network Core

        1. Packet Switching
            Store-and-Forward Transmission

            * In a network application, end systems exchange **messages** with each other.
            * To send a message from a source end system to a destination end system, the source breaks long messages into smaller chunks of data known as **packets**.
            * if a source end system or a packet switch is sending a packet of **L** bits over a link with transmission rate **R** bits/sec, then the time to transmit the packet is **L/R** seconds.
            * Most packet switches use **store-and-forward** transmission at the inputs to the links.
            * Only after the router has received **all** of the packet’s **bits** can it begin to transmit (i.e., “forward”) the packet onto the outbound link.
            * sending one packet from source to destination over a path consisting of **N** links each of rate **R** (thus, there are *N-1* routers between source and destination). Applying the same logic as above, we see that the end-to-end delay is : ${d_{\mbox{end-to-end}} = N{{L}\over{R}}}$

            Queuing Delays and Packet Loss

            * Each packet switch has multiple links attached to it. For each attached link, the packet switch has an **output buffer** (also called an output queue), Since the amount of buffer space is finite, an arriving packet may find that the buffer is completely full with other packets waiting for transmission. In this case, **packet loss** will occur.

            Forwarding Tables And Routing Protocols

            * each router has a forwarding table that maps destination addresses (or portions of the destination addresses) to that router’s outbound links.
            * the Internet has a number of special routing protocols that are used to automatically set the forwarding tables.
            * We now invite you to get your hands dirty by interacting with the Traceroute program. Simply visit the site www.traceroute.org, choose a source in a particular country, and trace the route from that source to your computer.

        2. Circuit Switching
            * In circuit-switched networks, the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are reserved for the duration of the communication session between the end systems.

            Multiplexing in Circuit-Switched Networks

            * the link dedicates a frequency band to each connection for the duration of the connection. In telephone networks, this frequency band typically has a width of 4 kHz (that is, 4,000 hertz or 4,000 cycles per second). The width of the band is called, not surprisingly, the bandwidth.

            Packet Switching Versus Circuit Switching

            * packet switching is more efficient.
            * packet switching is not suitable for real-time services due to end-to-end delays.
            * Circuit switching pre-allocates use of the transmission link regardless of demand, with allocated but unneeded link time going unused.
            * unused. Packet switching on the other hand allocates link use on demand.

        3. A Network of Networks

2. Application Layer

3. Transport Layer

4. The Network Layer

5. The Link Layer: Links,Access Networks,and LANs

6. Wirless And Mobile Networks

7. Multimedia Networking

8. Security in Computer Networks

9. Network Management
