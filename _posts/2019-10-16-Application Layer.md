---
title: Application Layer
layout: posts
tags: computer_network
use_math: false
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

        * With presistent connections the server leaves the TCP connection open after sending a response.

    3. HTTP Message Format
        * The HTTP specifications [RFC 1945;RFC 2616] include the definitions of the HTTP message formats. There are two types of HTTP messages,request messages and response messages.

        HTTP request Message

        * Message written in ASCII text.
        * Eachline is followed by an carriage return(回车) and line feed(换行).
        * The last line is followed by an additional carriage return and line feed.
        * The first line of an HTTP request message is called the request line.
        * The subsequent lines are called the header lines.
        * The request line has three fields : the method field,the URL field,and the HTTP version field.

            method \|sp\| URL \|sp\| Version \|cr\|lf\|

            header field name :\|sp\| value \|cr\|\|lf\|

            \|cr|\|lf\|

            Entity Body

        HTTP Response Message

        * Status code and their phrases:
        * 200 OK: Request succeeded and the information is returned in the response.
        * 301 Moved Permanently: Requested object has been permanently moved;the new URL is specified in Location: header of the response message.
        * 400 Bad Request: This is a generic error code indicating that the request could not be understood by the server.
        * 404 Not Found: The request document does not exist on this server.
        * 505 Http Version Not Supported: The requested HTTP protocol version is not supported by the server.

            Version\|sp\|status code\|sp\|phrase\|cr\|lf\|

            header field name:\|sp\|value\|cr\|\|lf\|

            ...

            \|cr\|\|lf\|

            body

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

    1. Services Provided by DNS

        Translate hostname to Ip address

        The DNS is
            1. a distributed database implemented in a hierarchy of DNS servers
            2. an application-layer protocol that allows hosts to query the distributed database.

        The DNS servers are often UNIX machines running the Berkeley Internet Name Domain (BIND) software [BIND 2012]. The DNS protocol runs over **UDP** and uses port **53**.

        DNS provides a few other important services in addtion to translating hostnames to IP addresses:

        * Host aliasing. A host with a complicated hostname can have one or more alias names.For example, a hostname such as relay1.west-coast.enterprise.com could have, say, two aliases such as enterprise.com and www.enterprise.com. In this case, the hostname relay1.westcoast.enterprise.com is said to be a canonical hostname.

        * Mail server aliasing.

        * Load distribution.

    2. Overview of How DNS Works

        A simple design for DNS would have one DNS server that contains all the mappings.The problem with a centralized design include:

        * A single point of failure. If the DNS server crashes,so does the entire Internet.
        * Traffic volume. A single DNS server would have to handle all DNS queries.
        * Distant centralized database. A single DNS server cannot be “close to” all the querying clients. This can lead to significant delays.
        * Maintenance. The single DNS server would have to keep records for all Internet hosts. Not only would this centralized database be huge, but it would have to be updated frequently to account for every new host.

        A Distributed,Hierarchical Database

        No single DNS server has all of the mappings for all of the hosts in the Internet. Instead, the mappings are distributed across the DNS servers.

        To a first approximation, there are three classes of DNS servers—root DNS servers, top-level domain (TLD) DNS servers, and authoritative DNS servers.

        * Root DNS servers. In the Internet there are 13 root DNS servers (labeled A through M), most of which are located in North America.
        * Top-level domain (TLD) servers. These servers are responsible for top-level domains such as com, org, net, edu, and gov, and all of the country top-level domains such as uk, fr, ca, and jp. The company Verisign Global Registry Services maintains the TLD servers for the com top-level domain, and the company Educause maintains the TLD servers for the edu top-level domain.
        * Authoritative DNS servers. Every organization with publicly accessible hosts (such as Web servers and mail servers) on the Internet must provide publicly accessible DNS records that map the names of those hosts to IP addresses. An organization’s authoritative DNS server houses these DNS records. An organization canchoose to implement its own authoritative DNS server to hold these records; alternatively, the organization can pay to have these records stored in an authoritative DNS server of some service provider. Most universities and large companies implement and maintain their own primary and secondary (backup) authoritative DNS server.

        DNS Caching

        * DNS extensively exploits DNS caching in order to improve the delay performance and to reduce the number of DNS messages ricocheting around the Internet.

        * A local DNS server can also cache the IP addresses of TLD servers, thereby allowing the local DNS server to bypass the root DNS servers in a query chain (this often happens).

    3. DNS Records and Messages

        The DNS servers that together implement the DNS distributed database store resource records (RRs), including RRs that provide hostname-to-IP address mappings.

        more details can be found in [Abitz 1993] or in the DNS RFCs [RFC 1034; RFC 1035].

        A resource record is a four-tuple that contains the following fields:

            (Name, Value, Type, TTL)

        TTL is the time to live of the resource record; it determines when a resource should be removed from a cache.

        The meaning of Name and Value depend on Type:

        * If Type=A ,then Name is a hostname and Value is the IP address for the hostname. Thus, a Type A record provides the standard hostname-to-IP address mapping. As an example, (relay1.bar.foo.com, 145.37.93.126, A) is a Type A record.

        * If Type=NS then Name is a domain (such as foo.com) and Value is the hostname of an authoritative DNS server that knows how to obtain the IP addresses for hosts in the domain. This record is used to route DNS queries further along in the query chain. As an example, (foo.com, dns.foo.com, NS) is a Type NS record.

        * If Type=CNAME,then Value is a canonical hostname for the alias hostname Name. This record can provide querying hosts the canonical name for a hostname. As an example, (foo.com, relay1.bar.foo.com, CNAME) is a CNAME record.

        * If Type=MX, then Value is the canonical name of a mail server that has an alias hostname Name. As an example, (foo.com, mail.bar.foo.com, MX) is an MX record. MX records allow the hostnames of mail servers to have simple aliases. Note that by using the MX record, a company can have the same aliased name for its mail server and for one of its other servers (such as its Web server). To obtain the canonical name for the mail server, a DNS client would query for an MX record; to obtain the canonical name for the other server, the DNS client would query for the CNAME record.

        If a DNS server is authoritative for a particular hostname, then the DNS server will contain a Type A record for the hostname. (Even if the DNS server is not authoritative, it may contain a Type A record in its cache.) If a server is not authoritative for a hostname, then the server will contain a Type NS record for the domain that includes the hostname; it will also contain a Type A record that provides the IP address of the DNS server in the Value field of the NS record.

        DNS Message

        The first 12 bytes is the header section, which has a number of fields. The first field is a 16-bit number that identifies the query. This identifier is copied into the reply message to a query, allowing the client to match received replies with sent queries. There are a number of flags in the flag field. A 1-bit query/reply flag indicates whether the message is a query (0) or a reply (1). A 1-bit authoritative flag is set in a reply message when a DNS server is an authoritative server for a queried name. A 1-bit recursion-desired flag is set when a client (host or DNS server) desires that the DNS server perform recursion when it doesn’t have the record. A 1-bit recursionavailable field is set in a reply if the DNS server supports recursion.
        In the header,there are also four number-of fields. These fields indicate the number of occurrences of the four types of data sections that follow the header.

        * The question section contains information about the query that is being made. This section includes (1) a name field that contains the name that is being queried, and (2) a type field that indicates the type of question being asked about the name—for example, a host address associated with a name (Type A) or the mail server for a name (Type MX).

        * In a reply from a DNS server, the answer section contains the resource records for the name that was originally queried. Recall that in each resource record there is the Type (for example, A, NS, CNAME, and MX), the Value, and the TTL. A reply can return multiple RRs in the answer, since a hostname can have multiple IP addresses (for example, for replicated Web servers, as discussed earlier in this section).

        * The authority section contains records of other authoritative servers.

        * The additional section contains other helpful records. For example, the answer field in a reply to an MX query contains a resource record providing the canonical hostname of a mail server. The additional section contains a Type A record providing the IP address for the canonical hostname of the mail server.

        Inserting Records into the DNS Database

