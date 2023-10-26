>In the Quadrature Amplitude Modulation (QAM) encoding technique, the carrier signal's ( ) and phase are changed.

-> **amplitude**

>The TCP protocol uses a three-way handshake to establish a connection. Assuming that the initial sequence numbers sent by both sides are X and Y respectively, Party A sends a frame with SYN=1, Seq=X to Party B. After Party B receives the frame, it sends () to Party A. After Party A sends an acknowledgment frame to Party B, a connection is established.

-> SYN=1, Seq=Y, AN=X+1

>The network layer of the Internet contains four important protocols, which are ( ).

->
1. IP (Internet Protocol)
2. ICMP (Internet Control Message Protocol)
3. IGMP (Internet Group Management Protocol)
4. ARP (Address Resolution Protocol)

> Among the following addresses, which has the same network prefix as 86.32/12? 
> A 86.38.224.123 
> B 86.48.65.216 
> C 86.68.119.75 
> D 86.78.206.153

-> The network number 86.32/12 has a total of 12 bits. The first byte has 8 bits, and the second byte occupies 4 bits. Looking only at the second byte, it's 00100000. We only need to ensure that the first 4 bits are 0010, and the last four bits can be any value. The maximum value of the last four bits is 15, so 15 + 32 = 47. Therefore, the range of addresses with the same network prefix as 86.32/12 is from 86.32/12 to 86.47/12. A is the answer.

> There is a complete P2P sharing protocol. After two nodes communicate, they can obtain all the information the other has acquired. Now, to ensure every node in the system knows the file information of all nodes, there are a total of 17 nodes. Assuming that communication can only occur between two peer nodes at a time, the minimum number of communications needed is ( ).

-> For odd number of nodes, it's 2n-3. For even number of nodes, it's 2n-4. The minimum is 2n-4.

>The IP address 202.100.80.110 is a ( ) address.

-> a **Class C** address
- Class A: 0.0.0.0 to 126.0.0.0
- Class B: 128.0.0.0 to 191.255.255.255
- Class C: 192.0.0.0 to 223.255.255.255
- Class D (multicast): 224.0.0.0 to 239.255.255.255
- Class E (experimental): 240.0.0.0 to 255.255.255.255

>Path selection functionality is completed at the OSI model's ( ).

-> **Network Layer**


>In Linux programming, which of the following TCP socket options is related to the enabling and disabling of the Nagle algorithm? 
>A TCP_MAXSEG 
>B TCP_NODELAY 
>C TCP_SYNCNT 
>D TCP_KEEPALIVE

-> TCP_NODELAY

Setting TCP_NODELAY to 1 disables the Nagle algorithm, and setting it to 0 enables the Nagle algorithm.

> The physical topology of Ethernet is (), and its logical topology is ().

-> The physical topology of Ethernet is **bus** (for traditional 10BASE5 and 10BASE2 Ethernet) or **star** (for modern Ethernet implementations using switches and hubs), and its logical topology is **bus**.

> The type of service provided by the IP protocol is:
> A Connection-oriented datagram service 
> B Connectionless datagram service 
> C Connection-oriented virtual circuit service 
> D Connectionless virtual circuit service

-> **Connectionless datagram service**.

> Common email software like Outlook uses the SMTP protocol for sending and receiving emails. ( ) 
> A True 
> B False

-> B False

Outlook and other common email software use the SMTP (Simple Mail Transfer Protocol) protocol for **sending** emails. However, for **receiving** emails, they use protocols like POP3 (Post Office Protocol 3) or IMAP (Internet Message Access Protocol).

> For the IPV6 address, the correct statements are ( ) 
> A To facilitate the transition from IPV4 to IPV6, IPV6 has defined the following compatible addresses: IPV4-compatible address, IPV4-mapped address, and 6to4 address. 
> B IPV6 address types can be divided into unicast addresses, multicast addresses, and broadcast addresses. 
> C FB80:0:0:0:2AA:FF:FE9A:4CAE can be abbreviated as FE80::2AA:FF:FE9A:4CAE. 
> D For packets with a destination address that is a link-local address or a site-local address, routers can forward them.

