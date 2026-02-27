# Investigation: persistence & privesc
Some of the questions we are answering in this case are quire straightforward:
- what did the attacker do?
- were backdoors or persistence mechanisms installed?

In order to successfully establish a persistence mechanism, elevated privileges are usually, but not always, required. <br />
However, if the user that clicked the malicious file is privileged, or the exploited app or service is already running as root, administrator, or another privileged service user, there is no need for privilege escalation.

## Persistence

## Privilege Escalation

## Investigate case 
Disk evidence: [TODO]()<br /> * most persistence is disk based
Memory evidence: [TODO]()<br />

- possible artifacts in memory:
  - WMI persistence: unusual child processes of `wmiprvse.exe`
  - Registry Run keys: `Run` or `RunOnce` loaded in `HKLM` or `HKCU`
  - Scheduled tasks: unusual child of `taskeng.exe` or `svchost.exe`
  - suspicious dll loaded into `explorer.exe`, `svchost.exe`, `winlogon.exe`, `userinit.exe`
- possible artifacts on disk:
  - Scheduled tasks: in `C:\Windows\System32\Tasks` 
  - Registry Run keys: `Run` or `RunOnce` loaded in `HKLM` or `HKCU`
    - _HKLM\Software\Microsoft\Windows\CurrentVersion\Run_
    - _HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce_
    - _HKCU\Software\Microsoft\Windows\CurrentVersion\Run_
    - _HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce_
  - Registry winlogon keys: `Winlogon\Userinit`, `Winlogon\Shell`, `Winlogon\Notify`
  - New service entry in `HKLM\SYSTEM\CurrentControlSet\Services`
  - Malicious startup files for all users and current user
    - `%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup`
    - `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup`


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
[+] []()<br />
[+] []()<br />
[+] []()<br />
[+] []()<br />
