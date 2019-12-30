---
title: Computer Networks and the Internet
layout: posts
tags: computer_network
use_math: true
---

1. ## ~~**What Is The Internet?**~~

2. ## ~~**The Network Edge**~~

3. ## **The Network Core**

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

4. ## Delay Loss And Throughput in Packet-Switched Networks

    1. Overview of Delay in Packet-Switched Networks

        * The most important of these delays are the nodal processing delay, queuing delay, transmission delay, and propagation delay; together, these delays accumulate to give a total nodal delay.

        Types of Delay

        * **Processing Delay** The time required to examine the packet’s header and determine where to direct the packet is part of the processing delay.The processing delay can also include other factors, such as the time needed to check for bit-level errors in the packet.Processing delays in high-speed routers are typically on the order of microseconds or less.less. After this nodal processing, the router directs the packet to the **queue** that precedes the link to router B.

        * **Queuing Delay** At the queue, the packet experiences a queuing delay as it waits to be transmitted onto the link. The length of the queuing delay of a specific packet will depend on the number of earlier-arriving packets that are queued and waiting for transmission onto the link. Queuing delays can be on the order of microseconds to milliseconds in practice.

        * **Transmission Delay** packets are transmitted in a **first-come-first-served** manner, as is common in packet-switched networks, our packet can be transmitted only after all the packets that have arrived before it have been transmitted. Denote the length of the packet by **L** bits, and denote the transmission rate of the link from router A to router B by **R** bits/sec.The transmission delay is **L/R**. Transmission delays are typically on the order of microseconds to milliseconds in practice.

        * **Propagation Delay** Once a bit is pushed into the link, it needs to propagate to router B. The time required to propagate from the beginning of the link to router B is the propagation delay. The propagation delay is **d/s**, where **d** is the distance between router A and router B and **s** is the propagation speed of the link. In wide-area networks, propagation delays are on the order of milliseconds.

        Comparing Transmission and Propagation Delay

        * The transmission delay is the amount of time required for the router to push out the packet; *it is a function of the packet’s length and the transmission rate of the link, but has nothing to do with the distance between the two routers*. The propagation delay, on the other hand, is the time it takes a bit to propagate from one router to the next; *it is a function of the distance between the two routers, but has nothing to do with the packet’s length or the transmission rate of the link.*
        * If we let $d_\mbox{proc}, d_\mbox{queue}, d_\mbox{trans}, d_\mbox{prop}$ denote the processing, queuing, transmission, and propagation delays, then the total nodal delay is given by

            $d_\mbox{nodal} = d_\mbox{proc} + d_\mbox{queue} + d_\mbox{trans} + d_\mbox{prop}$

    2. Queuing Dealy And Packet Loss

        Let **a** denote the average rate at which packets arrive at the queue (a is in units of packets/sec). Recall that **R** is the transmission rate; that is, it is the rate (in bits/sec) at which bits are pushed out of the queue. Also suppose, for simplicity, that all packets consist of L bits. Then the average rate at which bits arrive at the queue is La bits/sec. Finally, assume that the queue is very big, so that it can hold essentially an infinite number of bits. The ratio **La/R**, called the **traffic intensity**, often plays an important role in estimating the extent of the queuing delay.

        * If La/R > 1,then the average rate at which bits arrive at the queue exceeds the rate at which the bits can be transmitted from the queue.

        * La/R ≤ 1. Here, the nature of the arriving traffic impacts the queuing delay. For example, if packets arrive periodically—that is, one packet arrives every L/R seconds—then every packet will arrive at an empty queue and there will be no queuing delay. On the other hand, if packets arrive in bursts but periodically, there can be a significant average queuing delay.

        Packet Loss

        * In reality a queue preceding a link has finite capacity, Because the queue capacity is finite, packet delays do not really approach infinity as the traffic intensity approaches 1. Instead, a packet can arrive to find a full queue. With no place to store such a packet, a router will drop that packet; that is, the packet will be lost. a lost packet may be *retransmitted* on an end-to-end basis in order to ensure that all data are eventually transferred from source to destination

    3. End-to-End Delay

        * Suppose there are N - 1 routers between the source host and the destination host. Let’s also suppose for the moment that the network is uncongested (so that queuing delays are negligible), the processing delay at each router and at the source host is $d_\mbox{proc}$, the transmission rate out of each router and out of the source host is R bits/sec, and the propagation on each link is $d_\mbox{prop}$. The nodal delays accumulate and give an end-to-end delay, $d_\mbox{end-end} = N (d_\mbox{proc} + d_\mbox{trans} + d_\mbox{prop})$

        Traceroute

        * RFC 1393 describes Traceroute in detail.

        * [PingPlotter](https://www.pingplotter.com/download)

        End System, Application, and Other Delays
        * WiFi or cable modem scenario may purposefully delay its transmission as part of its protocol for sharing the medium with other end systems.
        * In VoIP, the sending side must first fill a packet with encoded digitized speech before passing the packet to the Internet.

    4. Throughput in Computer Networks

        * For simple two-link network, the throughput is min{Rc , Rs}, that is, it is the transmission rate of the **bottleneck** link.
        * We saw that when there is no other intervening traffic, the throughput can simply be approximated as the minimum transmission rate along the path between source and destination.

5. ## Protocol Layers And Their Service Models

    1. Layered Architecture

        * A layered architecture allows us to discuss a well-defined, specific part of a large and complex system. This simplification itself is of considerable value by providing modularity, making it much easier to change the implementation of the service provided by the layer and do not affect others that depends on this layer.

        * For large and complex systems that are constantly being updated, the ability to change the implementation of a service without affecting other components of the system is another important advantage of layering.

        Protocol Layering

        * To provide structure to the design of network protocols, network designers organize protocols—and the network hardware and software that implement the protocols— in layers. We are again interested in the services that a layer offers to the layer above—the so-called **service model** of a layer.Just as in the case of our airline example, each layer provides its service by (1) performing certain actions within that layer and by (2) using the services of the layer directly below it.

        |Five-layer|Seven-layer|
        |---|---|
        |Application|Application|
        |--|Presentation(表示层)|
        |--|Session(会话层)|
        |TransPort|Transport|
        |Network|Network|
        |Link|Link|
        |Physical|Physical|

        [Protocol layering has conceptual and structural advantages](https://tools.ietf.org/html/rfc3439)

        Application Layer

        * The application layer is where network applications and their application-layer protocols reside. The Internet’s application layer includes many protocols, such as the HTTP protocol SMTP , and FTP and the domain name system (DNS).An application-layer protocol is distributed over multiple end systems, with the application in one end system using the protocol to exchange packets of information with the application in another end system. We’ll refer to this packet of information at the application layer as a **message**.

        Transport Layer

        * The Internet’s transport layer transports application-layer messages between application endpoints. In the Internet there are two transport protocols, TCP and UDP, either of which can transport application-layer messages. we’ll refer to a transport-layer packet as a **segment**.

        Network Layer

        * The Internet’s network layer is responsible for moving network-layer packets known as **datagrams(数据报)** from one host to another. The network layer then provides the service of delivering the segment to the transport layer in the destination host. The Internet’s network layer includes the celebrated IP Protocol, which defines the fields in the datagram as well as how the end systems and routers act on these fields. There is only one IP protocol, and all Internet components that have a network layer must run the IP protocol.The Internet’s network layer also contains routing protocols that determine the routes that datagrams take between sources and destinations.

        Link Layer

        * The Internet’s network layer routes a datagram through a series of routers between the source and destination. To move a packet from one node (host or router) to the next node in the route, the network layer relies on the services of the link layer. The services provided by the link layer depend on the specific link-layer protocol that is employed over the link. Examples of linklayer protocols include Ethernet, WiFi, and the cable access network’s DOCSIS protocol.In this book, we’ll refer to the linklayer packets as **frames(帧)**.

        Physical Layer
        * the job of the physical layer is to move the individual bits within the frame from one node to the next.

        The OSI Model
        * The role of the presentation layer is to provide services that allow communicating applications to interpret the meaning of data exchanged. These services include data compression and data encryption (which are selfexplanatory) as well as data description (which, as we will see in Chapter 9, frees the applications from having to worry about the internal format in which data are represented/stored—formats that may differ from one computer to another). The session layer provides for delimiting and synchronization of data exchange, including the means to build a checkpointing and recovery scheme.

    2. Encapsulation

6. ## Network Under Attack

7. ## History of Compuer Networking and The History

    1. The Development of Packet Switching 1961-1972
    2. Proprietary Networks and Internetworking 1972-1980
    3. A Proliferation of Networks 1980-1990
    4. The Internet Explosion The 1990s
    5. The New Millennium
8. Summary

## Homework Problems and Questions

### Chapter 1 Review Questions

SECTION 1.1

R1. What is the difference between a host and an end system? List several different types of end systems. Is a Web server an end system?

Answer: There is no difference between host and an end system. Throughtout the text both words are used interchangeably.End systems contains laptops,smartphones,tablets,TVs,home electricals.

R2. The word protocol is ofsten used to describe diplomatic relations. How does Wikipedia describe diplomatic protocol?

Answer: In international politics, protocol is the etiquette of diplomacy and affairs of state. It may also refer to an international agreement that supplements or amends a treaty. A protocol is a rule which describes how an activity should be performed, especially in the field of diplomacy. In diplomatic services and governmental fields of endeavor protocols are often unwritten guidelines.

R3. Why are standards important for protocols?

Answer: Standards are important for protocols so that people can creating networking system and products that interoperate.

SECTION 1.2

R4. List six access technologies. Classify each one as home access, enterprise access, or wide-area wireless access.

Answer:
    1. DSL(digital subscribe line) ;home access
    2. cable Internet access ;home access
    3. HFC(hybrid fiber coax) ;home access
    4. FTTH(faber to the home); home access
    5. AONs(active optical networks);home access
    6. PONs(passive optical networks);home access
    7. Ethernet;enterprise home 
    8. WiFi;enterprise home
    9. 3G(third-generation wireless);wide-area
    10. 4G(fourth-generation wireless);wide area
    11. LTE(Long Term Evolution);wide area

R5. Is HFC transmission rate dedicated or shared among users? Are collisions possible in a downstream HFC channel? Why or why not?

Answer: HFC transmission rete is shared among users.collisions are possible in a down stream HFC channel because every packet sent by the head end travels downstream on every link to every home and every packet send by a home travels on the upstream to the head end.

R6. List the available residential access technologies in your city. For each type of access, provide the advertised downstream rate, upstream rate, and monthly price.

Answer: FTTP(fiber to the home),FTTB(fiber to the building)

R7. What is the transmission rate of Ethernet LANs?

Answer: users typically have 100Mbps access ,servers my have 1Gps or 10Gps access.

R8. What are some of the physical media that Ethernet can run over?

twisted-pair copper,fibers

R9. Dial-up modems, HFC, DSL and FTTH are all used for residential access. For each of these access technologies, provide a range of transmission rates and comment on whether the transmission rate is shared or dedicated.

* Dial-up modems: up to 56kbps,bandwith is dedicated
* HFC :downstream up to 42.8 Mbps and upstream up to 30.7 Mbps,bandwith is shared
* DSL :dwonstream up to 12Mbps and upstream up to 1.8Mbps,bandwith is dedicated
* FTTH :dwonstream max 100Mbps upstream max 4Mbps ,bandwith is dedicated

R10. Describe the most popular wireless Internet access technologies today. Compare and contrast them.

There are two most popular wireless Internet access

WiFi(802.11): In a wireless LAN,users transmit/reveive packets from a router withen a radious of few ten of meters. The router typically connected to the wired Internet.

4G: wide-area wireless network,in these systems packets are transmitted over the same wireless infrastrusture used for cellular telephony. the providers wireless access to users within a redious of tens of kilometers of the base station.

SECTION 1.3

R11. Suppose there is exactly one packet switch between a sending host and a receiving host. The transmission rates between the sending host and the switch and between the switch and the receiving host are R1 and R2, respectively. Assuming that the switch uses store-and-forward packet switching, what is the total end-to-end delay to send a packet of length L? (Ignore queuing, propagation delay, and processing delay.) 

    L/R1 + L/R2

R12. What advantage does a circuit-switched network have over a packet-switched network? What advantages does TDM have over FDM in a circuit-switched network? 

circuit-switched network has guaranteed bandwidth.Time division multiplexing (TDM) has an advantage over Frequency division multiplexing(FDM) as it gives bandwidth saving and there is low interference between multiplexed signals.

R13. Suppose users share a 2 Mbps link. Also suppose each user transmits continuously at 1 Mbps when transmitting, but each user transmits only 20 percent of the time. (See the discussion of statistical multiplexing in Section 1.3.)

a. When circuit switching is used, how many users can be supported?
    2/1 = 2 two users

b. For the remainder of this problem, suppose packet switching is used. Why will there be essentially no queuing delay before the link if two or fewer users transmit at the same time? Why will there be a queuing delay if three users transmit at the same time?

c. Find the probability that a given user is transmitting.

d. Suppose now there are three users. Find the probability that at any given time, all three users are transmitting simultaneously. Find the fraction of time during which the queue grows.

R14. Why will two ISPs at the same level of the hierarchy often peer with each other? How does an IXP earn money?

R15. Some content providers have created their own networks. Describe Google’s network. What motivates content providers to create these networks?

SECTION 1.4

R16. Consider sending a packet from a source host to a destination host over a fixed route. List the delay components in the end-to-end delay. Which of these delays are constant and which are variable?

R17. Visit the Transmission Versus Propagation Delay applet at the companion Web site. Among the rates, propagation delay, and packet sizes available, find a combination for which the sender finishes transmitting before the first bit of the packet reaches the receiver. Find another combination for which the first bit of the packet reaches the receiver before the sender finishes transmitting.

R18. How long does it take a packet of length 1,000 bytes to propagate over a link of distance 2,500 km, propagation speed 2.5 · 108 m/s, and transmission rate 2 Mbps? More generally, how long does it take a packet of length L to propagate over a link of distance d, propagation speed s, and transmission rate R bps? Does this delay depend on packet length? Does this delay depend on transmission rate?

R19. Suppose Host A wants to send a large file to Host B. The path from Host A to Host B has three links, of rates R1 = 500 kbps, R2 = 2 Mbps, and R3 = 1 Mbps. a. Assuming no other traffic in the network, what is the throughput for the file transfer? b. Suppose the file is 4 million bytes. Dividing the file size by the throughput, roughly how long will it take to transfer the file to Host B? c. Repeat (a) and (b), but now with R2 reduced to 100 kbps.

R20. Suppose end system A wants to send a large file to end system B. At a very high level, describe how end system A creates packets from the file. Whenone of these packets arrives to a packet switch, what information in the packet does the switch use to determine the link onto which the packet is forwarded? Why is packet switching in the Internet analogous to driving from one city to another and asking directions along the way?

R21. Visit the Queuing and Loss applet at the companion Web site. What is the maximum emission rate and the minimum transmission rate? With those rates, what is the traffic intensity? Run the applet with these rates and determine how long it takes for packet loss to occur. Then repeat the experiment a second time and determine again how long it takes for packet loss to occur. Are the values different? Why or why not?

SECTION 1.5

R22. List five tasks that a layer can perform. Is it possible that one (or more) of these tasks could be performed by two (or more) layers?

R23. What are the five layers in the Internet protocol stack? What are the principal responsibilities of each of these layers?

R24. What is an application-layer message? A transport-layer segment? A networklayer datagram? A link-layer frame?

R25. Which layers in the Internet protocol stack does a router process? Which layers does a link-layer switch process? Which layers does a host process?SECTION 1.6

R26. What is the difference between a virus and a worm? R27. Describe how a botnet can be created, and how it can be used for a DDoS attack.

R28. Suppose Alice and Bob are sending packets to each other over a computer network. Suppose Trudy positions herself in the network so that she can capture all the packets sent by Alice and send whatever she wants to Bob; she can also capture all the packets sent by Bob and send whatever she wants to Alice. List some of the malicious things Trudy can do from this position.

Problems

P1. Design and describe an application-level protocol to be used between an automatic teller machine and a bank’s centralized computer. Your protocol should allow a user’s card and password to be verified, the account balance (which is maintained at the centralized computer) to be queried, and an account withdrawal to be made (that is, money disbursed to the user). Yourprotocol entities should be able to handle the all-too-common case in which there is not enough money in the account to cover the withdrawal. Specify your protocol by listing the messages exchanged and the action taken by the automatic teller machine or the bank’s centralized computer on transmission and receipt of messages. Sketch the operation of your protocol for the case of a simple withdrawal with no errors, using a diagram similar to that in Figure 1.2. Explicitly state the assumptions made by your protocol about the underlying end-to-end transport service.

P2. Equation 1.1 gives a formula for the end-to-end delay of sending one packet of length L over N links of transmission rate R. Generalize this formula for sending P such packets back-to-back over the N links. 

P3. Consider an application that transmits data at a steady rate (for example, the sender generates an N-bit unit of data every k time units, where k is small and fixed). Also, when such an application starts, it will continue running for a relatively long period of time. Answer the following questions, briefly justifying your answer: a. Would a packet-switched network or a circuit-switched network be more appropriate for this application? Why? b. Suppose that a packet-switched network is used and the only traffic in this network comes from such applications as described above. Furthermore, assume that the sum of the application data rates is less than the capacities of each and every link. Is some form of congestion control needed? Why?

P4. Consider the circuit-switched network in Figure 1.13. Recall that there are 4 circuits on each link. Label the four switches A, B, C and D, going in the clockwise direction. a. What is the maximum number of simultaneous connections that can be in progress at any one time in this network? b. Suppose that all connections are between switches A and C. What is the maximum number of simultaneous connections that can be in progress? c. Suppose we want to make four connections between switches A and C, and another four connections between switches B and D. Can we route these calls through the four links to accommodate all eight connections?

P5. Review the car-caravan analogy in Section 1.4. Assume a propagation speed of 100 km/hour. a. Suppose the caravan travels 150 km, beginning in front of one tollbooth, passing through a second tollbooth, and finishing just after a third tollbooth. What is the end-to-end delay? b. Repeat (a), now assuming that there are eight cars in the caravan instead of ten.

P6. This elementary problem begins to explore propagation delay and transmission delay, two central concepts in data networking. Consider two hosts, A and B, connected by a single link of rate R bps. Suppose that the two hosts are separated by m meters, and suppose the propagation speed along the link is s meters/sec. Host A is to send a packet of size L bits to Host B. a. Express the propagation delay, dprop, in terms of m and s. b. Determine the transmission time of the packet, dtrans, in terms of L and R. c. Ignoring processing and queuing delays, obtain an expression for the endto-end delay. d. Suppose Host A begins to transmit the packet at time t = 0. At time t = dtrans, where is the last bit of the packet? e. Suppose dprop is greater than dtrans. At time t = dtrans, where is the first bit of the packet? f. Suppose dprop is less than dtrans. At time t = dtrans, where is the first bit of the packet? g. Suppose s = 2.5 · 108, L = 120 bits, and R = 56 kbps. Find the distance m so that dprop equals dtrans.

P7. In this problem, we consider sending real-time voice from Host A to Host B over a packet-switched network (VoIP). Host A converts analog voice to a digital 64 kbps bit stream on the fly. Host A then groups the bits into 56-byte packets. There is one link between Hosts A and B; its transmission rate is 2 Mbps and its propagation delay is 10 msec. As soon as Host A gathers a packet, it sends it to Host B. As soon as Host B receives an entire packet, it converts the packet’s bits to an analog signal. How much time elapses from the time a bit is created (from the original analog signal at Host A) until the bit is decoded (as part of the analog signal at Host B)?

P8. Suppose users share a 3 Mbps link. Also suppose each user requires 150 kbps when transmitting, but each user transmits only 10 percent of the time. (See the discussion of packet switching versus circuit switching in Section 1.3.) a. When circuit switching is used, how many users can be supported? b. For the remainder of this problem, suppose packet switching is used. Find the probability that a given user is transmitting. c. Suppose there are 120 users. Find the probability that at any given time, exactly n users are transmitting simultaneously. (Hint: Use the binomial distribution.) d. Find the probability that there are 21 or more users transmitting simultaneously.

P9. Consider the discussion in Section 1.3 of packet switching versus circuit switching in which an example is provided with a 1 Mbps link. Users are generating data at a rate of 100 kbps when busy, but are busy generating data only with probability p = 0.1. Suppose that the 1 Mbps link is replaced by a 1 Gbps link. a. What is N, the maximum number of users that can be supported simultaneously under circuit switching? b. Now consider packet switching and a user population of M users. Give a formula (in terms of p, M, N) for the probability that more than N users are sending data.

P10. Consider a packet of length L which begins at end system A and travels over three links to a destination end system. These three links are connected by two packet switches. Let di , si , and Ri denote the length, propagation speed, and the transmission rate of link i, for i = 1, 2, 3. The packet switch delays each packet by d proc. Assuming no queuing delays, in terms of di , si , Ri , (i = 1,2,3), and L, what is the total end-to-end delay for the packet? Suppose now the packet is 1,500 bytes, the propagation speed on all three links is 2.5 · 108 m/s, the transmission rates of all three links are 2 Mbps, the packet switch processing delay is 3 msec, the length of the first link is 5,000 km, the length of the second link is 4,000 km, and the length of the last link is 1,000 km. For these values, what is the end-to-end delay?

P11. In the above problem, suppose R1 = R2 = R3 = R and dproc = 0. Further suppose the packet switch does not store-and-forward packets but instead immediately transmits each bit it receives before waiting for the entire packet to arrive. What is the end-to-end delay?

P12. A packet switch receives a packet and determines the outbound link to which the packet should be forwarded. When the packet arrives, one other packet is halfway done being transmitted on this outbound link and four other packets are waiting to be transmitted. Packets are transmitted in order of arrival. Suppose all packets are 1,500 bytes and the link rate is 2 Mbps. What is the queuing delay for the packet? More generally, what is the queuing delay when all packets have length L, the transmission rate is R, x bits of the currently-being-transmitted packet have been transmitted, and n packets are already in the queue? 

P13. (a) Suppose N packets arrive simultaneously to a link at which no packets are currently being transmitted or queued. Each packet is of length L and the link has transmission rate R. What is the average queuing delay for the N packets? (b) Now suppose that N such packets arrive to the link every LN/R seconds. What is the average queuing delay of a packet?

P14. Consider the queuing delay in a router buffer. Let I denote traffic intensity; that is, I = La/R. Suppose that the queuing delay takes the form IL/R (1 – I) for I < 1. a. Provide a formula for the total delay, that is, the queuing delay plus the transmission delay. b. Plot the total delay as a function of L/R. 

P15. Let a denote the rate of packets arriving at a link in packets/sec, and let μ denote the link’s transmission rate in packets/sec. Based on the formula for the total delay (i.e., the queuing delay plus the transmission delay) derived in the previous problem, derive a formula for the total delay in terms of a and μ.

P16. Consider a router buffer preceding an outbound link. In this problem, you will use Little’s formula, a famous formula from queuing theory. Let N denote the average number of packets in the buffer plus the packet being transmitted. Let a denote the rate of packets arriving at the link. Let d denote the average total delay (i.e., the queuing delay plus the transmission delay) experienced by a packet. Little’s formula is N = a · d. Suppose that on average, the buffer contains 10 packets, and the average packet queuing delay is 10 msec. The link’s transmission rate is 100 packets/sec. Using Little’s formula, what is the average packet arrival rate, assuming there is no packet loss?

P17. a. Generalize Equation 1.2 in Section 1.4.3 for heterogeneous processing rates, transmission rates, and propagation delays. b. Repeat (a), but now also suppose that there is an average queuing delay of dqueue at each node.

P18. Perform a Traceroute between source and destination on the same continent at three different hours of the day. a. Find the average and standard deviation of the round-trip delays at each of the three hours. b. Find the number of routers in the path at each of the three hours. Did the paths change during any of the hours? c. Try to identify the number of ISP networks that the Traceroute packets pass through from source to destination. Routers with similar names and/or similar IP addresses should be considered as part of the same ISP. In your experiments, do the largest delays occur at the peering interfaces between adjacent ISPs? d. Repeat the above for a source and destination on different continents. Compare the intra-continent and inter-continent results.

P19. (a) Visit the site www.traceroute.org and perform traceroutes from two different cities in France to the same destination host in the United States. How many links are the same in the two traceroutes? Is the transatlantic link the same?
(b) Repeat (a) but this time choose one city in France and another city in Germany. (c) Pick a city in the United States, and perform traceroutes to two hosts, each in a different city in China. How many links are common in the two traceroutes? Do the two traceroutes diverge before reaching China?

P20. Consider the throughput example corresponding to Figure 1.20(b). Now suppose that there are M client-server pairs rather than 10. Denote Rs , Rc , and R for the rates of the server links, client links, and network link. Assume all other links have abundant capacity and that there is no other traffic in the network besides the traffic generated by the M client-server pairs. Derive a general expression for throughput in terms of Rs , Rc , R, and M.

P21. Consider Figure 1.19(b). Now suppose that there are M paths between the server and the client. No two paths share any link. Path k (k = 1, . . ., M ) consists of N links with transmission rates Rk 1, Rk 2, . . ., Rk N. If the server can only use one path to send data to the client, what is the maximum throughput that the server can achieve? If the server can use all M paths to send data, what is the maximum throughput that the server can achieve?

P22. Consider Figure 1.19(b). Suppose that each link between the server and the client has a packet loss probability p, and the packet loss probabilities for these links are independent. What is the probability that a packet (sent by the server) is successfully received by the receiver? If a packet is lost in the path from the server to the client, then the server will re-transmit the packet. On average, how many times will the server re-transmit the packet in order for the client to successfully receive the packet?

P23. Consider Figure 1.19(a). Assume that we know the bottleneck link along the path from the server to the client is the first link with rate Rs bits/sec. Suppose we send a pair of packets back to back from the server to the client, and there is no other traffic on this path. Assume each packet of size L bits, and both links have the same propagation delay d prop. a. What is the packet inter-arrival time at the destination? That is, how much time elapses from when the last bit of the first packet arrives until the last bit of the second packet arrives? b. Now assume that the second link is the bottleneck link (i.e., Rc < Rs ). Is it possible that the second packet queues at the input queue of the second link? Explain. Now suppose that the server sends the second packet T seconds after sending the first packet. How large must T be to ensure no queuing before the second link? Explain.

P24. Suppose you would like to urgently deliver 40 terabytes data from Boston to Los Angeles. You have available a 100 Mbps dedicated link for data transfer. Would you prefer to transmit the data via this link or instead use FedEx overnight delivery? Explain.

P25. Suppose two hosts, A and B, are separated by 20,000 kilometers and are connected by a direct link of R = 2 Mbps. Suppose the propagation speed over the link is 2.5  108 meters/sec. a. Calculate the bandwidth-delay product, R  dprop. b. Consider sending a file of 800,000 bits from Host A to Host B. Suppose the file is sent continuously as one large message. What is the maximum number of bits that will be in the link at any given time? c. Provide an interpretation of the bandwidth-delay product. d. What is the width (in meters) of a bit in the link? Is it longer than a football field? e. Derive a general expression for the width of a bit in terms of the propagation speed s, the transmission rate R, and the length of the link m. 

P26. Referring to problem P25, suppose we can modify R. For what value of R is the width of a bit as long as the length of the link?

P27. Consider problem P25 but now with a link of R = 1 Gbps. a. Calculate the bandwidth-delay product, R  dprop. b. Consider sending a file of 800,000 bits from Host A to Host B. Suppose the file is sent continuously as one big message. What is the maximum number of bits that will be in the link at any given time? c. What is the width (in meters) of a bit in the link?

P29. Suppose there is a 10 Mbps microwave link between a geostationary satellite and its base station on Earth. Every minute the satellite takes a digital photo and sends it to the base station. Assume a propagation speed of 2.4  108 meters/sec. a. What is the propagation delay of the link? b. What is the bandwidth-delay product, R · dprop ? c. Let x denote the size of the photo. What is the minimum value of x for the microwave link to be continuously transmitting?

P30. Consider the airline travel analogy in our discussion of layering in Section 1.5, and the addition of headers to protocol data units as they flow down the protocol stack. Is there an equivalent notion of header information that is added to passengers and baggage as they move down the airline protocol stack?

P31. In modern packet-switched networks, including the Internet, the source host segments long, application-layer messages (for example, an image or a music file) into smaller packets and sends the packets into the network. The receiver then reassembles the packets back into the original message. We refer to this process as message segmentation. Figure 1.27 illustrates the end-to-end transport of a message with and without message segmentation. Consider a message that is 8 · 106 bits long that is to be sent from source to destination in Figure 1.27. Suppose each link in the figure is 2 Mbps. Ignore propagation, queuing, and processing delays. a. Consider sending the message from source to destination without message segmentation. How long does it take to move the message from the source host to the first packet switch? Keeping in mind that each switch uses store-and-forward packet switching, what is the total time to move the message from source host to destination host? b. Now suppose that the message is segmented into 800 packets, with each packet being 10,000 bits long. How long does it take to move the first packet from source host to the first switch? When the first packet is being sent from the first switch to the second switch, the second packet is being sent from the source host to the first switch. At what time will the second packet be fully received at the first switch? c. How long does it take to move the file from source host to destination host when message segmentation is used? Compare this result with your answer in part (a) and comment. d. In addition to reducing delay, what are reasons to use message segmentation? e. Discuss the drawbacks of message segmentation.

P32. Experiment with the Message Segmentation applet at the book’s Web site. Do the delays in the applet correspond to the delays in the previous problem? How do link propagation delays affect the overall end-to-end delay for packet switching (with message segmentation) and for message switching? 

P33. Consider sending a large file of F bits from Host A to Host B. There are three links (and two switches) between A and B, and the links are uncongested (that is, no queuing delays). Host A segments the file into segments of S bits each and adds 80 bits of header to each segment, forming packets of L = 80 + S bits. Each link has a transmission rate of R bps. Find the value of S that minimizes the delay of moving the file from Host A to Host B. Disregard propagation delay.

P34. Skype offers a service that allows you to make a phone call from a PC to an ordinary phone. This means that the voice call must pass through both the Internet and through a telephone network. Discuss how this might be done.

Wireshark Lab

Interview: Leonard Kleinrock
