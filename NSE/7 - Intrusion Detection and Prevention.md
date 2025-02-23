##### Why do we need intrusion detection?
We already have firewalls!
- A firewall does access control
- A gatekeeper that blocks / drops packets
Alternatively, Intrusion Detection Systems (IDS):
- Detect and report intrusion attempts to the network
- If the firewall is the gatekeeper, IDS is the security camera *after* the gate
- Alerts any intrusion attempts to the security administrator
## Classes of Intruders
- Masqueraders
	- An individual who is not authorised to use the computer and who penetrates a system's access controls to exploit a legitimate user's account
	- An **outsider**
- Misfeasors
	- A legitimate user who accesses data, programs or resources for which such access is not authorised, or who is authorised for such access but misuses their privileges
	- An **insider**
- Clandestine user
	- An individual who seizes supervisory control of the system and uses this control to evade auditing and access controls or to suppress audit collection
	- An **insider or an outsider**
## Definitions
- Intrusion
	- Unwanted and unauthorised intentional access of computerised network resources
- Intrusion detection
	- Detecting unauthorised use of a system or network, detecting attacks upon a system or network
- Intrusion Detection System (IDS)
	- Does for a network what anti-virus software does with incoming files
	- Components:
		- Sensors, alerts
- Intrusion Prevention System (IPS)
	- An IDS with an automated response
	- Shuts down attackers' connections
	- Tries to back-trace attacker, and counter-attacks
## How to detect an intruder
![[Pasted image 20250223152431.png]]
- False positive
	- Authorised user identified as intruder
	- A loose interpretation of intruder behaviour catches more intruders, but also leads to more false positives.
- False negative
	- Intruders not identified as intruders
	- A tight interpretation of intruder behaviour limits false positives, but increases false negatives.
- Compromise between the two
### Approaches to Intrusion Detection
- Reputation detection
	- Detect host communication with someone of bad reputation
	- *How does an IDS know the reputation of hosts?*
- Anomaly-based detection
	- Detect intruders based on behavioural anomalies
	- First must define normal system behaviour
	- *What is normal system behaviour?*
- Misuse-based detection
	- Abnormal system behaviour defined first
	- *Can we define abnormal system behaviour?*
#### Reputation-based Detection
Can identify hosts of bad reputation using blacklists
- Blacklists can be public or private
- Examples:
	- Malware Domain List (MDL)
		- Google Safe Browsing
	- Spamhaus Block Lists
		- http://www.spamhaus.org/drop/
	- PhishTank
		- htttp://www.phishtank.com/
##### OSINT
- Data collected from publicly available sources that is used in intelligence (e.g. MDL)
- VirusTotal
	- https://www.virustotal.com/#/ip-address/71.78.24.146
#### Anomaly-based Detection
Define normal system behaviour first
- Record data from behaviour of legitimate users
- Apply statistical methods to observed behaviour to determine whether it is malicious or not
- Threshold detection
	- Involves defining thresholds (independent of user) for the frequency of occurrence of various events
- Profile based detection
	- A profile of the activity of each user is developed and used to detect changes in the behaviour of individual accounts
	- Focuses on characterising the past behaviour of a user (or a group of users) and then detects significant deviations.
##### Statistical Test
- Mean and standard deviation
	- Applied to a variety of behaviours and measures
	- Inadequate if used alone
- Multivariate claculation
	- Determine a correlation between two or more variables
- Markov process
	- Estimate transition probabilities among various states
	- Efficient in describing a protocol run
- Time series model
	- Observers calculate values based on sequence of events over time
	- Can be used to detect a series of actions that happens too fast or slow
##### Pros and Cons
- Advantages
	- Detects previously unknown attacks
	- Doesn't need updating
- Disadvantages
	- Difficult to configure (training)
	- Assumes that anomalous means malicious
	- Generates many false alarms (false positives)
	- Resource-intensive
#### Misuse-based Detection
Define abnormal system behaviour
- A set of rules or attack patterns that can be used to decide that a given behaviour is that of an intruder
- Often referred to as signature detection
- Rely on models of malicious behaviour
- Identify matching entries in the event stream
- Use OSINT: Indicators of Compromise (IoC)
- The rules / signatures / IoCs can be thought of as the blacklist in a firewall
##### IoCs
![[Pasted image 20250223154503.png]]
Pieces of forensic data, such as data found in logs, that identify potentially malicious activity on a system or network
- Can be host-based or network-based
- But can also be complex shellcode
- Often needs to be looked at together for correlation
##### Pros and Cons
- Advantages
	- Generates few false positives
	- Has a clear justification for alarms
	- Fast
	- More resilient to evasion
- Disadvantages
	- Only detects known attacks
	- Needs continuous updating
	- Vulnerable to over-stimulation attacks? (What does this mean)
#### Other IDS Classifications
- Timeliness
	- Real-time (on-line, continuously running)
	- Non real-time (off-line, periodic)
- Response type
	- Passive (generates alerts)
	- Active (blocks malicious traffic)
- State-dependency
	- Stateful analysis
	- Stateless analysis
- System type
	- Software
	- Hardware
- Topology
	- Host IDS (HIDS)
	- Network IDS (NIDS)
	- Distributed IDS (you guessed it, DIDS)
		- Monitor the whole network with multiple probes (which are network or host-based)
##### Host IDS (HIDS)
Looks at the traffic on the local host
- NIC (Network Interface Controller) in non-promiscuous mode
- Can also consider:
	- Logs
	- Sys calls
	- Host activities
- An IDS tuned for local services only
##### Network IDS (NIDS)
Eavesdrop on all hosts in a network segment
- NIC in promiscuous mode
- Placed at strategic locations
	- Choke points (like firewalls)
	- DMZ (demilitarised zone)
	- Internal network (unlike firewalls)
###### Deployment
- Traditional deployment:
	- ![[Pasted image 20250223161142.png]]
	- Connected to a trunk port and can see all network traffic
	- Can detect attacks, no countermeasures
- Inline deployment:
	- ![[Pasted image 20250223161152.png]]
	- Traffic passes through the IDS, and becomes a de facto IPS
	- Can now prevent attacks
##### Distributed IDS (DIDS)
- Multiple sensors collect events and send events to a centralised manager
- Sensors can be NIDS and / or HIDS
- Manager collects correlated events
- Use a dedicated network or VPN for Probe-Manager traffic
- Have evolved towards what is commercially known as Security Information and Event Management (SIEM)
- 
[[8 - NSE]]