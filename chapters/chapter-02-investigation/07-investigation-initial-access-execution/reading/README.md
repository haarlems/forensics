# Investigation: initial access & execution
The most frequently asked questions of the forensic investigator:
- what happened?
  - what type of incident this is (ransomware, data exfil, insider threat, etc.) 
- when did the breach first occur?
  - how was it detected?
  - is the attack ongoing?
  - what is the earliest indicator of compromise? (dwell time)
- how did the attackers get in?
  - initial access (phishing, web app exploit, exposed login portal, etc.)
  - known vulnerabilities exploited?
  - were credentials stolen or bruteforced? MFA bypassed?
- what systems are affected?
  - which endpoints, server, cloud resources are compromised?
  - are backups impacted?
  - are domain controllers affected? did the attacked gain administrative or domain-level access?
  - what data is at risk?
- what did the attacker do?
  - was data accessed, exfiltrated, or altered?
  - were backdoors or persistence mechanisms installed?
  - was lateral movement observed?
  - were logs deleted or tampered with?
- who is responsible?
  - known [threat actor](https://attack.mitre.org/groups/)? ransomware gangs, nation-state activity, hacktivism?
  - insider involvement?
- what must be done immediately?
  - should credentials be reset?
  - should systems be isolated?
  - should law enforcement or regulators be notified?
  - should Incident Response plans be triggered?

While the MITRE ATT&CK Matrix lists Reconnaissance and Resource Development first, this class will focus on Initial Access and Execution.<br />

![MITRE](../media/mitre.png)

## Initial access vectors
- [phishing](https://attack.mitre.org/techniques/T1566/) - the most common
  - targeted or non-targeted
- [valid accounts](https://attack.mitre.org/techniques/T1078/)
  - attackers don't break in, they log in
  - default accounts
  - credential stuffing from previous breach data
- [exploit public-facing application](https://attack.mitre.org/techniques/T1190/)
  - web apps, APIs (see [OWASP Top 10](https://owasp.org/Top10/2025/))
  - SSH, exposed ESXi OpenSLP services, exposed VMware vCenter servers
- [external remote services](https://attack.mitre.org/techniques/T1133/)
  - VPNs, Citrix
  - WinRM, VNC
  - Docker API, Kubernetes API server, kubelet, Kubernetes dashboard
- [trusted relationship](https://attack.mitre.org/techniques/T1199/)
  - third parties with delegated permissions
- [supply chain compromise](https://attack.mitre.org/techniques/T1195/)
  - software dependencies
  - CI/CD pipeline components (ex. Github Actions)
- and others

## Execution
- [exploitation for client execution](https://attack.mitre.org/techniques/T1203/)
  - exploit software vulnerabilities in client applications
  - browser-based, office-applications, 3rd party applications (ex. Adobe Reader)
- [user execution](https://attack.mitre.org/techniques/T1204/)
  - social engineering to execute malicious code or open a malicious document or link
  - enabling remote access tools (RAT)
  - running malicious javascript in the browser that steals session cookies
- [command and scripting interpreter](https://attack.mitre.org/techniques/T1059/)
  - cli interfaces (unix shells, powershell, cmd), python, visual basic, javascript
  - used to execute initial access payloads
- and others

### Known Exploited Vulnerabilities
The [KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) database is a list of CVEs that are confirmed to be exploited in the wild.<br />
- helps narrow down initial access hypotheses: phishing, credential abuse or _exploitation of a vulnerability_?
- vulnerabilities in: edge devices, web apps, email servers, app frameworks
- cross-reference exposed services against KEV CVEs
  - _ex. if the victim has exposed Atlassian servers, worth checking if any new KEVs_
- search for exploitation artifacts tied to specific CVEs

## Investigate case "Breaking Dawn"
Disk evidence: [TODO]()<br />
Memory evidence: [TODO]()<br />

First, we check the memory dump:
- list processes and process tree

Next, the disk image:
- analyze $MFT starting from the breadcrumbs found in memory
- analyze logs (process creation EID 1, process access EID 10)

## Summary
- initial access frequently phishing, stolen credentials to exposed remote services
- execution frequently user execution via social engineering

### Challenge 1
Description
### Challenge 2
Description
### Challenge 3
Description

## Further reading
[+] [MITRE ATT&CK Matrix](https://attack.mitre.org/)<br />
[+] [Threat actor groups](https://attack.mitre.org/groups/)<br />
[+] [OWASP Top 10](https://owasp.org/Top10/2025/)<br />
[+] [Known Exploited Vulnerabilities](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)<br />
[+] []()<br />
[+] []()<br />
[+] []()<br />
[+] []()<br />
[+] []()<br />
