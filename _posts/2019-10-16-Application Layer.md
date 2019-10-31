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

    1. Overview of HTTP

        * The HyperText Transfer Protocol (HTTP), the Web’s application-layer protocol, is at the heart of the Web.

        * HTTP use TCP as its underlying transport protocol.

        * Because an HTTP server maintains no information about the clients, HTTP is said to be a **statless protocol**

    2. Non-Persistent and Persistent Connections

        * If each request/response be sent over a separate TCP connection its non-persistent connections.

        * If each request/response be sent over same TCP connection its persistent connection.

        HTTP with Non-Persistent Connections

        * client will automatically request url in a html file. such as images.

        * The **round-trip time (RTT)** is the time it takes for a small packet to travel from client to server and then back to the client.

        * Three way handshake--the client send a small TCP segment to server,the server acknowledges(ack) and response with a small TCP segment,and finally ,the client acknowledges back to the server.The first two part of three way handshake take one RTT.

        Http with Persistent Connections

    3. HTTP Message Format

        The HTTP specifications [RFC 1945;RFC 2616] includes the definitions of the HTTP message formats.

        HTTP request message

            GET /somedir/page.html HTTP/1.1
            Host: www.someschool.edu
            Connection: close User-agent: Mozilla/5.0
            Accept-language: fr

        * First message is written in ASCII text.

    4. User-Server Interaction: Cookies

        Cookies defined in [RFC 6265],allow sites to keep track of users.

        Cookie technology has four componments:

            (1) a cookie header line in the HTTP response message;
            (2) a cookie header line in the HTTP request message;
            (3) a cookie file kept on the user’s end system and managed by the user’s browser;
            (4) a back-end database at the Web site.

    5. Web Caching

    6. The Conditional Get

        Use http header `If-modified-since: Wed, 7 Sep 2011 09:23:24`

3. File Transfer: FTP

    Http and FTP are both file transfer protocols and have many common characteristics.

    The most striking difference is that FTP use **two** parallel TCP connections to transfer a file, a `control connection` and a `data connection`.
    The control connection is used for send controll information between two hosts, information such as identification,password,commands to change remote dictionary. The data connection is used to transfer file.FTP is said to send its control information **out-of-band**.

    Http send requeset and response header lines into the same TCP connection that carries the transferred file itself.

    FTP will create new TCP connection for each file.

    FTP must keep tracke of the user's current dictionary. HTTP is statless - it does not have to keep track of any user state.

    1. FTP commands and Replines.

        * The commands, from client to server, and replies, from server to client, are sent across the control connection in 7-bit ASCII format.
        * command. Each command consists of four uppercase ASCII characters, some with optional arguments.

        * USER username: Used to send the user identification to the server.
        * PASS password: Used to send the user password to the server.
        * LIST: Used to ask the server to send back a list of all the files in the current remote directory. The list of files is sent over a (new and non-persistent) data connection rather than the control TCP connection.
        * RETR filename: Used to retrieve (that is, get) a file from the current directory of the remote host. This command causes the remote host to initiate a data connection and to send the requested file over the data connection.
        * STOR filename: Used to store (that is, put) a file into the current directory of the remote host.

        Each FTP command is followed by a reply,send from server to client.

        * 331 Username OK, password required
        * 125 Data connection already open; transfer starting
        * 425 Can’t open data connection
        * 452 Error writing file

        Readers who are interested in learning about the other FTP commands and replies are encouraged to read `RFC 959`.

