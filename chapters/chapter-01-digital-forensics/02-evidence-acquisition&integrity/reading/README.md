# Evidence acquisition & integrity
Because digital forensics is concerned with _"analyzing digital evidence in a manner that is legally acceptable in any legal proceedings”_, evidence integrity is key. [1]<br /> 
After an incident has been identified and scoped, next comes the evidence collection phase.<br /> 
Collected evidence must:
- be relevant and of consequence to proving or disproving a hypothesis;
- have its integrity preserved.

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
-  archival media.<br />

For example, if network logs roll over a 24h period, they should be acquired as soon as possible at the debut of the investigation, and then assess if they are required.

## Evidence acquisition - Host-based
From the hosts we usually collect disk and memory artifacts.<br />
Disk artifacts include:
- information about file system metadata (ex. $MFT file on windows)
- evidence of execution (ex. Prefetch files, SRUM)
- evidence of past file presence (ex. USN Journal, Windows Search Index)
- evidence of network communication (ex. BITS Database)
- evidence of access (ex. shellbags)
- browser artifacts (ex. credentials, cookies, browsing history, etc)

Memory artifacts include memory dumps.

### UAC - [Unix-like Artifacts Collector](https://github.com/tclahr/uac)
Live collection tool for Unix-like operating systems.<br />
Usage: 
```
./uac -p ir_triage /tmp # Collect all artifacts based on the ir_triage profile, and save the output file to /tmp.
```
UAC comes with two basic pre-defined profiles:
- "ir_triage" – captures important system artifacts needed for typical investigations
- "full" – everything in "ir_triage" plus browser and other user application artifacts

### Autopsy 

### FTK Imager

### EZ Tools
- mftecmd (extract and parse MFT and USN Journal)

- pecmd (Prefetch parser)

- Registry Explorer (parser and viewer, fixes dirty hives)

- AppCompatCacheParser (shimcache, amcache parser)

- JLECmd (jumplist parser)

## Evidence acquisition - Network

## Evidence acquisition - Remote

## Preserving integrity
### Hash verification

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
[1] ISACA Whitepaper - [Overview of Digital Forensics](https://www.isaca.org/resources/white-papers/overview-of-digital-forensics)<br />
[+] IETF [RFC 3227 - Guidelines for Evidence Collection and Archiving](https://www.ietf.org/rfc/rfc3227.txt)<br />
[+] Hal Pomeranz's [Linux Forensics](https://archive.org/details/HalLinuxForensics)<br />
