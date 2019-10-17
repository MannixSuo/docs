---
title: Computer Networking A Top Down Approach
layout: posts
tags: computer_network
use_math: true
---

1. Principles of Network Applications

        network applications are based on application layer.

    1. Network Application Architectures

        From the application developer’s perspective, the network architecture is fixed and provides a specific set of services to applications. The application architecture, on the other hand, is designed by the application developer and dictates how the application is structured over the various end systems.

        client-server architecture and peer-to-peer architecture

        three challenges for peer-to-peer applications

            1. ISP friendly, because asymmetrical bandwidth usage.
            2. Security
            3. Incentive, how to convence user to volunteer bandwidth,stroge,and computation resources to application.

    2. Processes Communicating

        * In the jargon of operating systems, it is not actually programs but processes that communicate.

        * Processes on two different end systems communicate with each other by exchanging messages across the computer network.

        Client and Server Processes
        * In the context of a communication session between a pair of processes, the process that initiates the communication (that is, initially contacts the other process at the beginning of the session) is labeled as the client. The process that waits to be contacted to begin the session is the server.

        The Interface Between the Process and the Computer Network

        * the network through a software interface called a socket.

        Addressing Processes

        * one **ip** address refer to one host, one **port number** refer to one process.
        * Popular applications have been assigned specific port numbers.
            a Web server is identified by port number 80.
            A mail server process (using the SMTP protocol) is identified by port number 25.
            [more](http://www.iana.org) [detail](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)

    3. Transport Services Available to Applications
        Reliable Data Transfer
        * Thus, to support these applications, something has to be done to guarantee that the data sent by one end of the application is delivered correctly and completely to the other end of the application. If a protocol provides such a guaranteed data delivery service, it is said to provide reliable data transfer.

        * One important service that a **transport-layer** protocol can potentially provide to an application is process-to-process reliable data transfer.

        * When a transport-layer protocol doesn’t provide reliable data transfer, some of the data sent by the sending process may never arrive at the receiving process. This may be acceptable for loss-tolerant applications, most notably multimedia applications such as conversational audio/video that can tolerate some amount of data loss.
        Throughput

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
