# Disk Analysis
After data has been extracted and measures taken to maintain integrity, we focus on initial event analysis. <br />
Depending on the type of incident investigated (ransomware, insider threat, cryptojacking, etc.) we look for obvious IoCs. At this stage:
- high potential for false positives
- include anything that looks suspicious and rule out later

## System activity artifacts
- $MFT file
- USN Journal, Windows Search Index
## Evidence of execution
- Prefetch files, SRUM
- Registry (UserAssist, ShimCache / AppCompatCache, AmCache, RunMRU)

## Log analysis

## App & user activity artifacts
- Registry (RecentDocs, OpenSaveMRU, ShellBags)

### Identify logins

### Command history

### Browser artifacts

### Persistence
- Registry (Run/RunOnce Keys, Windows Services, Winlogon, Scheduled Tasks)

## [bulk_extractor](https://github.com/simsong/bulk_extractor)
- scans and extracts structured information (ex. emails, CCN, JPEGs, JSON snippets) without parsing file system structures
## [Autopsy](https://www.autopsy.com/download/)
- start a case
- select data source (evidence already acquired)
- analyze artifacts (installed programs, metadata, recent documents, run programs, shellbags, web accounts, cookies, history)

![Autopsy](../media/autopsy.png)

## Timeline building

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
[+] [Windows Registry Forensics](https://www.cybertriage.com/blog/windows-registry-forensics-cheat-sheet-2025/)<br />
[+] []()<br />
[+] []()<br />
[+] []()<br />
[+] []()<br />
[+] []()<br />
