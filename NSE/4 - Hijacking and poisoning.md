## Hijacking Concepts

### Hijacking

Taking over what belongs to someone else  
In network contexts, taking over an existing connection.  
(In comparison, spoofing creates a new connection)

### Off-path vs On-path
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXevZErUjAhgdzsIXE7M_NnAOIw213Ah-Tde-60YQm6fpKwqUmLkXCzjyhtQ2THy3oKgfE8mP5TOVMWukWC_bj7EDRDPVOBMw1pz6txq5WOhc-d48nyKUjRp2u42e-4u_YHPacu60g?key=QsrhzRGmibZ4Z-AewHplBmKW)On path: Attacker resides on the communication path between two parties (man in the middle)  
Off path: Attacker is not on the path (lol) 

- Inserting false data is very difficult if off-path

## TCP Hijacking
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd92q97MhdVjwrZhe0OS7F6hFQ5H38VJ8kHNcAxeyaSpzndotQ1vQy9zv1kVhFyAOY8hLL2sdRoLV1fSRggr_d3STxsSgwCNCdZ-sQ9_wxbHveOIyeD2nd_vvReFSEVbs-tyyqN?key=QsrhzRGmibZ4Z-AewHplBmKW)TCP connection is established through three-way handshake  
In order to hijack a TCP connection, the attacker needs to construct a valid TCP segment acceptable by the client/server

- Requires the following:  
  - Source IP and port (easy)  
  - Destination IP and port (easy)  
  - Sequence number (easy if on-path)

### On-path TCP hijacking

1. Sniff packets  
2. Predict the sequence number  
   1. x \+ 1 is good  
   2. x \+ delta (where delta is in the boundary of the TCP buffer) is good  
      1. Just means that a segment has been ‘missed / overshot’, receiver will think TCP data has arrived out of order  
   3.   
3. Inject data spoofing as client / server

### Off-path capabilities

Blindly spoofing means the attacker has to guess to get the sequence number correct
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeeXm6kBi3Ik6p-zImnY1bNHQBnJFumLTocOdfmLNc_Pjj87uXjTYjAKNFU8-s7f4xrHgtzsg9VntjBe_YxL_1FRgjU2aeSmW9YmRMXnxVHZ1c4rKBUnQAVG6OEkhce_ksO1qy35w?key=QsrhzRGmibZ4Z-AewHplBmKW)

### How to guess y (from the previous diagram)

Exploit a vulnerability in the mismatch between TCP theory and practice

- Theory  
  - In the TCP specification, sequence numbers should not repeat for at least 4.55 hours  
  - Due to 32 bit ISN (Initial Sequence Number) generators being incremented every 4 microseconds  
- Practice  
  - BSD Unix implementation didn’t follow the guidelines  
  - Instead, sequence number increases by 128,000 every second, and by 64,000 for every TCP connection  
  - Becomes relatively easy to predict, and much more readily exploited.

### Annotated Example
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdKoul-9vwWxJWrjWYlg9h1TfpgBFidSoXVFw59syaF6w-Eb-bOSIG9YCJtVRnwV8VM4N_hvkBah8D1naIx97wqkN748cM2AZIA7KcqAeRw6xnvWzVrps6aMYPRUVcS7vqv_oP0EQ?key=QsrhzRGmibZ4Z-AewHplBmKW)
1. Attacker floods the client, meaning they cannot respond to requests  
2. Attacker sends SYN packet to the server, asking the server to set up a legitimate TCP connection  
3. Server responds, using an initial sequence number y.  
   1. The attacker can now predict the value of the next initial sequence number.  
4. The attacker sends an RST (reset) packet to the server, to terminate the connection.  
5. The client sends a spoofed SYN packet to the server, with the source IP as the IP of the client  
6. Server responds to what it thinks is the client, with a new sequence number of y \+ N.  
   1. This packet is ignored by the client, as the client is already being flooded.  
7. Attacker replies as the client, with the ack no. y \+ N \+ 1\. Attacker knows y \+ N based on previous information (y), and N can be predicted.  
   1. The session has then been successfully hijacked, because the server believes the data is coming from the client.

## BGP Route Hijacking

## Border Gateway Protocol (BGP)

- Connects ISPs  
- Inter-Autonomous Systems routing protocol  
  - Connects autonomous systems

### Autonomous Systems (AS)