6. Peer-to-Peer Applications

    1. P2P File Distribution

        In P2P file distribution, each peer can redistribute any portion of the file it has received to any other peers, thereby assisting the server in the distribution process.

        Scalability of P2P Architectures

        In the client-server architecture, none of the peers aids in distributing the file. We make the following observations:

        * The server must transmit one copy of the file to each of the N peers. Thus the server must transmit NF bits. Since the server’s upload rate is us , the time to distribute the file must be at least**NF/us**

        * Let dmin denote the download rate of the peer with the lowest download rate, that is, dmin = min{d1,dp,...,dN}. The peer with the lowest download rate cannot obtain all F bits of the file in less than F/dmin seconds. Thus the minimum distribution time is at least **F/dmin**.

        So distribution time Dcs = max{NF/us,F/dmin}

        In P2P architectures each peer can assist the server in distributing the file.In particular, when a peer receives some file data, it can use its own upload capacity to redistribute the data to other peers.

        * At the beginning of the distribution, only the server has the file. To get this file into the community of peers, the server must send each bit of the file at least once into its access link. Thus, the minimum distribution time is at least F/us . (Unlike the client-server scheme, a bit sent once by the server may not have to be sent by the server again, as the peers may redistribute the bit among themselves.)

        * As with the client-server architecture, the peer with the lowest download rate cannot obtain all F bits of the file in less than F/dmin seconds. Thus the minimum distribution time is at least F/dmin.

        * Finally, observe that the total upload capacity of the system as a whole is equal to the upload rate of the server plus the upload rates of each of the individual peers, that is, utotal = us + u1 + … + uN. The system must deliver (upload) F bits to each of the N peers, thus delivering a total of NF bits. This cannot be done at a rate faster than utotal. Thus, the minimum distribution time is also at least NF/(us + u1 + … + uN).

        Putting these three observations together, we obtain the minimum distribution time for P2P, denoted by DP2P.

        DP2P = max{F/us,F/dmin,NF/(us + u1 + … + uN)}

        BitTorrent

        BitTorrent is a popular P2P protocol for file distribution,In BitTorrent lingo, the collection of all peers participating in the distribution of a particular file is called a torrent. Peers in a torrent download equal-size chunks of the file from one another, with a typical chunk size of 256 KBytes. When a peer first joins a torrent, it has no chunks. Over time it accumulates more and more chunks. While it downloads chunks it also uploads chunks to other peers. Once a peer has acquired the entire file, it may (selfishly) leave the torrent, or (altruistically) remain in the torrent and continue to upload chunks to other peers. Also, any peer may leave the torrent at any time with only a subset of chunks, and later rejoin the torrent.

    2. Distributed Hash Tables(DHTs)

        In this section, we will consider how to implement a simple database in a P2P network. Let’s begin by describing a centralized version of this simple database, which will simply contain (key, value) pairs. For example, the keys could be social security numbers and the values could be the corresponding human names; in this case, an example key-value pair is (156-45-7081, Johnny Wu). Or the keys could be content names (e.g., names of movies, albums, and software), and the value could be the IP address at which the content is stored;in this case, an example key-value pair is (Led Zeppelin IV, 128.17.123.38).

        Building such a database is straightforward with a client-server architecture that stores all the (key, value) pairs in one central server. So in this section, we’ll instead consider how to build a distributed, P2P version of this database that will store the (key, value) pairs over millions of peers. In the P2P system, each peer will only hold a small subset of the totality of the (key, value) pairs. We’ll allow any peer to query the distributed database with a particular key. The distributed database will then locate the peers that have the corresponding (key, value) pairs and return the key-value pairs to the querying peer. Any peer will also be allowed to insert new key-value pairs into the database. Such a distributed database is referred to as a distributed hash table (DHT).

        Circular DHT

        Each peer is only aware of its immediate successor and predecessor; for example, peer 5 knows the IP address and identifier for peers 8 and 4 but does not necessarily know anything about any other peers that may be in the DHT. This circular arrangement of the peers is a special case of an overlay network.

        Thus, in designing a DHT, there is tradeoff between the number of neighbors each peer has to track and the number of messages that the DHT needs to send to resolve a single query. On one hand, if each peer tracks all other peers (mesh overlay), then only one message is sent per query, but each peer has to keep track of N peers. On the other hand, with a circular DHT, each peer is only aware of two peers, but N/2 messages are sent on average for each query. Fortunately, we can refine our designs of DHTs so that the number of neighbors per peer as well as the number of messages per query is kept to an acceptable size. One such refinement is to use the circular overlay as a foundation, but add “shortcuts” so that each peer not only keeps track of its immediate successor and predecessor, but also of a relatively small number of shortcut peers scattered about the circle.

        Peer Churn

        In P2P systems, a peer can come or go without warning.To handle peer churn, we will now require each peer to track (that is, know the IP address of) its first and second successors; for example, peer 4 now tracks both peer 5 and peer 8. We also require each peer to periodically verify that its two successors are alive (for example, by periodically sending ping messages to them and asking for responses). Let’s now consider how the DHT is maintained when a peer abruptly leaves. For example, suppose peer 5 in Figure 2.27(a) abruptly leaves. In this case, the two peers preceding the departed peer (4 and 3) learn that 5 has departed, since it no longer responds to ping messages. Peers 4 and 3 thus need to update their successor state information. Let’s consider how peer 4 updates its state:

        1. Peer 4 replaces its first successor (peer 5) with its second successor (peer 8).
        2. Peer 4 then asks its new first successor (peer 8) for the identifier and IP address of its immediate successor (peer 10). Peer 4 then makes peer 10 its second successor.

        DHTs have been finding widespread use in practice. For example, BitTorrent uses the Kademlia DHT to create a distributed tracker. In the BitTorrent, the key is the torrent identifier and the value is the IP addresses of all the peers currently participating in the torrent [Falkner 2007, Neglia 2007]. In this manner, by querying the DHT with a torrent identifier, a newly arriving BitTorrent peer can determine the peer that is responsible for the identifier (that is, for tracking the peers in the torrent). After having found that peer, the arriving peer can query it for a list of other peers in the torrent.

