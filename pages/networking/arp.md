---
title: "ARP"
---

# ARP


## Address Resolution Protocol 

Address Resolution Protocol (ARP) is a layer 2 protocol for mapping an Internet Protocol address (IP address) to a physical machine address that is recognized in the local network. For example, in IP Version 4, the most common level of IP in use today, an address is 32 bits long. In an Ethernet local area network, however, addresses for attached devices are 48 bits long. (The physical machine address is also known as a Media Access Control or MAC address.) A table, usually called the ARP cache, is used to maintain a correlation between each MAC address and its corresponding IP address. ARP provides the protocol rules for making this correlation and providing address conversion in both directions.

## How ARP Works

When an incoming packet destined for a host machine on a particular local area network arrives at a gateway, the gateway asks the ARP program to find a physical host or MAC address that matches the IP address. The ARP program looks in the ARP cache and, if it finds the address, provides it so that the packet can be converted to the right packet length and format and sent to the machine. If no entry is found for the IP address, ARP broadcasts a request packet in a special format to all the machines on the LAN to see if one machine knows that it has that IP address associated with it. A machine that recognizes the IP address as its own returns a reply so indicating. ARP updates the ARP cache for future reference and then sends the packet to the MAC address that replied.

Since protocol details differ for each type of local area network, there are separate ARP Requests for Comments (RFC) for Ethernet, ATM, Fiber Distributed-Data Interface, HIPPI, and other protocols.

There is a Reverse ARP (RARP) for host machines that don't know their IP address. RARP enables them to request their IP address from the gateway's ARP cache.


## ARP Steps

1. At the network layer when the source wants to find out the MAC address of the destination device it first looks for the MAC address (Physical Address) in the ARP cache or ARP table. If present there then it will use the MAC address from there for communication.

2. If the MAC address is not present in the ARP table then the source device will generate an ARP Request message. In the request message the source puts its own MAC address, its IP address, destination IP address and the destination MAC address is left blank since the source is trying to find this.

```
Sender's MAC Address	00-11-0a-78-45-AD 
Sender's IP Address	192.16.10.104

Target's MAC Address	00-00-00-00-00-00
Target's IP Address	192.16.20.204
```

3. The source device will broadcast the ARP request message to the local network.

4. The broadcast message is received by all the other devices in the LAN network. Now each device will compare the IP address of the destination with its own IP address. If the IP address of destination matches with the device's IP address then the device will send an ARP Reply message. If the IP addresses do not match then the device will simply drop the packet.

5. The device whose IP address has matched with the destination IP address in the packet will reply and send the ARP Reply message. This ARP Reply message contains the MAC address of this device. The destination device updates its ARP table and stores the MAC address of the source as it will need to contact the source soon. Now, the source becomes destination(target) for this device and the ARP Reply message is sent.

```
Sender's MAC Address	00-11-0a-78-45-AA	
Sender's IP Address	192.16.20.204	

Target's MAC Address	00-11-0a-78-45-AD	
Target's IP Address	192.16.10.104
```

6. The ARP reply message is unicast and it is not broadcasted because the source which is sending the ARP reply to the destination knows the MAC address of the source device.

7. When the source receives the ARP reply it comes to know about the destination MAC address and it also updates its ARP cache. Now the packets can be sent as the source nows destination MAC address.

## ARP Message Format

Address Resolution Protocol (ARP) is one of the major protocol in the TCP/IP suit and the purpose of Address Resolution Protocol (ARP) is to resolve an IPv4 address (32 bit Logical Address) to the physical address (48 bit MAC Address). Network Applications at the Application Layer use IPv4 Address to communicate with another device.  But at the Datalink layer, the addressing is MAC address (48 bit Physical Address), and this address is burned into the network card permanently. You can view your network card’s hardware address by typing the command `ipconfig /all` at the command prompt (Without double quotes using Windows Operating Systems).

The purpose of Address Resolution Protocol (ARP) is to find out the MAC address of a device in your Local Area Network (LAN), for the corresponding IPv4 address, which network application is trying to communicate.

