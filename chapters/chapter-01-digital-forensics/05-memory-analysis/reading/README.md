# Memory analysis
A wealth of evidence resides in the memory of the analyzed system, which can be used to correlate with other evidence, such as network or disk activity. <br />
When the system is shutdown, the random access memory (RAM) data is lost.

## Memory artifacts
- network connections
  - IP and port source and destination, state of the connection, protocols
- running processes
  - each process has its own memory space and is tracked via process ids (PID)
- loaded DLLs (Dynamic Link Libraries)
- handles
  - used by processes to interact with system resources (files, registry keys, etc.)
- loaded drivers
- command history
- open registry keys 
### Identify rogue processes

- process name (disguised to mimic legitimate processes)
- process path (executed from illegitimate source)
- suspicious ppid-pid (for example notepad.exe spawning cmd.exe)

### Identify library loading and handles
- examine DLLs associated with the rogue process

### Identify network artifacts
- c2 beacons

### Identify code injection
- process following
- unmapped sections of memory

### Identify persistence
- signs of rootkits

### Dump suspicious processes and drivers

## Volatility

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

