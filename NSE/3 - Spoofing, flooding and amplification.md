## Spoofing

#### Internet Layer Identity

- A computer is identified by its IP address (either IPv4, 32 bit, or IPv6, 128 bit) which is assigned by users or by software.

#### Link Layer Identity

- A computer is identified by its MAC (media access control) address, or the physical address, associated with its network card. This is assigned by the manufacturer.  
  - 48 bits long, comprised of:  
    - OUI (Organizationally Unique Identifier): 24 bits  
    - NIC (Network Interface Controller): 24 bits

How do we translate from IP addresses to MAC addresses when trying to reach an individual computer? Using the **Address Resolution Protocol**

#### 

### Address Resolution Protocol (ARP)

Translates from IP to MAC addresses. RARP (Reverse ARP) does the opposite.

##### ARP Operation

- Client 1 wants to communicate with Client 2, but doesn’t know Client 2’s MAC address (only its IP)  
- Client 1 broadcasts an ARP request across the entire network, with the IP address of who it’s trying to reach.  
- Client 2 responds to Client 1 with its MAC address.

##### RARP Operation

- Similarly, a RARP request is broadcast with the MAC address, but in this case the host is trying to find its own IP address.  
- The RARP server responds to the client with the client’s IP address.

Frames of type 0x080**6** are **ARP** frames, and type 0x080**5** are **RARP**

##### ARP Request Message Generation

1. ARP protocol is called by a hardware driver when an IP packet needs to be sent out on the local area network (LAN)  
2. Get the IP address of the target  
3. Do I have the corresponding MAC address in my translation table?  
   1. If yes, return the 48 bit MAC address to the caller (hardware driver)  
   2. If no, prepare the ARP request message with:  
      1. Sender’s MAC address  
      2. Sender’s IP address  
      3. Target’s IP address  
      4. Target MAC address (default value of 0\)  
4. The message is passed to the link layer, where it’s encapsulated in a frame  
   1. Source address: Sender’s MAC address  
   2. Destination address: The broadcast address  
   3. Type: 0x0806 	

##### ARP Request Message Reception

1. Every host or router on the LAN receives the frame  
   1. All nodes pass it to ARP  
   2. ARP updates the **translation table** with:  
      1. Sender’s IP address, sender’s MAC address  
   3. All nodes except the targeted one drop the packet  
2. Target node replies with an ARP message that contains its MAC address  
   4. Source address: MAC of the target  
   5. Destination address: MAC of the sender  
   6. (Unicast message)

##### ARP Reply Message Reception

1. Update the translation table 

### The ARP Translation Table
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcat0FcEIWRB2ucJdhcXC_ihgjMDoCg_JL0Y9SNRhe-uZZPVy2ns4mjancNxfMeVzscoAlzCbtNzEMlAqdN4tfmyp19Lzh7Cjp9pfIwlzMejvdix9Rg-acjuxPgFCchY6V0hRO95Q?key=QsrhzRGmibZ4Z-AewHplBmKW)
To prevent making an ARP request every time, a host maintains a cache / table of IP addresses and their corresponding MAC addresses

- An entry in the ARP table is usually ‘aged’ and contents are erased if no activity occurs within a period.  
- The table is updated when hearing an ARP request or reply.  
- ARP is a stateless protocol, so most OSes will updatae their table if:  
  - A request is received, regardless of whether they are the target  
  - A reply is received regardless of whether they have sent out an actual request

### ARP Cache Poisoning by Spoofing

Constructing spoofed ARP replies and actively join the ARP protocol run. 

- A target computer A could be convinced to send frames to an attacker, instead of to the intended recipient.  
  - Computer A **will not know** that the attack took place

### MITM Attack with ARP Poisoning
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdlSamBvZTD4mnKL0roK1AfNSytEvxjPHgv5OEqveiziI2PLWaBCsUunRaNV8WWDtVqC9P-VNGWnRZVedp5HQwfRldGwXSgfp5kwxXhPx1z863XFcqWuw73wRFHIaKR2ZSfE3ShHQ?key=QsrhzRGmibZ4Z-AewHplBmKW)
### MAC Flooding with ARP Spoofing