4. Electronic Mail in the internet

    * Modern e-mail has many powerful features, including messages with attachments, hyperlinks, HTML-formatted text, and embedded photos.

    Simple Mail Transfer Protocol (SMTP)

    * SMTP is the principal application-layer protocol for Internet electronic mail. It uses the reliable data transfer service of TCP to transfer mail from the sender’s mail server to the recipient’s mail server. As with most application-layer protocols, SMTP has two sides: a client side, which executes on the sender’s mail server, and a server side, which executes on the recipient’s mail server. Both the client and server sides of SMTP run on every mail server. When a mail server sends mail to other mail servers, it acts as an SMTP client. When a mail server receives mail from other mail servers, it acts as an SMTP server.

    1. SMTP

        * SMTP, defined in `RFC 5321`, is at the heart of Internet electronic mail.
        * TCP port 25.
        * SMTP clients and servers introduce themselves before transferring information.
        * Let’s next take a look at an example transcript of messages exchanged between an SMTP client (C) and an SMTP server (S).The following transcript begins as soon as the TCP connection is established.

                S: 220 hamburger.edu
                C: HELO crepes.fr
                S: 250 Hello crepes.fr, pleased to meet you
                C: MAIL FROM: <alice@crepes.fr>
                S: 250 alice@crepes.fr ... Sender ok
                C: RCPT TO: <bob@hamburger.edu>
                S: 250 bob@hamburger.edu ... Recipient ok
                C: DATA
                S: 354 Enter mail, end with “.” on a line by itself
                C: Do you like ketchup?
                C: How about pickles?
                C: .
                S: 250 Message accepted for delivery
                C: QUIT
                S: 221 hamburger.edu closing connection

        * SMTP uses persistent connections: If the sending mail server has several messages to send to the same receiving mail server, it can send all of the messages over the same TCP connection.

    2. Comparsion with HTTP

        Common:

        * Both protocols are used to transfer files from one host to another:HTTP transfers files (also called objects) from a Web server to a Web client (typically a browser); SMTP transfers files (that is, e-mail messages) from one mail server to another mail server.
        * When transferring the files, both persistent HTTP and SMTP use persistent connections.

        Difference:

        * HTTP is mainly a `pull protocol`—someone loads information on a Web server and users use HTTP to pull the information from the server at their convenience.
        * SMTP is primarily a `push protocol`—the sending mail server pushes the file to the receiving mail server. In particular, the TCP connection is initiated by the machine that wants to send the file.
        * SMTP requires each message, including the body of each message, to be in 7-bit ASCII format. If the message contains characters that are not 7-bit ASCII (for example, French characters with accents) or contains binary data (such as an image file), then the message has to be encoded into 7-bit ASCII. HTTP data does not impose this restriction.
        * HTTP encapsulates each object in its own HTTP response message. Internet mail places all of the message’s objects into one message.

    3. Mail Message Formats

        This peripheral information is contained in a series of header lines, which are defined in RFC 5322.

    4. Mail Access Protocols

        * There are currently a number of popular mail access protocols, including Post Office Protocol—Version 3 (POP3), Internet Mail Access Protocol (IMAP), and HTTP.

        POP3

        * It is defined in [RFC 1939], which is short and quite readable.
        * use port 110.

        IMAP

        * POP3 does not provide any means for a user to create remote folders and assign messages to folders.
        * defined in [RFC 3501]
        * An IMAP server will associate each message with a folder; when a message first arrives at the server, it is associated with the recipient’s INBOX folder.
        * The IMAP protocol provides commands to allow users to create folders and move messages from one folder to another. IMAP also provides commands that allow users to search remote folders for messages matching specific criteria.
        * Another important feature of IMAP is that it has commands that permit a user agent to obtain components of messages.

        Web-Based Email

        * With this service, the user agent is an ordinary Web browser, and the user communicates with its remote mailbox via HTTP. When a recipient, such as Bob, wants to access a message in his mailbox, the e-mail message is sent from Bob’s mail server to Bob’s browser using the HTTP protocol rather than the POP3 or IMAP protocol. When a sender, such as Alice, wants to send an e-mail message, the e-mail message is sent from her browser to her mail server over HTTP rather than over SMTP. Alice’s mail server, however, still sends messages to, and receives messages from, other mail servers using SMTP.

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

    SECTIONS 2.2–2.5

Socket Programing Assignments

Wireshark Labs: HTTP,DNS.

Interview: Marc Andreessen