7. Socket Programing:Creating Network Applications

8. Summary

    In this chapter, we’ve studied the conceptual and the implementation aspects of network applications. We’ve learned about the ubiquitous client-server architecture adopted by many Internet applications and seen its use in the HTTP, FTP, SMTP, POP3, and DNS protocols. We’ve studied these important application-level protocols, and their corresponding associated applications (the Web, file transfer, e-mail, and DNS) in some detail. We’ve also learned about the increasingly prevalent P2P architecture and how it is used in many applications. We’ve examined how the socket API can be used to build network applications. We’ve walked through the use of sockets for connection-oriented (TCP) and connectionless (UDP) end-to-end transport services. The first step in our journey down the layered network architecture is now complete!

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

R7. Referring to Figure 2.4, we see that none of the applications listed in Figure 2.4 requires both no data loss and timing. Can you conceive of an application that requires no data loss and that is also highly time-sensitive?

Phone text message, Bank transfer application.

R8. List the four broad classes of services that a transport protocol can provide. For each of the service classes, indicate if either UDP or TCP (or both) provides such a service.

|services|description|UDP |TCP  |
|:---:|:----:|:---:|:---:|
|reliable data transfer|A guarantee that no data loss|n|y|
|throughput            |A guarantee that a certain value for throughput will be maintained|n|n|
|timing                |A guarantee that data will be delivered within a specified amount of time|n|n|
|security              |security|n|n|

