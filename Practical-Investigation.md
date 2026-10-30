<img width="1899" height="873" alt="image" src="https://github.com/user-attachments/assets/689a0836-085f-4a05-8bed-446986dcab71" /># Practical Investigation

## Sample 1 - Hash Value

### Scenario

As you can see here, Sphinx will conduct threat simulation and detection engineering tests. From here, I will open the provided URL.

<img width="1919" height="829" alt="Sphinx threat simulation scenario" src="https://github.com/user-attachments/assets/64d9c2b1-bc9d-415e-a889-936a1e63281d" />

---

### Malware Sample

From the image above, there is a file attached that we could scan with Malware Sandbox.

<img width="1918" height="833" alt="Malware sample attachment" src="https://github.com/user-attachments/assets/2c4329a3-43f0-4a41-882f-3884c62fc69d" />

<img width="1912" height="472" alt="Malware sample details" src="https://github.com/user-attachments/assets/1161525d-8c86-4b08-a1d4-1040965b4d6a" />

---

### Analysis

Submit for analysis to scan the malware and gather the evidence contained.

<img width="1919" height="930" alt="Malware Sandbox analysis results" src="https://github.com/user-attachments/assets/b744eaa7-0ece-425f-82b1-da32cc02cc7d" />

---

### Pyramid of Pain

Following the pyramid of pain methodology, this technique belongs to Hash Values (lvl easy).

A file hash, such as MD5 or SHA-256, is a fingerprint derived from file contents. It can identify an exact known sample, but changing the file bytes generally changes the hash. An attacker may evade a hash-only block by repacking or modifying the sample.

---

### Detection and Prevention

I will now submit a hash value to the blocklist.

**Navigation:** `Manage Hashes` → `Submit Hash`  
**Algorithm:** `MD5`  
**Hash:** `cbda8ae000aa9cbe7c8b982bae006c2a`

<img width="1909" height="688" alt="MD5 hash blocklist" src="https://github.com/user-attachments/assets/c8b0d3b1-5599-4d5f-9aaa-210439155ba8" />

---

After that, I will get my flag and to the next challenge. 
Flag: `THM{f3cbf08151a11a6a331db9c6cf5f4fe4}`

---

### What I Learned

Attackers can modify metadata, append random junk data, or recompile their malware source code in seconds. Not only that, adding a simple space or using automated scripts can also re-hash and repackage malware instantly, and blocking an old hash will not stop them from doing it again!

---

## Sample 2

### Scenario

For sample2.exe, I will do the same step by uploading it to the Malware Sandbox.

---
### Malware Sample

<img width="1870" height="825" alt="image" src="https://github.com/user-attachments/assets/0a6f3ecf-7904-4e5e-a7ba-e10cbf3d8d9a" />
<img width="1220" height="604" alt="image" src="https://github.com/user-attachments/assets/46df0aa5-5322-42c5-8255-3d8366df14fe" />

---

### Analysis

On the network activity, there is: 

**1 HTTP(S) requests** 

**3 TCP/UDP connections**

**0 DNS requests**

**0 Threats reported by the sandbox**

Let's look at HTTP requests and connections

### HTTP Requests

| PID | Process | Method | IP | URL |
|---|---|---|---|---|
| 1927 | `sample2.exe` | GET | `154.35.10.113:4444` | `http://154.35.10.113:4444/uvLk8YI32` |


### Connections

| PID | Process | IP | Domain | ASN |
|---|---|---|---|---|
| 1927 | `sample2.exe` | `154.35.10.113:4444` | - | Intrabuzz Hosting Limited |
| 1927 | `sample2.exe` | `40.97.128.3:443` | - | Microsoft Corporation |
| 1927 | `sample2.exe` | `40.97.128.4:443` | - | Microsoft Corporation |


### Pyramid of Pain

For this pyramid of pain methodology, I will classify it as IP Address (lvl easy). It was classified as easy because attackers/threat actors could rotate these quickly using proxy services, fast-flux networks, or new ISP leases. Therefore, blocking an IP can disrupt communication temporarily, but attackers may migrate to another address. 

### Detection and Prevention

As you can see below, I denied an outbound connection to a suspicious destination IP. Why? An outbound connection indicates that a local process is communicating with an external destination. In this lab, the sandbox associated `sample2.exe` with the destination `154.35.10.113:4444`, so I chose an egress rule to block traffic leaving the environment for that destination. The connection alone does not prove C2 activity; its context within the simulation makes it the indicator selected for containment. 

