# Evidence acquisition & integrity
Because digital forensics is concerned with analyzing _“digital evidence in a manner that is legally acceptable in any legal proceedings (i.e., a court of law)”_[1], evidence integrity is key.<br /> 
After an incident has been identified and scoped, next comes the evidence collection phase.<br /> 
Evidence must:
- be relevant and of consequence to proving or disproving a hypothesis;
- have its integrity preserved;

## Evidence volatility
Evidence collection should be prioritized based on evidence volatility.<br />
According to the IETF [RFC 3227 - Guidelines for Evidence Collection and Archiving](https://www.ietf.org/rfc/rfc3227.txt), when collecting evidence you should proceed from the volatile to the less volatile. <br />
Here is an example order of volatility for a typical system.
-  registers, cache;
-  routing table, arp cache, process table, kernel statistics, memory;
-  temporary file systems;
-  disk;
-  remote logging and monitoring data that is relevant to the system in question;
-  physical configuration, network topology;
-  archival media.

## Evidence acquisition

### UAC - [Unix-like Artifacts Collector](https://github.com/tclahr/uac)
Live collection tool for Unix-like operating systems.<br />
Usage: 
```
./uac -p ir_triage /tmp # Collect all artifacts based on the ir_triage profile, and save the output file to /tmp.
```
UAC comes with two basic pre-defined profiles:
- "ir_triage" – captures important system artifacts needed for typical investigations
- "full" – everything in "ir_triage" plus browser and other user application artifacts

### 
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
[1] ISACA Whitepaper - [Overview of Digital Forensics](https://www.isaca.org/resources/white-papers/overview-of-digital-forensics)
[*] IETF [RFC 3227 - Guidelines for Evidence Collection and Archiving](https://www.ietf.org/rfc/rfc3227.txt)
[*] Hal Pomeranz's [Linux Forensics](https://archive.org/details/HalLinuxForensics)
