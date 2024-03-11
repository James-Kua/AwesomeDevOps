---
title: "TCP-UDP"
---

# TCP/UDP

## TCP

The transmission control protocol (TCP) is defined as a connection-oriented communication protocol that allows computing devices and applications to send data via a network and verify its delivery, forming one of the crucial pillars of the global internet.

![](https://images.spiceworks.com/wp-content/uploads/2022/04/05123619/10-2.png)

TCP (Transmission Control Protocol) is a reliable transport protocol because it includes mechanisms to ensure that data is delivered to the destination without errors and in the correct order. Here's how TCP achieves reliability:

+ Sequence Numbers: TCP assigns sequence numbers to each byte of data, allowing the receiver to identify missing or duplicate data.
+ Acknowledgments (ACKs): The receiver sends ACKs to the sender, confirming the successful receipt of data segments.
+ Retransmissions: If the sender does not receive an ACK within a certain time, it will retransmit the unacknowledged data.
+ Error Detection: TCP uses checksums to detect corrupted data segments.
+ Flow Control: TCP regulates the amount of data that can be sent to prevent overwhelming the receiver's buffer.
+ Connection-oriented: TCP establishes a reliable end-to-end connection before transmitting data.
These features work together to ensure that data is delivered reliably, even in the presence of packet loss, corruption, or reordering.

## UDP

![](https://images.spiceworks.com/wp-content/uploads/2022/04/14111046/105.png)

User datagram protocol (UDP) is a message-oriented communication protocol that allows computing devices and applications to send data via a network without verifying its delivery, which is best suited to real-time communication and broadcast systems. 

As with TCP, its purpose is to send and receive messages, so its functioning is similar to the transmission control protocol. What is distinctive about UDP is that it is not connection-based. In this case, “connectionless” refers to the fact that no connection is established before communication occurs. 

Furthermore, it does not ensure the delivery of the data packets from the server. It is commonly referred to as the “fire-and-forget” protocol because it is not concerned about whether or not the client receives the data.

In most cases, UDP is faster than TCP because it does not assure delivery of the packets as TCP does.

The UDP protocol is not suitable for sending electronic mail, viewing a web page, or downloading a file. However, it is preferred mainly for real-time applications like broadcasting or multitasking network traffic. UDP’s key features are as follows:

+ It adapts to bandwidth-intensive applications that tolerate a loss of packets.
+ There will be fewer delays in data transmission.
+ It is used to send a large number of packets at a time.
+ There is a possibility that you may lose some data.

## Connection

Since TCP is a connection-oriented protocol, it relies on a server in a passive open state. A passive open server listens for any client trying to connect with it. The client must first connect with the server and then send or receive data. The connection is established via a three-way handshake. The client sends a synchronization request, the server sends back an acknowledgment, and the client returns a synchronization acknowledgment in response. 

Comparatively, UDP is a connectionless protocol. This type of data transmission involves an endpoint of a network sending an IT signal without checking whether a receiver is available or available to receive the signal. The message is sent out, without as much regard for the recipient, without considering the destination. Connectionless transport protocols can lose a minimal number of packets. However, this isn’t always apparent to the receiving client, for example, during video calls.

## Error Checking

Transmission control protocol uses three different mechanisms to check for errors and ensure data integrity at the time of delivery. This makes it highly reliable. TCP checks for errors by: 

+ Restraining the connection after a timeout period: The connection has a designated timeout period. If the server or client does not receive an acknowledgment message within this period, the connection will close and must be reestablished before you can transfer data. 

+ Including a checksum field in the header: Data packets include a 16-bit value in the header, known as the checksum field. TCP includes a checksum field for every data segment, which it evaluates for integrity during transmission. 

+ Sending and receiving acknowledgments: When a connection is established, or data is sent, the server transmits an acknowledgment or ACK message. The client receives the acknowledgment and sends back its message by adding one to the ACK message value. 

These three measures ensure that the correct data streams are transmitted via TCP without any loss or corruption, are transmitted via TCP. In contrast, UDP only runs a basic error check using a checksum. 

## Sequence

To determine which application process it needs to hand the data segment on to, TCP uses port numbers. Moreover, it synchronizes itself with the remote host by using sequence numbers. Every segment of data is sent and received with sequence numbers. This allows the system to track the specific order in which data is transmitted, maintaining the desired sequence. 

UDP does not follow a sequencing mechanism. Data packets are sent independently and in no fixed order and are stitched back together at the recipient application. Keep in mind that they will be stitched back together in the order they are received – i.e., the protocol has no way of telling which data packets should come first, and if they are received in the wrong order. Applications will receive packets incorrectly. UDP also drops any data packet that it is unable to process.

## Efficiency

One of the key reasons why UDP is so popular, despite its intrinsic flaws, is its speed and efficiency. User datagram protocol does not need an established connection to start sending packets. Therefore, it saves the time typically required to turn on the server and place it in a “passive open,” listening state. It allows data transmission to begin faster without delays or extended latency time. There is also no need to put the packets in sequence or send and receive acknowledgments, saving time.

In addition to latency, UDP is also more efficient in terms of bandwidth. Once the data is in motion from the server to the client, TCP engages in many error check mechanisms, acknowledgment processes, and sequencing measures, which occupy a lot of bandwidth. In contrast, UDP quickly gets the data stream from one computing location to another without a lot of checks and balances. This makes it suitable for low-performing networks, mobile devices, and other connectivity conditions where resources may not be so readily available. 

The transmission control protocol is slower than UDP and more resource-intensive. If a data sequence gets corrupted, TCP will restart the connection all over again, requiring the server to send and receive an acknowledgment, establish a three-way handshake, etc. UDP simply drops the lost or corrupted packet and then moves on to the next one, making it significantly more efficient. 

## Flow Control

Flow control is a mechanism by which the server first checks the recipient’s capacity to understand how much data it can accept and at what speed. Transmission control protocol implements flow control through the sliding window method. The recipient gives the transmitter permission to send data until a window is full in a sliding window. Once this happens, the transmitter must wait until the recipient clarifies that a larger window is available. 

TCP utilizes flow control information to calibrate the pace of data transmission. Depending on the recipient host, transmission control protocol can adjust the speed at which data packets travel and avoid overwhelming the recipient. However, this also means that the server will wait for flow control information before sending every packet, making it slower and less efficient. 

UDP does not use any flow control techniques. It sends data at a pace best suited to the originating server, and as a result, a powerful server may bombard a recipient device with multiple consecutive data streams. Organizations may deploy routers to intervene in UDP data flows and calibrate the pace at which data packets are sent through traffic policing policies. When UDP sends data too fast, and the recipient is overwhelmed, it simply drops the data packets that the recipient cannot accept. 

## Reliability

TCP’s biggest advantage is its high reliability, this can be attributed to: 

+ Transmission control protocol is connection-based. It will only send data to clients that are listening for it. 

+ It uses a three-way handshake system to maintain the connection while data is transmitted consistently. If the connection is interrupted, the transmission will also stop, and there will be no loss of data packets. 

+ TCP uses sequencing mechanisms to send data in the correct order. This means that images, web pages, data files, and other information types sent via this protocol will arrive in an uncorrupted condition. 

+ TCP provides a guarantee that the data will be delivered. It obtains an acknowledgment for every data packet received and sends the next packet only after the client sends an ACK message. 

+ TCP uses flow and congestion control mechanisms to ensure that data is not lost, damaged, duplicated, or delivered out of order. 

In contrast, the user datagram protocol is not inherently reliable. Its architecture is designed to continuously send data packets to one or more receiving clients without waiting for a “listening” state or acknowledgment. In challenging network conditions, TCP and UDP may result in lost packets. The difference is that TCP will recognize the loss and identify the lost packet to retransmit the information. UDP has no way to tell if packets are lost in transmission, which ones were lost, or how to resend them. This makes UDP less reliable, despite being more efficient. 

Applications using the UDP protocol must separately configure reliability mechanisms. For instance, it can separately configure a time-off period for data transmission and proactively cut off the UDP protocol if no signal is received from the recipient within a stipulated time. 

## Headers

Any communication protocol allows information to be exchanged in a string of bytes. These “bitstrings” comprise multiple fields, and each field contains some information relevant to a particular protocol. A bitstring has two parts: the header and the payload. The payload contains the main body of the message, while the header is used to identify and support the operation of the communication protocol. TCP and UDP data transmissions leverage two different kinds of headers. 

To begin with, TCP uses a variable-length header to support more complex data transmissions without compromising on reliability. The header can have anywhere between 20 and 60 bytes. In comparison, UDP has a fixed-length header, which is fast and efficient but less versatile. A UDP header can have only eight bytes. 

TCP and UDP headers (i.e., their fields) are also different. TCP headers contain designated fields for the sequence number, checksum, the ACK number, a control bit, sliding window information, source port, destination port, and several others. In contrast, UDP headers are shorter and simpler as they only contain fields for checksum, source port, destination port, and a few other elements. 

## Applications

Despite its inherently unreliable nature, UDP continues to be a staple for online operations. This is because it is ideal for real-time data transmissions, where the loss of a few packets does not matter. 

For example, in an online game, a lost packet will only skip a few frames and may cause the player to lose a few points. User datagram protocol will continue to send the subsequent data packets, and the user can keep playing. However, TCP will take cognizance if a single packet is lost. It will restart the connection and retransmit the data, which will freeze the game. Transmission control protocol can negatively impact the user experience in such scenarios. 

TCP is best for use cases where data integrity matters more than transmission speed. It will ensure that files and web pages arrive intact and can even be helpful for real-time analytics and content delivery networks, where dropped packets would fudge the outcomes. In comparison, UDP is suitable for media transmissions, such as: 

+ Video calling: UDP can support video 30 frames per second or more refresh rates. The data transmission is so fast that a few dropped packets do not affect the user experience. 

+ Online gaming: TCP’s many checklists and balances will significantly impact gaming experiences. Without perfect network conditions, frames will frequently freeze, and connections will restart if using TCP. That is why UDP is recommended. 
