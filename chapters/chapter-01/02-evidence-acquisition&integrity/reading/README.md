# Evidence acquisition & integrity
Because digital forensics is concerned with analyzing _“digital evidence in a manner that is legally acceptable in any legal proceedings (i.e., a court of law).”_, evidence integrity is key.<br /> 
After an incident has been identified and scoped, next comes the evidence collection phase.
Evidence must:
- be relevant and of consequence to proving or disproving a hypothesis;
- have its integrity preserved;

## Evidence volatility
Evidence collection should be prioritized based on evidence volatility. 
According to the IETF [RFC 3227 - Guidelines for Evidence Collection and Archiving](https://www.ietf.org/rfc/rfc3227.txt), when collecting evidence you should proceed from the volatile to the less volatile. <br />
Here is an example order of volatility for a typical
   system.
-  registers, cache
-  routing table, arp cache, process table, kernel statistics, memory
-  temporary file systems
-  disk
-  remote logging and monitoring data that is relevant to the system in question
-  physical configuration, network topology
-  archival media.

## Evidence acquisition

## Preserving integrity

## Summary
- summary
## Drills
### Challenge 1
Description
### Challenge 2
Description
### Challenge 3
Description

## References & further reading
[*] IETF [RFC 3227 - Guidelines for Evidence Collection and Archiving](https://www.ietf.org/rfc/rfc3227.txt)
