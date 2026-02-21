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

### Dump suspicious processes and drivers

## Volatility
Volatility and Volatility3 are open source memory forensics frameworks. 

The difference between them lies in capability and usage:
- `Volatility` or `Volatility2` is the original version, with multiple plugins and functionality
- `Volatility3` is the newer version, with most of the functionality of 2 ported, but not all, yet

We will use Volatility3, and resort to Volatility2 only in case of missing functionality.
### Memory scan with YARA

## Strings
Unix utility used to extract strings from memory. 

`strings memory.dmp` # default for ASCII strings

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