- Either a single network or a group of networks that is controlled by a common network administrator on behalf of a single administrative entity  
- A.k.a. a routing domain  
- Assigned a globally unique number, called an Autonomous System Number (ASN)

### Intra-AS Routing

- RIP, OSPF  
  - Routing Information Protocol, Open Shortest Path First  
- Destination IP address is used to route packets

### Inter-AS routing and CIDR Prefix

- A.k.a. Inter-domain routing  
- All ASs run the same Inter-AS routing protocol, BGP  
- In BGP, packets are routed to a Classless Inter-Domain Routing (CIDR) prefix, which represents a subnet or collection of subnets.  
- Classless?  
  - Any number of bits for network part

#### CIDR Prefix

- The number of 1 bits in the subnet mask  
  - E.g. an IP address where 16 bits are for the network, and 16 bits are for the host  
  - Subnet mask is 16 1’s and then 16 0’s  
  - CIDR prefix \= 16  
  - An example subnet can be expressed as 172.16.0.0/16 in CIDR prefix notation

### BGP Routing Table

- Entries in the form of:  
  - (x, I)  
  - Where x is a CIDR prefix (length of subnet mask)  
  - I is an interface number for one of the router’s interfaces

### BGP Functionality

- Obtain network prefix reachability info from neighbouring ASs  
  - BGP allows each subnet to advertise its existence to the rest of the internet  
  - BGP makes sure that all the routers in the Internet know about that subnet  
- Determines the best routes to the network prefixes, based on:  
  - Policy   
  - Prefix reachability information  
- ASs preform longest prefix matching to select which neighoburs to route through  
  - E.g.: When the address 192.168.20.19 needs to be looked up in a router which has two entries:  
    - 192.168.20.16/28  
    - 192.168.0.0/16  
  - Both entries contain the looked up address, but the longest matching prefix is the first entry, so that entry will be chosen.

### BGP Route Advertisement

- Routers exchange routing information over TCP connections using port 179  
- eBGP (external BGP) TCP connection spans two ASs  
- iBGP (internal BGP) TCP connection between two routers in the same AS

## BGP Route Hijacking

- An AS can announce it has a route that it doesn’t actually have  
- Meaning traffic will get routed to this AS, but go nowhere  
- Can be done through prefixes or sub-prefixes  
  - Prefixes  
    - The whole prefix  
  - Sub-prefixes  
    - Just a subset of the prefix  
    - Will cause the false route to be chosen because of longest prefix matching

### A Routing Blackhole

- ASs advertises routes it cannot actually offer  
  - Packets disappear into the wrong AS

## DNS Cache Poisoning

### Domain Name System

- A distributed database implemented in a hierarchy of DNS servers  
- Application-layer protocol that allows hosts to query the distributed database

DNS has a hierarchy

- Tree like structure  
- Root server  
  - Top level domain (.com, .org, etc.)  
    - Authoritative servers (google.com)  
- For kcl.ac.uk:  
  - UK DNS server (.uk)  
    - UK education institutions (.ac)  
      - .kcl  
        - Web server (keats)

DNS name resolution is the process of mapping names to IP addresses, and vice versa

- A DNS resolver performs lookups by  contacting name servers  
- Typically iterative in practice (contacts each server one by one) rather than recursive (each subsequent server performs a lookup) due to recursion scaling poorly

### DNS Caching

- The performance of DNS is critical to the web, as for each query multiple sub-queries need to be performed.  
- Thus a cache is used

#### DNS Cache Poisoning

- If a local DNS server asks a malicious server for a DNS lookup, the attacker can return a malicious IP. This incorrect IP will be stored in the DNS server’s cache, and poisnon all subsequent queries to that name.

### How to do DNS caching

#### Query QID attack

- A DNS query contains:  
  - Source IP \+ port  
  - Destination IP \+ port  
  - Query ID  
- We care about Query ID  
  - Links queries and replies from the DNS server’s perspective  
  - If an attacker responds to a query with the right QID, then the attacker’s response will poinson the cache

How to get the QID?

- Can use tools like \`dig\` 

#### RRSet attack

- DNS response contains different Resosurce Record Sets (RRSets)  
- Basically additional DNS-lookup information supplied to the DNS server  
  - Possible to inject malicious DNS entries for other names here  
- Solution: Don’t allow additional information

[[5 - Firewalls]]