### Evidence

<img width="1899" height="873" alt="image" src="https://github.com/user-attachments/assets/2d017ff9-8708-4423-b7c0-2e6d1f5fb692" />

### What I Learned

I've learned that firewall rules are not only about allowing or denying traffic between internal and external systems, but can also be used to block suspicious outbound connections to known malicious IP addresses. In this case, choosing an egress rule made sense because `sample2.exe` was trying to communicate with an external destination.

What also stood out to me is that IP-based detection is still easy for attackers to work around. They can rotate infrastructure, switch to another proxy or hosting provider, or simply move their command-and-control server to a new address. So while blocking the IP is useful, I learned that I should not rely on the IP alone and should keep looking for stronger indicators higher up the Pyramid of Pain.

---

## Sample 3 - Domain Name
---

<img width="1274" height="622" alt="image" src="https://github.com/user-attachments/assets/bbe61eb5-d051-433d-9bcd-035c727dcee4" />

---

### Scenario

Sphinx reported that the previous IP-based block had disrupted the malware, but that the simulated attacker could move to another public IP address. The next task was to investigate `sample3.exe` and identify a more durable network indicator.

### Analysis

The PicoSecure malware sandbox reported `sample3.exe` as a Windows executable tagged `Trojan.Metasploit.A`. Its behavior summary flagged Metasploit activity, unusual network connections, and executable downloads. These are sandbox findings, not independent proof of every stage of compromise.

| Evidence | Observation |
|---|---|
| HTTP requests | 2 |
| TCP/UDP connections | 4 |
| DNS requests | 2 |
| Reported threats counter | 0 |
| Suspicious domain | `emudyn.bresonicz.info` |
| Resolved IP | `62.123.140.9` |
| Process | `sample3.exe` (PID 1021) |
| Additional process | `backdoor.exe` (PID 2712) |

The HTTP request log showed two GET requests by `sample3.exe`:

- `http://emudyn.bresonicz.info:1337/kzn293la`
- `http://emudyn.bresonicz.info/backdoor.exe`

The connection table also linked `backdoor.exe` to `62.123.140.9:80`. A connection to `services.microsoft.com` was listed separately; the evidence does not establish that Microsoft infrastructure was malicious.

**Reasoning:** The domain appeared in DNS resolution, HTTP requests, and connection records associated with the simulated malware. The executable download behavior strengthened the case for blocking this destination within the exercise. The HTTP logs alone do not establish the contents of the downloaded file or prove successful command execution.

### Pyramid of Pain

**Level: Domain Names.** Unlike a single IP address, a domain may remain consistent even when its backing IP changes. Blocking it can disrupt the attacker's ability to reuse that hostname, although attackers can register or switch to another domain. DNS blocking can also be bypassed by direct-IP communication or alternative name resolution.

### Detection and Prevention

In **IOC Management → DNS Rule Manager**, I created a rule with these settings:

| Field | Value |
|---|---|
| Rule name | `Backdoor domain` |
| Category | `Other` |
| Domain name | `emudyn.bresonicz.info` |
| Action | `Deny` |

The application displayed a success message confirming that the DNS rule prevented `sample3.exe` from connecting to the tester's command-and-control server. This confirms the outcome in the simulated environment, not a guarantee against all variants of the malware.

**Flag:** `THM{4eca9e2f61a19ecd5df34c788e7dce16}`

### Evidence

---

<img width="1220" height="759" alt="image" src="https://github.com/user-attachments/assets/ae59f8b3-3c4a-49a5-9d32-dce5b134f840" />
<img width="1218" height="592" alt="image" src="https://github.com/user-attachments/assets/bb539b96-f5dd-446e-9c33-27194034bfbb" />
<img width="1205" height="214" alt="image" src="https://github.com/user-attachments/assets/90a8b8dd-61a6-451e-bd08-d2dc05aa653b" />

---

<img width="1916" height="887" alt="image" src="https://github.com/user-attachments/assets/20811f68-5672-4a68-8c92-771e1ad0bd1d" />

---


### What I Learned

A malicious destination may be represented by both an IP address and a domain name. IP blocking can help immediately, but attackers can move their server to another IP. Investigating DNS, HTTP, and process evidence together can reveal a hostname worth blocking. I also learned to separate the sandbox's detection labels from behavior I can directly verify, and to confirm that a defensive rule actually took effect.

---

## Sample 4 - Host Artifacts
---