![](https://media.geeksforgeeks.org/wp-content/uploads/20230210180525/ARP-Green-768.png)

Following are the fields in the Address Resolution Protocol (ARP) Message Format.

**Hardware Type:** Hardware Type field in the Address Resolution Protocol (ARP) Message specifies the type of hardware used for the local network transmitting the Address Resolution Protocol (ARP) message. Ethernet is the common Hardware Type and he value for Ethernet is 1. The size of this field is 2 bytes.

**Protocol Type:** Each protocol is assigned a number used in this field. IPv4 is 2048 (0x0800 in Hexa).

**Hardware Address Length:** Hardware Address Length in the Address Resolution Protocol (ARP) Message is length in bytes of a hardware (MAC) address. Ethernet MAC addresses are 6 bytes long.

**Protocol Address Length:** Length in bytes of a logical address (IPv4 Address). IPv4 addresses are 4 bytes long.

**Opcode:** Opcode field in the Address Resolution Protocol (ARP) Message specifies the nature of the ARP message. 1 for ARP request and 2 for ARP reply.

**Sender Hardware Address:** Layer 2 (MAC Address) address of the device sending the message.

**Sender Protocol Address:** The protocol address (IPv4 address) of the device sending the message

**Target Hardware Address:** Layer 2 (MAC Address) of the intended receiver. This field is ignored in requests.

**Target Protocol Address:** The protocol address (IPv4 Address) of the intended receiver.

## Types of ARP

There are different versions and use cases of ARP.

**Proxy ARP**

Proxy ARP is a technique by which a proxy device on a given network answers the ARP request for an IP address that is not on that network. The proxy is aware of the location of the traffic's destination and offers its own MAC address as the destination. 

**Gratuitous ARP**

Gratuitous ARP is almost like an administrative procedure, carried out as a way for a host on a network to simply announce or update its IP-to-MAC address. Gratuitous ARP is not prompted by an ARP request to translate an IP address to a MAC address.

**Reverse ARP (RARP)**

Host machines that do not know their own IP address can use the Reverse Address Resolution Protocol (RARP) for discovery.

**Inverse ARP (IARP)**

Whereas ARP uses an IP address to find a MAC address, IARP uses a MAC address to find an IP address.

## ARP Spoofing

ARP spoofing is also known as ARP poison routing or ARP cache poisoning. This is a type of malicious attack in which a cyber criminal sends fake ARP messages to a target LAN with the intention of linking their MAC address with the IP address of a legitimate device or server within the network. The link allows for data from the victim's computer to be sent to the attacker's computer instead of the original destination. 

ARP spoofing attacks can prove dangerous, as sensitive information can be passed between computers without the victims' knowledge. ARP spoofing also enables other forms of cyberattacks, including the following:

**Man-in-the-Middle (MTM) Attacks**

A man-in-the-middle (MITM) attack is a type of eavesdropping in which the cyberattacker intercepts, relays, and alters messages between two parties—who have no idea that a third party is involved—to steal information. The attacker may try to control and manipulate the messages of one of the parties, or of both, to obtain sensitive information. Because these types of attacks use sophisticated software to mimic the style and tone of conversations—including those that are text- and voice-based—a MITM attack is difficult to intercept and thwart.

A MITM attack occurs when malware is distributed and takes control of a victim's web browser. The browser itself is not important to the attacker, but the data that the victim shares very much is because it can include usernames, passwords, account numbers, and other sensitive information shared in chats and online discussions. 

Once they have control, the attacker creates a proxy between the victim and a legitimate site, usually with a fake lookalike site, to intercept any data between the victim and the legitimate site. Attackers do this with online banking and e-commerce sites to capture personal information and financial data. 

**Denial-of-Service Attacks**

A denial-of-service (DoS) attack is one in which a cyberattacker attempts to overwhelm systems, servers, and networks with traffic to prevent users from accessing them. A larger-scale DoS attack is known as a distributed denial-of-service (DDoS) attack, where a much larger number of sources are used to flood a system with traffic.

These types of attacks exploit known vulnerabilities in network protocols. When a large number of packets are transmitted to a vulnerable network, the service can easily become overwhelmed and then unavailable. 

## ARP vs DHCP vs DNS

ARP, DHCP, and DNS are all response-request protocols used on IP networks. But they serve different purposes.

+ ARP maps the MAC address of a machine to its IP address on the local network. ARP comes into the spotlight when you want to send a data packet to another device on the local network, but you only know its IP address, not the MAC address.

+ DHCP retrieves network configuration, including IP and DNS server addresses. DHCP automatically assigns an IP address to the new device you add to the local network, so you wouldn’t need to set an IP address for each device manually. It can provide other network configuration parameters, too.

+ DNS finds the IP address of a website based on its domain name or URL. For example, when you enter `nordvpn.com` into your browser’s address bar, DNS translates this human-readable domain name into IP addresses that computers can identify.

When it comes to ARP and DHCP, we are talking about the local area network and the local (or private) IP addresses. DHCP is (usually) what assigns the devices with their IP addresses on the local network, and ARP translates those IP addresses into MAC addresses.

Meanwhile, DNS also works with global (or public) IP addresses outside the local network. DNS translates any domain name on the internet into its corresponding IP addresses.