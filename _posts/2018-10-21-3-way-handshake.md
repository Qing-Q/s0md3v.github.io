---
layout: post
title: TCP Header & 3 Way Handshake
description: "How a TCP connection is established?"
modified: 21-10-2018
tags: [tcp, networking]
image:
  path: /images/featured/tcp-3-way-handshake.png
  feature: /featured/tcp-3-way-handshake.png
---

Previously we learned [how data travels over Internet](https://somdev.me/tcp-vs-udp/) and today we will be talking about TCP Header.
So how do data packets know where they have to go? What port they have to reach? How will the client know who sent the data? Blah! Blah! Blah!<br>
<!--more-->
This is where Headers come into play. A data packet has to carry some information which is necessary for the transmission and this information is what we call a Header.
Today we are going to talk about TCP Header. The size of a TCP Header can range from 20 bytes to 60 bytes.<br>
So what kind of information does the header contain?
Lets take a look at this representation of TCP Header

![tcp header](/images/tcp-header.png)

Now lets see what this all weird stuff means,

1. **Source Port**: The port which sent the data packet.

2. **Destination Port**: The port which is supposed to receive the data packet.

3. **Sequence Number**: It used to identify and reassemble the received data packets.

3. **Data Offset**: It tells the receiver where data is present in the data packet so the client can directly jump to the data. Basically its function is to tell where header ends and data (which is meant to be sent and received) begins.

5. **Control Flags**: These are used to indicate the nature of the data packet.

- **URG (Urgent Flag)**: It is used when we need to prioritize a data packet.
- **ACK (Acknowledgement Flag)**: It is used in TCP 3 way handshake. (You will read about it later in the article)
- **PSH (Push Flag)**: It is used to when we need to send the data packets immediately and is mainly used in streaming (where we need continuity).
- **RST (Reset Flag)**: It is used to reset the connection.
- **SYN (Synchronize Flag)**: It is used in TCP 3 way handshake. (You will read about it later in the article)
- **FIN (End Of Data)**: It indicates the end of connection like when a download is complete the sender (server) sends a FIN Flagged packet and connection is terminated.
  
  There are 3 other flags too but we are only discussed the standard 6.

6. **Urgent Pointer**: It points to the end of urgent data in the packet. It only used when there is URG Flag on the data packet.

7. **Checksum**: This field is used by the receiver to verify the integrity of the data. If the check fails, receiver rejects the data.

8. **Window Size**: It is the maximum amount of received data, in bytes, that can be buffered at one time on the receiving side of a connection. The sender can send only that amount of data before waiting for an acknowledgment and window update from the receiver.

9. **Reserved**: It is reserved for future use. Its value is always zero.

10. **Padding**: It is the area of header which is filled with zeros to ensure that the size of header is a multiple of 32 bits. If you are not good at maths then let me tell you, being multiple of 32 means the size of the data packet (in bits, 8 bits = 1 byte) should be divisible by 32 and that necessary for the proper processing of the data.

11. **Options**: It may contain additional fields which can be added to the header. (Out of the scope of this article)

### TCP 3 Way Handshake
Lets say two computers A and B wants to connect to each other.

Step 1. Computer A sends a TCP data packet with SYN flag.<br>
Step 2. Computer B receives the TCP data packet and by looking at SYN flag it finds out that A wants to connect.<br>
Step 3. Computer B sends a TCP data packet with two flags, SYN and ACK.<br>
Step 4. Computer A receives the TCP data packet and with the SYN-ACK Flags it finds out that B has confirmed the connection.<br>
Step 5. Computer A sends a TCP data packet with ACK Flag on it.<br>
Step 6. Computer B receives the TCP data packet and by looking at the ACK flag it finds out A received its SYN-ACK Packet successfully.<br>
Step 7. A TCP connection is established.

Yay!