#### CAM / MAC table

Switches have an internal CAM (Content Addressable Memory) table that maps switch ports to MAC addresses.  
When a frame arrives at a switch, the table is checked to know which port to forward each specific packet to.

In a MAC flooding attack, a switch is spammed with frames containing different source MAC addresses, to overload the limited memory (table) of the switch. The intention is to force legitimate MAC addresses out of the table, and for incoming traffic to be ‘flooded’ out of all ports of the switch.

Then, since all traffic is essentially being broadcast to everyone connected to the switch, the attacker can capture and analyse packets.

## DHCP Starvation and Spoofing

### Dynamic Host Configuration Protocol (DHCP)

Automatically provides clients with IP addresses  
**DORA** between client and DHCP server:

- **D**iscovery  
  - Client broadcasts a message to discover the server  
- **O**ffer  
  - Server replies with a message offering an IP  
- **R**equest  
  - Client accepts the IP offered  
- **A**cknowledge  
  - Server acknowledges the IP and sends other related configuration

#### DHCP Request

Contains the source MAC address of the network device requiring an IP address.

### DHCP Starvation

If an attacker sends a large number of DHCP Request messages to the server (with various spoofed \[fake\] MAC addresses), the available IP addresses will be exhausted (starvation).

### DHCP Spoofing

Once the DHCP server is effectively out of commission, the attacker can set up their own malicious DHCP server, which responds to **D**iscovery messages coming from clients.

The attacker can then start distributing IP addresses and other network information, meaning they can supply malicious addresses and perform MITM / sniffing attacks.

## ICMP Smurfing

#### Ping

Used to verify that a particular IP address exists and is accepting requests.

- Request-reply format.

A smurf attack consists of the following:

- The attacker generates a fake ping (echo request), where the source IP is the IP of the intended target, and the destination as a network’s broadcast address.  
- The request will reach all nodes on the network (due to the broadcast address), which will all reply to the request, sending replies to the target IP.  
- With enough replies, the target will be overloaded, and DoS will be achieved.

This is made much more effective with **amplification.**

## NTP Amplification DDoS Attack

### NTP

Network time protocol allows computers to synchronise their clocks.

- UDP based protocol using port 123

‘monlist’ is a deprecated command that replies to the source with a list of the last 600 IP addresses that connected to the NTP server

- Deprecated, but old servers might still have it enabled.  
- What’s important is that compared to the sent UDP packet size, the received packet size (600 IPs\!) is much bigger.  
  - For specifics, the NTP request is 234 bytes.  
  - The NTP response is 48200 bytes.  
  - This means an amplification factor of approximately 206 times.

So, if you make a bunch of monlist requests to an NTP server, with the spoofed source IP as the IP of your victim, you can overwhelm them and result in DoS.

## Botnet

A ro**bot** **net**work.

- A collection of internet-connected devices that are remotely controlled by a third party.  
- Typically used for DDoS attacks

#### Building a Botnet

- Infect device with malware  
- Connect the device (now a bot) to a Command and Control server, that is used to monitor and instruct the bot.

## TCP

Transport-layer protocol

- Connection oriented  
- Reliability ensured by segment sequencing, re-transmission and loss detection  
- Flow and congestion control through sliding window and loss detection

### Three-way handshake

- SYN  
- SYN, ACK  
- ACK

### TCP SYN Attack

- Send a SYN request, but with the source as a spoofed IP  
- Server will reply with a SYN, ACK and wait for an ACK  
- However, since the spoofed IP never sent a SYN, the server is waiting for an ACK that will never arrive.  
- While waiting, the server will effectively be putting some resources on hold.  
- Making the server wait for enough ACKs will result in a DoS.

[[4 - Hijacking and poisoning]]