# Disk Analysis

After data has been extracted and measures taken to maintain integrity, we focus on initial event analysis. <br />
Early triage can offer indication of the type of incident investigated (ransomware, insider threat, cryptojacking, etc), based on which we look for obvious IoCs.

At this stage:

- high potential for false positives
- include anything that looks suspicious and rule out later
- analyze system and user activity artifacts, including logs
- narrow down to timeframe of suspicious activity

The disk image used in this class: [download here](https://cfreds.nist.gov/all/JeanMiguel%2FUTFPR/Linuxforensicsscenariosimulatedattackonacompanyserver) from the [Computer Forensics Reference Dataset](https://cfreds.nist.gov/).

## Looking for evil

Attackers leave breadcrumbs everywhere: program installation, execution, file modification, user account usage.<br />
Investigators are looking for anything out of the ordinary. <br />
This implies taking time to learn what normal looks like.<br />
Types of data on disk:

- file system metadata
- allocated files
- deleted files
- unallocated space and slack space

### Timeline analysis

Timeline analysis serves in piecing the breadcrumbs together into a coherent record of the intrusion.<br />
Timelines are a guide to evidence, not evidence themselves.<br />

#### On Unix

We collect raw timestamp data into a bodyfile.
``` bash
# use fls from TSK
# -r recursive
# -m / display files in time machine format, the string given will be prepended to the filenames as the mounting point
$ sudo fls -r -m / /dev/sda1 > bodyfile.txt
$ ls -lh bodyfile.txt
-rw-rw-r-- 1 sss sss 26M Jun 11 09:34 bodyfile.txt
$ head bodyfile.txt
[..]
0|/var/lib/dpkg/info/samba-libs:amd64.list|552740|r/rrw-r--r--|0|0|8812|1781087381|1780559162|1780559162|1780559162
[..]
# or use the UAC output (the ir_triage profile collects a bodyfile by default)
$ sudo ./uac -p ir_triage /tmp
$ ls -lf /tmp/bodyfile/bodyfile.txt

```

The bodyfile contains: `MD5|name|inode|mode_as_string|UID|GID|size|atime|mtime|ctime|crtime`

- `MD5` (0 because fls does not calculate hashes)
- filename with the mount point appended (that we specified with `-m /`)
- inode number
- file type and permissions (d for directory, r for regular file, l symlink, c character device, b block device)
- user ID that owns the file
- group ID that owns the file
- file size in bytes
- atime last access time (unix epoch)
- mtime last content modification time
- ctime last metadata modification time
- crtime creation time (or birth) if the filesystem supports that

We generate a csv timeline with `mactime` using the bodyfile as input.
``` bash
$ mactime -b bodyfile.txt -d > timeline.csv
```

We analyze the csv timeline with Timeline Explorer from EZ Tools.

``` csv
Line	Tag	Timestamp	macb	Meta	File Name	File Size
388074	Unchecked	2026-06-04 10:46:02	m.cb	552740	/var/lib/dpkg/info/samba-libs:amd64.list	8812
[..]
583881	Unchecked	2026-06-10 13:29:41	.a..	552740	/var/lib/dpkg/info/samba-libs:amd64.list	8812
[..]
```

Note the example file shown above has one entry in the `bodyfile` but shows as two entries in the timeline.

That is because `mactime` builds a chronological timeline, and the access time (seen as .a..) for this file differred from the other three (seen as m.cb).

The bodyfile shows unique entries per file, and the timeline shows unique entries per time per file.


- on Windows:
- extract the $MFT table with `mftecmd.exe`
- visualize with Timeline Explorer from EZ Tools

### Time normalization

- make sure all timestamps are converted to UTC before analysis starts
- multiple timezones in between physical or cloud investigated devices can slow analysis

### File system metadata

- inodes on Unix
  - displays inode number, file type, permissions, owner uid/guid, file size, timestamps, block pointers but not the filename
- $MFT file on Windows
  - displays FILE record header, $STANDARD_INFORMATION, $FILE_NAME, $DATA

![mft](../media/mft-te.png)

### Evidence of execution

- auditd logs, `cron` logs, shell history, systemd journal
- Prefetch files, SRUM, Registry (UserAssist, ShimCache / AppCompatCache, AmCache, RunMRU)

### Evidence of past file presence

- auditd logs, shell history
- USN Journal, Windows Search Index

### Log analysis

- unix logs
  - found under /var/log by convention, but can be placed anywhere
  - usually simple text files (easy for attackers to edit or remove)
  - format varies widely (starts with timestamp, hostname, process name, pid, and the rest is left to the developers)
    - syslog - primary logging service that routes logs to different destinations (including over the network)
    - /var/log/wtmp - user logins and system reboots, read with `last -if <logfile>`
    - /var/log/btmp - failed logins, not enabled by default, read with `lastb -if <logfile>`
    - /var/log/lastlog - last login info for each user, read with `lastlog`
    - web server logs - often document initial compromise
- windows event logs
  - security logs
    - eventID 4624 and 4634 - logon and logoff
    - eventID 4625 - failed logon
    - eventID 4672 - privilege escalation
    - eventID 4688 - new process created
  - application logs
  - system logs
  - sysmon event logs (optional, will be integrated in Windows 11 and Server 2025, but off by default)
    - eventID 1 - process creation
    - eventID 8 - CreateRemoteThread (indicator of process injection)
    - eventID 10 - process access (indicator of post-exploitation, credential dumping, process injection)

### App & user activity artifacts

- authentication logs
- Registry (RecentDocs, OpenSaveMRU, ShellBags)

### Command history

- bash_history - standard Unix shell command history
  - simple text file, can be easily deleted, more info [here](https://www.youtube.com/watch?v=wv1xqOV2RyE)
- cmd history
- powershell history

### Browser artifacts

- browsing history, search, cookies, credentials, bookmarks, autofill
  - Firefox or Chromium on Unix
  - Chrome or Firefox on Windows
  - Safari on macOS

### Persistence

- rootkits (Loadable Kernel Modules)
- scheduled tasks
- service start-up scripts
- account modification (new admin accounts, new ssh authorized_keys, enhanced `sudo` privileges uid 0)
- Registry (Run/RunOnce Keys, Windows Services, Winlogon, Scheduled Tasks)

## [Autopsy](https://www.autopsy.com/download/)

- start a case
- select data source (evidence already acquired)
- analyze artifacts (installed programs, metadata, recent documents, run programs, shellbags, web accounts, cookies, history)

![Autopsy](../media/autopsy.png)

## [bulk_extractor](https://github.com/simsong/bulk_extractor)

- scans and extracts structured information (ex. emails, CCN, JPEGs, JSON snippets) without parsing file system structures

## Summary

- high volume data, high potential for false positives
- learn what normal looks like, look for outliers
- convert all timestamps to UTC
- seek evidence of execution, past file existence
- analyze logs, user activity artifacts, persistence

## Drills

### Challenge 1

Description

### Challenge 2

Description

### Challenge 3

Description

## Further reading

[+] [Windows Registry Forensics](https://www.cybertriage.com/blog/windows-registry-forensics-cheat-sheet-2025/)<br />
[+] [Windows Event IDs](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/) <br />
[+] [Body file](https://wiki.sleuthkit.org/index.php?title=Body_file)<br />
[+] [You don't know jack about bash history](https://www.youtube.com/watch?v=wv1xqOV2RyE)<br />
