# Memory analysis
A wealth of evidence resides in the random access memory (RAM) of the analyzed system, which can be used to correlate with other evidence, such as network or disk activity. <br />
When the system is shutdown, the memory data is lost.

The memory image used in this class: [download here](TODO)

The main focus of memory analysis is finding malicious processes or executables to extract and examine further.

## Memory artifacts
- network connections
  - IP and port source and destination, state of the connection, protocols
- running processes
  - each process has its own memory space and is tracked via process ids (PID)
- loaded DLLs (Dynamic Link Libraries)
- loaded drivers
- handles
  - used by processes to interact with system resources (files, registry keys, etc.)
- command history
- open registry keys
- credentials
  - session tokens, plaintext app passwords, encryption keys

### Identify rogue processes
- process name (disguised to mimic legitimate processes)
- process path (executed from illegitimate source)
- suspicious ppid-pid (for example notepad.exe spawning cmd.exe)

### Identify library loading and handles
- examine DLLs associated with the rogue process
- look for unfamiliar DLL names or paths (ex: Temp or AppData are non standard DLL locations)
- look for DLLs with timestamps inconsistent with OS installation date
- use `windows.memmap` volatility plugin to identify DLLs in memory but not on disk (DLL reflection)

### Identify network artifacts
- connections to suspicious external hosts: Command and Control (C2) beacons
- extract PID and process name initiating the connection
- analyze PPID to confirm or deny legitimacy
- check associated objects: DLLs, handles, reg keys

### Identify code injection
- process following
- unmapped sections of memory

### Identify persistence
- signs of rootkits
- signs of malicious drivers

### Dump suspicious files from process memory
- dump files from process memory space to investigate further

## Volatility
Volatility and Volatility3 are open source memory forensics frameworks. They can run on any common OS supporting Python.

The difference between them lies in capability and usage:
- `Volatility` or `Volatility2` is the original version developed in python2, with multiple plugins and functionality
- `Volatility3` is the newer version developed in python3, with most of the functionality of 2 ported, but not all, yet

We will use Volatility3, and resort to Volatility2 only in case of missing functionality.

`python3 vol.py -f memory.dmp windows.info` # display memory image information

`python3 vol.py -f memory.dmp windows.psscan` # list all processes

`python3 vol.py -f memory.dmp windows.pstree` # list process tree

`python3 vol.py -f memory.dmp windows.netscan` # scan for network objects

`python3 vol.py -f memory.dmp windows.dlllist --pid 1337` # list dlls loaded by PID

`python3 vol.py -f memory.dmp windows.memmap` # list all memory mapped regions

`python3 vol.py -f memory.dmp windows.handles --pid 1337` # list handles opened by PID

`python3 vol.py -f memory.dmp windows.ldrmodules --pid 1337` # list modules

`python3 vol.py -f memory.dmp windows.cmdline` # show command history

`python3 vol.py -f memory.dmp windows.malfind` # scan for rogue activity

`python3 vol.py -f memory.dmp windows.registry.hivelist` # list registry hives mapped in memory, based on OS structures

`python3 vol.py -f memory.dmp windows.registry.hivescan` # scan raw memory for registry hives, based on hive signatures (can find hives not loaded in OS structures, or corrupted hives)

`python3 vol.py -f memory.dmp windows.dumpfiles --pid 1337 -o ~/outputpid1337/` # extract files from process memory

### Memory scan with YARA
`python3 vol.py -f memory.dmp windows.yarascan --yara-file=rule.yar`

## Strings
Unix utility used to extract strings from memory. 

`strings memory.dmp | grep [pattern|regex]` # default for ASCII strings, grep a specific pattern or regex

`strings memory.dmp -e l ` # for windows UTF-16LE strings
- -e encoding
- l for Windows Unicode UTF-16LE

See `man strings` for other encoding options.

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
[+] Digital Forensics and Incident Response, chapter 10. Analyzing System Memory, by Gerard Johansen<br />
[+][SANS Memory Forensics Cheatsheet](https://www.sans.org/posters/memory-forensics)<br />
[+][Volatility3](https://github.com/volatilityfoundation/volatility3)<br />
[+][Volatility](https://github.com/volatilityfoundation/volatility)<br />
[+][]()<br />
[+][]()<br />
[+][]()<br />
[+][]()<br />
[+][]()<br />
