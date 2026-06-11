# Disk Analysis

After data has been extracted and measures taken to maintain integrity, we focus on initial event analysis. <br />
Early triage can offer indication of the type of incident investigated (ransomware, insider threat, cryptojacking, etc), based on which we look for obvious IoCs.

At this stage:

- high potential for false positives
- include anything that looks suspicious and rule out later
- analyze system and user activity artifacts, including logs
- narrow down to timeframe of suspicious activity

## Prerequisites

- a unix forensic vm with
  - TSK
  - nice to have but not mandatory
    - UAC
- a windows forensic vm with
  - Timeline Explorer
  - nice to have but not mandatory
    - KAPE
    - [LogFileParser](https://github.com/jschicht/LogFileParser)

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
Built by the investigator, timelines are a guide to evidence not evidence themselves.<br />
They tell the story of what happened and make a note on the gaps, if any.

Individual logs or artifacts show what happened in isolation, while timelines show how events relate to each other.

Before you begin building a timeline, remember:

- make sure all timestamps are converted to UTC before analysis starts
- multiple timezones in between physical or cloud investigated devices can slow analysis

### File system metadata

#### Unix

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
$ ls -lh /tmp/bodyfile/bodyfile.txt
-rw-r--r-- 1 sss sss 48M Jun 11 10:40 bodyfile.txt
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

That is because `mactime` builds a chronological timeline, and the access time (seen as `.a..` in the macb column) for this file differred from the other three (seen as `m.cb`).

The bodyfile shows unique entries per file, and the timeline shows unique entries per time per file.

#### Windows

The most important artifact showing filesystem metadata is the `$MFT` table.

We extracted the $MFT table with KAPE, using the `MFTECmd.exe` module to parse it into a csv.

``` powershell
PS C:\Users\test1\Documents\KAPE> .\kape.exe --tsource C: --tdest C:\Users\test1\Downloads\kapemfttout --target '$MFT' --module 'MFTECmd_$MFT' --mdest C:\Users\test1\Downloads\kapemftmout
KAPE version 1.3.0.2, Author: Eric Zimmerman, Contact: https://www.kroll.com/kape (kape@kroll.com)

KAPE directory: C:\Users\test1\Documents\KAPE
Command line:   --tsource C: --tdest C:\Users\test1\Downloads\kapemfttout --target $MFT --module MFTECmd_$MFT --mdest C:\Users\test1\Downloads\kapemftmout

System info: Machine name: TEST, 64-bit: True, User: test1 OS: Windows10 (10.0.26200)

Using Target operations
  Creating target destination directory C:\Users\test1\Downloads\kapemfttout
Found 1 targets. Expanding targets to file list...
Found 1 file in 0.032 seconds. Beginning copy...
  Deferring C:\$MFT due to UnauthorizedAccessException...
Deferred file count: 1. Copying locked files...
  Copied deferred file C:\$MFT to C:\Users\test1\Downloads\kapemfttout\C\$MFT. Hashing source file...

Copied 1 out of 1 files in 36.2443 seconds. See C:\Users\test1\Downloads\kapemfttout\2026-06-11T08_23_28_4665615_CopyLog.csv for copy details

Using Module operations
  Setting --msource to C:\Users\test1\Downloads\kapemfttout since --msource was not provided
  Creating module destination directory C:\Users\test1\Downloads\kapemftmout
    Found processor Executable: MFTECmd.exe, Cmd line: -f %sourceFile% --csv %destinationDirectory%, Export: csv, Append: False!
Discovered 1 processor to run
Executing modules with file masks...
  Running MFTECmd.exe: -f "C:\Users\test1\Downloads\kapemfttout\C\$MFT" --csv C:\Users\test1\Downloads\kapemftmout\FileSystem
Executing remaining modules...
Executed 1 processor in 46.8196 seconds

Total execution time: 83.0793 seconds
```

We visualize it with Timeline Explorer from EZ Tools.
``` csv
Line	Tag	Entry Number	Sequence Number	Parent Entry Number	Parent Sequence Number	In Use	Parent Path	File Name	Extension	Is Directory	Has Ads	Is Ads	File Size	Created0x10	Created0x30	Last Modified0x10	Last Modified0x30	Last Record Change0x10	Last Record Change0x30	Last Access0x10	Last Access0x30	Zone Id Contents	Reparse Target	Reference Count	SI<FN	u Sec Zeros	Copied
647	    Unchecked	647	    5	139442	8	  Checked	.\Windows\Prefetch	    PAYLOAD.EXE-C2669AB8.pf	.pf	  Unchecked	Unchecked	Unchecked	7125	  2026-06-10 16:38:49		2026-06-10 16:52:09	2026-06-10 16:38:49	2026-06-11 05:32:46	2026-06-10 16:38:49	2026-06-11 05:52:43	2026-06-10 16:38:49			1	Unchecked	Unchecked	Unchecked
245199	Unchecked	141215	6	171279	1	  Checked	.\Users\test1\Downloads	payload.exe	           .exe	  Unchecked	  Checked	Unchecked	256000	2026-04-19 18:46:15	  2026-06-10 17:07:29	2026-04-19 18:46:15	2026-06-10 17:07:29	2026-06-10 17:07:29	2026-06-10 17:07:29	2026-06-11 06:12:38	2026-06-10 17:07:29			1	Checked	Checked	Unchecked
```

![mft](../media/mft-te.png)

The `$LogFile` keeps a record of changes to the NTFS volume (creations, modifications, renaming, and deletions), so in the event of a crash it can roll back to the previous state.

Though it doesn't keep timestamps for each event, it uses Log Sequence Numbers to help in tracking events over time.

It can be analyzed with `LogFileParser.exe`.

If both the `$MFT` and the `$LogFile` are passed to it, the parser can reconstruct full paths.
Another way is to point the parser to a disk image and it will extract everything it needs.

``` powershell
PS C:\Users\sss\> .\LogFileParser.exe /LogFileFile:'C:\Users\sss\$LogFile' /OutputFormat:csv /Separator:','/OutputPath:'C:\Users\sss\'
NTFS $LogFile Parser 2.0.0.53
Timestamps presented in UTC: 0.00
Output directory: C:\Users\sss\LogFile_2026-06-11_13-51-30
NTFS $LogFile Parser 2.0.0.53
Using $LogFile: C:\Users\sss\$LogFile
Unicode: 0
Using precision separator: .
LSNs for verbose output:
LSN Validation level: 0.1 (10 %)
Timestamps presented in UTC: 0.00
Sectors per cluster: 8
DataRun reconstruct configuration: 0
Rebuild broken header for transactions found in slack: False
32-bit: False
DoExtractResidentUpdates: 0
FragmentMode: 0
SkipFixups: 0
BrokenLogFile: 0
Using DateTime format: 6
Using timestamp precision: 3
------------------- END CONFIGURATION -----------------------
[..]
```

We analyze the csv output with Timeline Explorer from EZ Tools.

Note that by default, the separator is `|` so a comma `,` must be specified in order for the Timeline Explorer to parse it correctly.
Alternatively, it can be opened with LibreOffice.

### Evidence of execution

#### Unix
- auditd logs
- `cron` logs
- shell history
- systemd journal

#### Windows
- Prefetch files
- SRUM
- Registry (UserAssist, ShimCache / AppCompatCache, AmCache, RunMRU)

### Evidence of past file presence

#### Unix
- auditd logs
- shell history

#### Windows
- cmd history, PowerShell history
- USN Journal, Windows Search Index

### Log analysis

#### Unix
- found under /var/log by convention, but can be placed anywhere
- usually simple text files (easy for attackers to edit or remove)
- format varies widely (starts with timestamp, hostname, process name, pid, and the rest is left to the developers)
  - syslog - primary logging service that routes logs to different destinations (including over the network)
  - /var/log/wtmp - user logins and system reboots, read with `last -if <logfile>`
  - /var/log/btmp - failed logins, not enabled by default, read with `lastb -if <logfile>`
  - /var/log/lastlog - last login info for each user, read with `lastlog`
  - web server logs - often document initial compromise

#### Windows
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

#### Unix
- authentication logs

#### Windows
- Registry (RecentDocs, OpenSaveMRU, ShellBags)

### Command history

#### Unix

- bash_history - standard Unix shell command history
  - simple text file, can be easily deleted, more info [here](https://www.youtube.com/watch?v=wv1xqOV2RyE)

#### Windows
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
[+] [$LogFile](https://forensafe.com/blogs/windowslogfile.html)<br />
[+] [Forensic Timelines](https://www.thedfirspot.com/post/from-chaos-to-chronology-the-power-of-forensic-timelines)<br />
[+] [You don't know jack about bash history](https://www.youtube.com/watch?v=wv1xqOV2RyE)<br />
