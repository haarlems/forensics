# Forensic investigation methodology
[Digital forensics](https://www.nist.gov/digital-evidence) is the field of forensic science concerned with ***retrieving, storing and analyzing*** data that can be useful in investigations. <br />
This includes information from computers, mobile phones, IoT devices, motor vehicles, drones, or the cloud. <br />
**Digital forensic investigators** face challenges such as:
- extracting data from damaged devices;
- locating evidence among vast quantities of data;
- ensuring that their methods capture data **reliably**, without altering it in any way.

The term **DFIR (Digital Forensics and Incident Response)** is often used referring to the broader discipline including incident response.

## Objectives of the [digital forensic process](https://www.unodc.org/cld/en/education/tertiary/cybercrime/module-4/key-issues/standards-and-best-practices-for-digital-forensics.html)
- identification;
- collection;
- analysis;
- reporting;
- preservation.

## Evidence
Main _**types of evidence**_ retrieved and analyzed:
- disk (files, logs, command history, browser artifacts, etc.);
- memory (process, linked libraries, network connections, etc.);
- network capture.

## Chain of custody
Preserving the *integrity* of the evidence (preventing alteration or deletion) is prioritized. <br />
**Chain of custody** is used to track evidence by documenting each person and organization who handles it, the date/time it was collected or transferred, and the purpose of the transfer. <br />
<br />
Example [chain of custody form](https://www.oreilly.com/library/view/implementing-digital-forensic/9780128045015/XHTML/B9780128044544150142/B9780128044544150142.xhtml):
![Example SVG](../media/coc.jpg)
<br />
## Case examples
[Cat's Got Your Files: Lynx Ransomware](https://thedfirreport.com/2025/11/17/cats-got-your-files-lynx-ransomware/)
- initial access: RDP with compromised credentials to internet-exposed system
- internal recon: enumerate virtual infrastructure and file shares using standard windows utilities and [netscan](https://www.softperfect.com/products/networkscanner/)
- lateral movement: to Domain Controller via separate compromised domain account (likely obtained from initial access broker)
- persistence: create new accounts via [dsa.msc](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage-user-accounts-in-windows-server) and add them to privileged groups
- exfil: sensitive files from network shares, 7z compressed via temporary file service temp.sh
- ransomware: delete backup jobs, deploy lynx across multiple backup and file servers via RDP

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
ISO/IEC 27037:2012
[Chain of custody](https://www.cisa.gov/sites/default/files/publications/cisa-insights_chain-of-custody-and-ci-systems_508.pdf)

