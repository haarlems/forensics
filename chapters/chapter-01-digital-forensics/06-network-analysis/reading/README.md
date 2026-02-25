# Network analysis
Network analysis offers insight into the network activity of threat actors: the flow of traffic from a compromised host to a C2 (Command and Control) server. The main challenge is sifting through vast amounts of logs.
- the network is the great equalizer 
- most malware needs to communicate (except some wipers or air-gapped purposed malware)
- no matter how much time it may lie dormant, it eventually calls home to a C2 server

## Network evidence
Network traffic should be captured before, during and post compromise. The rolling retention of network evidence at key points in the network architecture is recommended. 

When analyzed, network evidence can give insights into:
- how the initial access was performed
- command and control activity
- if and how much data was exfiltrated
- if there was any recorded external reconnaissance (prior to initial access)
- internal lateral movement (depending on where the network evidence is collected from)

Types of network evidence:
- network log files
  - internal logs from switches, routers, firewalls, proxies, WAFs 
- network traffic data
  - full packet captures, zeek logs, netflow data

## Network protocols
During a breach, the same network protocols seen in normal network activity are abused by attackers.
- TCP = connection-oriented transport protocol that ensures reliable delivery of data between hosts
- UDP = connectionless transport protocol that sends packets without ensuring reliable delivery or order
- ICMP = protocol used for network troubleshooting, used by utilities like `ping` or `tracert`
- DNS = protocol that resolves domain names into IP addresses, operating over UDP or TCP
- TLS = protocol used to encrypt data to secure communications between clients and servers, operates over TCP
- HTTP = protocol used to transfer web content between clients and servers, operating over TCP
- HTTPS = encrypted version of HTTP using TLS (Transport Layer Security)

## Identify initial access

## Identify C2 patterns
- the act of a compromised system (client) checking in with the server for any commands is often referred to as **beaconing**
- if no commands received from the server, the client goes to sleep a set amount of time, called **sleep** time
- sleep time can vary a set % called **jitter** (ex. sleep 10s with jitter 20% -> sleep can be anything between 8-12s)
- beaconing creates regular traffic patterns that can be identified when looking at behaviour over a time range (ex. 24h)

Protocols used for C2 communication:
- HTTP/HTTPS = the most common, blends in with normal traffic that flows in an enterprise environment
- DNS = commonly used but suspicious traffic can be identified (abnormally large number of requests to the same domain, abusing [TXT, CNAME, NS, MX](https://www.activecountermeasures.com/a-network-threat-hunters-guide-to-dns-records/) records)
- ICMP = not very common as it's immediately suspicious to see large amounts of ICMP requests to remote hosts
- SMB = used to pivot peer-to-peer in windows environments, does not cross external network boundaries

With an additional layer of abstraction, C2s can also be achieved via applications, like the Google suite (Calendar, Drive, etc.), Discord, Velociraptor and many others, but they are outside the scope of this class. 

## Identify data exfiltration


## Summary
- summary
## Drills
### Challenge 1
Description
### Challenge 2
Description
### Challenge 3
Description

## Further reading
[+] [Detecting DNS C2](https://www.activecountermeasures.com/malware-of-the-day-encrypted-dns-comparison-detecting-c2-when-you-cant-see-the-queries/)<br />
[+] [A Network Threat Hunter’s Guide to DNS Records](https://www.activecountermeasures.com/a-network-threat-hunters-guide-to-dns-records/)<br />
[+] []()<br />
[+] []()<br />