-> A C

> Regarding the HTTP protocol, which of the following statements are incorrect ( )? 
> A Stateful, previous and subsequent requests are related. 
> B FTP can also use the HTTP protocol. 
> C HTTP responses include numeric status codes, and 200 means the request was successfully returned. 
> D HTTP and TCP, UDP are protocols at the same level in the network layering.

-> A, B, and D

A is incorrect. HTTP is a stateless protocol, which means each request from a client to a server is treated as a new request and is handled independently, without any knowledge of previously sent requests. However, mechanisms like cookies and sessions are used to achieve statefulness at a higher level. B is incorrect. FTP (File Transfer Protocol) and HTTP (Hypertext Transfer Protocol) are distinct protocols. FTP is designed for transferring files, while HTTP is designed for transferring web documents and resources. D is incorrect. HTTP is an application layer protocol, while TCP and UDP are transport layer protocols in the context of the OSI model or Internet protocol suite.

> A switch with a speed of 100M has 20 ports. One of its ports is connected to a laptop. How long might it take for this computer to download a 1G movie

-> The switch has dedicated bandwidth, meaning each port can transmit data at a maximum rate of 100Mb/s. Note that the unit is Mb. Therefore, the shortest time is: 1GB/(100Mb/s) = 1024MB/(12.5MB/s) = 81.92s.

> In the OSI model, the relationship between the Nth layer and the layer above it, N+1, is ( ).
> A. The Nth layer provides services to the N+1 layer. 
> B. The N+1 layer adds a header to the information received from the Nth layer. 
> C. The Nth layer utilizes the services provided by the N+1 layer. 
> D. The Nth layer has no effect on the N+1 layer.

-> A

> For a Class A network, if the subnet-id consists of 16 ones, what is its subnet mask? ( )

-> 255.255.255.0

For a Class A network, the default subnet mask is 255.0.0.0. This means the first 8 bits (the first octet) represent the network portion, and the remaining 24 bits represent the host portion.

If the subnet-id consists of 16 ones, it implies that 16 bits are being used for subnetting.

Given this:

