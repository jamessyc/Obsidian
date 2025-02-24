## Recap \+ Intro

Sniffing \= listening to network conversations that are not intended for you  
What are **‘network conversations’**?

- Nodes exchanging messages according to network protocols

#### Network Protocols

A set of rules that allow peer layers to communicate  
Consist of:

- Syntax  
  - Format of data block  
- Semantics  
  - Includes control information for coordination and error handling  
- Timing  
  - Speed matching and sequencing

## TCP/IP Protocol architecture

Consists of layers

| Layer | Protocols | Medium |
| ----- | ----- | ----- |
| **Application layer** Provides access to the TCP/IP environment for users and distributed information services Works with the transport layer to send and receive data | Defines standard internet services and network applications that anyone can use SMTP (Email) FTP (file transfer) SSH (remote access) HTTP (web) SIP (VoIP) DNS (name service) | Application byte stream |
| **Transport layer** Transfer of data between endpoints. May provide error control, flow control, congestion control | TCP \- Transmission Control Protocol UDP \- User Datagram Protocol | TCP/UDP segment |
| **Internet layer** Shield higher layers from details of physical network configuration.  Provides routing and forwarding between networks. | IPv4 IPv6 Mobile IP | IP packet |
| **Network access / link layer** Logical interface to network hardware. Media access control Transmission and access within the same local network | Ethernet Wifi  | MAC frame |
| **Physical layer** Transmission of bit stream Defines relationship between device and transmission medium Digital data \-\> Physical data Physical interface between computer and network |  | Physical bit stream |
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf4lBHZuzLIWpc3z15uWnz-BhuvhVJRIoB0ZqLhaeVw6kaHlROZknnknbeQnsEtOPtJmmbhOtIavi1MUHsQ4Re6HGs-qPNw2u8WkhsN8_6nBOzzqb4oaggw3AYcrLeVHPcTsCb5wg?key=QsrhzRGmibZ4Z-AewHplBmKW)

[[3 - Spoofing, flooding and amplification]]