R9. Recall that TCP can be enhanced with SSL to provide process-to-process security services, including encryption. Does SSL operate at the transport layer or the application layer? If the application developer wants TCP to be enhanced with SSL, what does the developer have to do?

SSL operate at the application layer.The SSL socket takes unencrypted data fromthe application layer, encrypts it and then passes it to the TCP socket. If theapplication developer wants TCP to be enhanced with SSL, she has to include the SSL code in the application.

SECTIONS 2.2–2.5

R10. What is meant by a handshaking protocol?

before client and server exchange information with each other they should know status of eachother by handshaking.

R11. Why do HTTP, FTP, SMTP, and POP3 run on top of TCP rather than on UDP?

because tcp privider a reliable data transfer

R12. Consider an e-commerce site that wants to keep a purchase record for each of its customers. Describe how this can be done with cookies.

when the consumer first visit the site,it give a cookie number to customers and the cookie number is stored in customers browser. During each visit or purchase the browser send this information and cookie number back to the site. So the site can keep the purchase record for it's customers.

R13. Describe how Web caching can reduce the delay in receiving a requested object. Will Web caching reduce the delay for all objects requested by a user or for only some of the objects? Why?

because the throughput of our host to the caching server is more large than our host to the remote host.When our host request a object from caching server the caching server will request to the remote address and store the object. so next time the request to the caching server for the same object the caching server will ask the remote server wether this object is modified if not modified return it's caching. web caching reduce the delay for all requested objects.

R14. Telnet into a Web server and send a multiline request message. Include in the request message the If-modified-since: header line to force a response message with the 304 Not Modified status code.

R15. Why is it said that FTP sends control information “out-of-band”?

because FTP use two parallel tcp connection,one connection for send command one connection for data transfer.

R16. Suppose Alice, with a Web-based e-mail account (such as Hotmail or gmail), sends a message to Bob, who accesses his mail from his mail server using POP3. Discuss how the message gets from Alice’s host to Bob’s host. Be sure to list the series of application-layer protocols that are used to move the message between the two hosts.

first Alice write a mail address to Bob and then send this mail to her server use http,the her mail server send the mail to Bob's mail server using SMTP,when Bob open his mail client ,the client get mail from mail server using POP3.

R17. Print out the header of an e-mail message you have recently received. How many Received: header lines are there? Analyze each of the header lines in the message.

```
Received: from x.x.com (unknown [x.x.x.x])
	by udms (Coremail) with SMTP id xxxx+AA--.x;
	Thu, 28 Nov 2019 12:25:44 +0800 (CST)
From: x@x.com
To: x@x.com
Cc: x@x.com
Message-ID: <344956422.51.1574915445464.JavaMail.root@x>
Subject: x
MIME-Version: 1.0
Content-Type: multipart/mixed;
```

R18. From a user’s perspective, what is the difference between the download-anddelete mode and the download-and-keep mode in POP3?

With download and delete, after a user retrieves its messages from a POP server,the messages are deleted. This poses a problem for the nomadic user, who may want to access the messages from many different machines (office PC, home PC,etc.). In the download and keep configuration, messages are not deleted after the user retrieves the messages. This can also be inconvenient,as each time the user retrieves the stored messages from a new machine, all of non-deleted messages will be transferred to the new machine (including very old messages).

Socket Programing Assignments

Wireshark Labs: HTTP,DNS.

Interview: Marc Andreessen