1. The first 8 bits (Class A's network portion): 11111111 in binary = 255 in decimal
2. The next 8 bits (first 8 bits of the subnet-id): 11111111 in binary = 255 in decimal
3. The following 8 bits (second 8 bits of the subnet-id): 11111111 in binary = 255 in decimal
4. The last 8 bits are for the host portion: 00000000 in binary = 0 in decimal

Combining the values, the subnet mask is:

255.255.255.0

> 10Gb/s Ethernet only operates in full-duplex mode, so there is no contention issue. It does not use the CSMA/CD protocol, which means the transmission distance of 10Gb/s Ethernet is no longer limited by collision detection. ( ) 
> A. Correct 
> B. Incorrect

-> A

> Assuming the disk head is currently at track 105 and is moving in the direction of increasing track numbers, given a sequence of track access requests as 35, 45, 12, 68, 110, 180, 170, 195, the sequence of track access determined using SCAN scheduling (elevator scheduling) is ( ).

-> 110 -> 170 -> 180 -> 195 -> 68 -> 45 -> 35

The SCAN scheduling algorithm (often referred to as the elevator algorithm) moves the disk arm towards the end (either the nearest end or the furthest end, depending on the initial direction) until the requests are serviced or until it reaches the end, and then it reverses its direction.

Given:

- Current position: 105
- Direction: increasing track numbers
- Track requests: 35, 45, 12, 68, 110, 180, 170, 195

Using SCAN scheduling, the disk head will move in the direction of increasing track numbers first, servicing requests in that direction. Once it reaches the highest requested track number or the end of the disk, it will reverse direction and service the remaining requests in the direction of decreasing track numbers.

Following this:

1. From 105, it moves to the next closest requested track in the increasing direction: 110
2. Continues in the increasing direction: 170
3. Then to: 180
4. And finally: 195

Now, having reached the highest requested track number in the queue, the disk head reverses direction:

5. Moves to: 68
6. Then to: 45
7. And finally to: 35 (Note: 12 is not visited because the head had already passed this point when it started and SCAN will not reverse direction until it hits the end or the highest/lowest request.)

So, the sequence of track access determined using SCAN scheduling is: 105 (starting point) -> 110 -> 170 -> 180 -> 195 -> 68 -> 45 -> 35.

> Among the following options, the event that causes a process to transition from the running state to the ready state is:
> A. Execute P(wait) operation 
> B. Failed memory request 
> C. Initiating an I/O device 
> D. Being preempted by a higher priority process

-> Requesting memory is the process of preparing the running space for a process. Starting I/O might also be preparing storage space for the process, for instance, a hard drive is a typical I/O device. Executing the P operation is a basic judgment for process resource access. All these operations are part of the process creation and resource allocation process and do not represent characteristics of transitioning from the running state back to the ready state.


> The classic OSI model defines a seven-layer network protocol. Among the following protocols, which one belongs to the network layer? ( ) 
> A. IP 
> B. TCP 
> C. UDP 
> D. HTTP

-> A

Physical Layer: EIA/TIA-232, EIA/TIA-499, V.35, V.24, RJ45, Ethernet, 802.3, 802.5, FDDI, NRZI, NRZ, B8ZS

Data Link Layer: Frame Relay, HDLC, PPP, IEEE 802.3/802.2, FDDI, ATM, IEEE 802.5/802.2

Network Layer: IP, IPX, AppleTalk DDP

Transport Layer: TCP, UDP, SPX

Session Layer: RPC, SQL, NFS, NetBIOS, names, AppleTalk ASP, DECnet, SCP

Presentation Layer: TIFF, GIF, JPEG, PICT, ASCII, EBCDIC, encryption, MPEG, MIDI, HTML

Application Layer: FTP, WWW, Telnet, NFS, SMTP, Gateway, SNMP

> If router R drops an IP packet due to congestion, the type of ICMP message that R can send to the source host of that IP packet is ()
> A. Route Redirection 
> B. Destination Unreachable 
> C. Source Quench 
> D. Time Exceeded

-> C. Source Quench

A. Route Redirection - This message is used to notify a host to update its routing information (usually to send its packets through another router).

B. Destination Unreachable - This message indicates that the destination is unreachable for some reason other than congestion.

C. Source Quench - This message is used to request the sender to reduce the rate at which it is sending traffic to the internet destination. It is essentially a congestion control mechanism.

D. Time Exceeded - This message indicates that a packet has been discarded because it was in the network for too long (either due to its TTL expiring or due to reassembly time for a fragmented packet expiring).

> The following descriptions about the router are correct (switch refers to a Layer 2 switch) ( ) A. Compared to switches and bridges, routers have more complex functions. 
> B. Compared to switches and bridges, routers have a lower latency. 
> C. Compared to switches and bridges, routers can provide greater bandwidth and data forwarding capabilities. 
> D. Routers can facilitate communication between different subnets, whereas switches and bridges cannot. 
> E. Routers can facilitate communication between different VLANs, whereas switches and bridges cannot.

-> ADE

A. Routers operate at the network layer, while switches operate at the link layer. Routers have all the functionalities of switches and are more complex.

B. Incorrect. The more complex the device, the greater the latency. This is because routers need to maintain a routing table.

C. Incorrect. Switches can provide greater bandwidth and data forwarding capabilities.

D. Correct. Routers can establish a local area network, while switches connect hosts within the same local area network.

E. Correct. Routers can establish VLANs, but Layer 2 switches cannot.