<img width="1228" height="600" alt="image" src="https://github.com/user-attachments/assets/82ca5973-590c-438a-a68b-b8d70400ddc4" />

---

### Scenario

Sphinx's next challenge focused on artifacts left on the victim's host, rather than only the malware's file hash or network infrastructure. I uploaded `sample4.exe` to the lab's malware sandbox and examined its host activity.

### Analysis

The sandbox's registry activity included a particularly important event:

| Field | Observed value |
|---|---|
| Process | `sample4.exe` |
| Operation | Registry write |
| Key | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection` |
| Value name | `DisableRealtimeMonitoring` |
| Value data | `1` |

Other registry events involved `explorer.exe` writing `EnableBalloonTips` and `notepad.exe` reading `Progid`. The Defender-related write was the priority because it targeted a security control.

**Analyst reasoning:** A write to `DisableRealtimeMonitoring` with data `1` is consistent with an attempt to disable Microsoft Defender's real-time monitoring. This is evidence of attempted **defense evasion**, but the sandbox event alone does not prove Defender was successfully disabled; system protection state and tamper protection would need separate verification.

### Pyramid of Pain

**Level: Host Artifacts.** This detection focuses on an action performed on the endpoint. Even if the attacker changes the file hash, IP address, or domain, a repeat attempt to modify the same Defender registry value could still trigger a host-based detection. An attacker would have to change their behavior or technique to avoid it.

### Detection and Prevention

The appropriate defensive approach is a host-based rule monitoring suspicious writes to the Defender real-time protection registry setting, with attention to the process responsible and the value written. This should be investigated in context to avoid false positives from authorized administration.

**Lab completion:** The Sample 4 flag was obtained. The exact rule configuration and test confirmation were not recorded in the evidence available for this write-up, so they are not claimed here.

**Flag:** `THM{c956f455fc076aea829799c0876ee399}`

### Evidence

<img width="1879" height="838" alt="image" src="https://github.com/user-attachments/assets/b0f2d515-0bec-46b1-88c9-07a28d2afe69" />
<img width="1230" height="550" alt="image" src="https://github.com/user-attachments/assets/8c3384bd-373d-49cb-9973-06795fecc20b" />
<img width="1273" height="672" alt="image" src="https://github.com/user-attachments/assets/7c4432a0-3d54-4d6d-ad34-007bf9bc476d" />

---
<img width="769" height="680" alt="image" src="https://github.com/user-attachments/assets/6eac97ee-f61a-457b-a2da-3ee59d1c45ab" />
<img width="756" height="582" alt="image" src="https://github.com/user-attachments/assets/5fa5a131-3a6c-47d9-9b00-8828e83f94e5" />
<img width="762" height="423" alt="image" src="https://github.com/user-attachments/assets/8f101fac-b51d-486d-b6a2-cbd3d34f5680" />




### What I Learned

Host artifacts reveal what a program does *on the machine*, not just where it communicates. A suspicious registry write can indicate defense evasion and may remain detectable even after network indicators change. I also learned to distinguish an **attempted** security-setting modification from proof that protection was actually disabled.

---

## Sample 5 - Network Artifacts
<img width="1919" height="898" alt="image" src="https://github.com/user-attachments/assets/20324c8a-2e07-4592-b50f-e6d5763f7c94" />


### Scenario

Sphinx explained that the new sample relied on server-side functionality and could change its infrastructure and host artifacts. The task was to identify abnormal network behavior rather than block a particular hash, IP address, or domain. The email provided an attachment named `outgoing_connections.log` containing the victim host's outbound connections.

### Analysis

The log showed repeated connections from `10.10.15.12` to `51.102.10.19` on port `443`, with each recorded connection showing **97 bytes**. Entries occurred at **30-minute intervals**, including 09:00, 09:30, 10:00, and 10:30.

| Observed feature | Evidence |
|---|---|
| Source host | `10.10.15.12` |
| Repeated destination | `51.102.10.19` |
| Destination port in the log | `443` |
| Recorded size | `97 bytes` |
| Interval | `1800 seconds (30 minutes)` |
| Additional activity | Other destinations and varied sizes occurred between the repeated connections |

**Reasoning:** The regular interval and consistent size are consistent with possible **beaconing**. They do not independently prove command-and-control traffic, since legitimate applications can also communicate periodically. The exercise's simulated attacker context makes the pattern worth targeting.

### Pyramid of Pain

**Level: Network Artifacts.** The detection uses recurring communication characteristics rather than one fixed destination. Changing the IP address or port alone would not necessarily avoid this rule; the attacker may need to change the beacon timing or traffic characteristics.

### Detection and Prevention

In the PicoSecure rule builder, I created a network-connection detection rule using the following fields:

| Field | Configured value |
|---|---|
| Remote IP | `Any` |
| Remote Port | `Any` |
| Size (bytes) | `97` |
| Frequency (seconds) | `1800` |
| MITRE ATT&CK tactic | `Command and Control (TA0011)` |

**Validation result: Success.** The lab accepted the rule configuration.

**Implementation note:** Standard Sysmon network connection Event ID 3 does not natively expose payload byte counts or a 30-minute frequency calculation. A real-world equivalent would need supporting network telemetry and/or correlation logic. Broad IP/port matching may also generate false positives.

**Flag:** `THM{46b21c4410e47dc5729ceadef0fc722e}`

### Evidence

<img width="1877" height="853" alt="image" src="https://github.com/user-attachments/assets/95e4281b-01fc-46ec-9e0c-f7feaa23947d" />
<img width="1232" height="817" alt="image" src="https://github.com/user-attachments/assets/56bbd12c-c4b6-40a2-812c-7df1570c688a" />
<img width="1225" height="398" alt="image" src="https://github.com/user-attachments/assets/3f61875a-de71-48f4-9525-10ea679cbb71" />




### What I Learned

I learned how to recognize a possible beaconing pattern by correlating destination, connection size, and timing. Instead of relying on a specific IP or domain, I can build a behavior-focused rule that may remain useful when infrastructure changes. I also need to verify suspicious patterns against legitimate periodic traffic before applying production blocking.

---

## Sample 6 - Tactics, Techniques and Procedures (TTPs)

### Scenario

In the final challenge, Sphinx explained that the previous detection forced changes to their attack tools and methodology. The task was to identify a repeatable behavior from `commands.log`, a record of commands used across earlier samples, and build a detection for that activity.

### Analysis

The log showed a sequence of Windows commands used to inspect the host and its environment:

| Observed command | Investigative significance |
|---|---|
| `dir c:\` and directory listings | Enumerates accessible directories |
| `net localgroup administrator` | Attempts to enumerate local group membership |
| `ver` and `systeminfo` | Collects operating system and host details |
| `ipconfig /all` | Collects network configuration |
| `netstat -ano` | Lists network connections and related process IDs |
| `net start` | Lists started services |

The commands repeatedly appended output using `>>` to `%temp%\exfiltr8.log`. This creates a consolidated local record that could be reviewed or transferred later.

**Analyst reasoning:** The commands show **host and network discovery**, plus local collection of results. The name `exfiltr8.log` suggests a possible exfiltration objective, but the supplied log does not demonstrate that the file was actually sent outside the host. Some commands can also be used legitimately by administrators, so context matters.

### Pyramid of Pain

**Level: Tactics, Techniques and Procedures (TTPs).** The focus is on the attacker's recurring discovery and collection workflow, rather than only a hash or network destination. Changing a workflow can be more costly than changing infrastructure. The lab's specific rule still relies on a literal output filename, so a filename change could evade this implementation; broader behavioral correlation would be stronger.

### Detection and Prevention

I used **IOC Management → Sigma Rule Builder**, selected **Sysmon Event Logs**, then **Process Creation**, and configured:

| Field | Value |
|---|---|
| Process Name | `cmd.exe` |
| CommandLine operator | `Contains` |
| String | `%temp%\exfiltr8.log` |
| ATT&CK tactic selected | `Exfiltration (TA0010)` |

**Validation result: Success.** PicoSecure accepted the rule.

**ATT&CK interpretation:** The chosen lab label was Exfiltration (TA0010), while the observed commands directly support Discovery (TA0007) and collection of output into a local file. No outbound transfer is visible in the provided evidence. A real detection should also account for how command lines are represented in Sysmon logs and whether the literal string is present.

### Evidence

<!-- Sample 6 screenshots will be attached here later. Do not remove existing evidence images in other sections. -->

Pending image uploads: Sphinx email and `commands.log`; Sigma Rule Builder event source; Process Creation selection; configured detection rule and validation result.

### What I Learned

I learned to recognize an attacker's repeatable workflow from a sequence of commands. Multiple discovery commands redirecting results into one file are more informative together than any single command. I also learned to distinguish local collection from confirmed exfiltration and to test a detection rule rather than assuming it works.

