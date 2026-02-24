# Network analysis
Network analysis offers insight into the network activity of threat actors: the flow of traffic from a compromised host to a C2 (Command and Control) server.
- the network is the great equalizer 
- most malware needs to communicate (except some wipers or air-gapped purposed malware)
- no matter how much time it may lie dormant, most malware dropped on a host eventually calls home to a C2 server

## Network evidence
- network log files (internal logs from switches, routers, firewalls, proxies, WAFs)
- network packet captures

## Network protocols
### TCP/UDP
### DNS
### HTTP/HTTPS

## Identify C2 patterns
- the act of checking in with the server for any commands is often referred to as **beaconing**
- if no commands from the server, the client goes to sleep a set amount of time, called **sleep** time
- sleep time can vary a set % called **jitter** (ex. sleep 10s with jitter 20% -> sleep can be anything between 8-12s)

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
[+] []()<br />
[+] []()<br />
[+] []()<br />
