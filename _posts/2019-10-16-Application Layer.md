---
title: Application Layer
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

        * the network through a software interface called a **socket**.

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

        * These observations lead to another natural service that a **transport-layer** protocol could provide, namely, guaranteed available throughput at some specified rate.

        * Applications that have throughput requirements are said to be bandwidth-sensitive applications.

        * Elastic applications can make use of as much, or as little, throughput as happens to be available.

        Timing

        * A transport-layer protocol can also provide timing guarantees.

        Security

        * a transport protocol can provide an application with one or more security services.

    4. Transport Services Provided by the Internet

        The Internet (and, more generally, TCP/IP networks) makes two transport protocols available to applications, UDP and TCP.

        TCP Services

            The TCP service model includes a connection-oriented service and a reliable data transfer service. When an application invokes TCP as its transport protocol, the application receives both of these services from TCP.

            * Connection-oriented service. TCP has the client and server exchange transportlayer control information with each other before the application-level messages begin to flow. This so-called handshaking procedure alerts the client and server, allowing them to prepare for an onslaught of packets. After the handshaking phase, a TCP connection is said to exist between the sockets of the two processes.
            
            * Reliable data transfer service. The communicating processes can rely on TCP to deliver all data sent without error and in the proper order.

            TCP also includes a congestion-control mechanism, a service for the general welfare of the Internet rather than for the direct benefit of the communicating processes.

        UDP Services

        * UDP is a no-frills, lightweight transport protocol, providing minimal services. UDP is connectionless, so there is no handshaking before the two processes start to communicate. UDP provides an unreliable data transfer service—that is, when a process sends a message into a UDP socket, UDP provides no guarantee that the message will ever reach the receiving process. Furthermore, messages that do arrive at the receiving process may arrive out of order.

        Services Not Provided by Internet Transport Protocols

            * Timing and thoughtput are not provided by Internet Transport Protocols

    5. Application-layer Protocols

        An application-layer protocol defines how an application’s processes, running on different end systems, pass messages to each other.

        Application-layer defines:

        * The types of messages exchanged, for example, request messages and response messages.

        * The syntax of the various message types, such as the fields in the message and how the fields are delineated

        * The semantics of the fields, that is, the meaning of the information in the fields

        * Rules for determining when and how a process sends messages and responds to messages

    6. Network Applications Covered in This Book

        In this chapter we discuss five important applications: the Web, file transfer, electronic mail, directory service, and P2P applications.

2. The Web and HTTP

3. File Transfer:FTP
4. Electronic Mail in the internet
5. DNS-The internet's Directory Service
6. Peer-to-Peer Applications
7. Socket Programing:Creating Network Applications
8. Summary

## Homework Problems and Questions

    SECTION 2.1 
    
    R1. List five nonproprietary Internet applications and the application-layer protocols that they use. 

        QQ OICQ.
        Wechat http.
        Foxmail smtp.
        Chrome HTTP.
        Xshell SSH/FTP.

    R2. What is the difference between network architecture and application architecture? 

        network architecture are pre defined and well knowned and only one architecture. but application architure are different from other applications,developer can choose any architecture they like.
    
    R3. For a communication session between a pair of processes, which process is the client and which is the server? 
        
        the process that initiates the communication is label as client,The process that waits to be contacted to begin the session is the server.

    R4. For a P2P file-sharing application, do you agree with the statement, “There is no notion of client and server sides of a communication session”? Why or why not?
        I agree, because any computer can initiates a communication, themselves also wait for connection.
    
    R5. What information is used by a process running on one host to identify a process running on another host? 

        to identify a host use ip address,to identify a process use port number.

    R6. Suppose you wanted to do a transaction from a remote client to a server as fast as possible. Would you use UDP or TCP? Why?

        TCP, because tcp provides a reliable data transfer service.

Socket Programing Assignments

Wireshark Labs: HTTP,DNS.

Interview: Marc Andreessen
