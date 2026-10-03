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

It is a unique file signatures like MD5 or SHA-256. Why is it easy? Because attackers can change them instantly by recompiling and modifying a single bit of code.

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

**0 Threats**

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

For this pyramid of pain methodology, I will classify it as IP Address (lvl easy). It was classified as easy because attackers/threat actors could rotate these quickly using proxy services, fast-flux networks, or new ISP leases. Therefore, blocking them would also lead them to excitement! 

### Detection and Prevention

As you can see below, I denied an outbound connection to a suspicious destination IP. Why? An outbound connection to a suspicious IP usually indicates that an internal server or device is trying to communicate with a known malicious external destination. Therefore, Egress is a choice I made. 

### Evidence

<img width="1899" height="873" alt="image" src="https://github.com/user-attachments/assets/2d017ff9-8708-4423-b7c0-2e6d1f5fb692" />

### What I Learned

I've learned that not only can we create a firewall rule for internal and external server, but also any suspicious IP address that can be marked as a threat. But because the level is easy, attackers will find a way to change IP addresses in multiple ways they could as I mentioned earlier. Therefore, staying vigilant on this stage is a must. 

---

## Sample 3

### Scenario

<!-- Add your Sample 3 context here. -->

### Analysis

<!-- Add what you analyzed here. -->

### Pyramid of Pain

<!-- Explain which Pyramid of Pain level this sample belongs to and why. -->

### Detection and Prevention

<!-- Explain what you configured or blocked here. -->

### Evidence

<!-- Add your screenshots here. -->

### What I Learned

<!-- Write what Sample 3 taught you here. -->

---

## Sample 4

### Scenario

<!-- Add your Sample 4 context here. -->

### Analysis

<!-- Add what you analyzed here. -->

### Pyramid of Pain

<!-- Explain which Pyramid of Pain level this sample belongs to and why. -->

### Detection and Prevention

<!-- Explain what you configured or blocked here. -->

### Evidence

<!-- Add your screenshots here. -->

### What I Learned

<!-- Write what Sample 4 taught you here. -->

---

## Sample 5

### Scenario

<!-- Add your Sample 5 context here. -->

### Analysis

<!-- Add what you analyzed here. -->

### Pyramid of Pain

<!-- Explain which Pyramid of Pain level this sample belongs to and why. -->

### Detection and Prevention

<!-- Explain what you configured or blocked here. -->

### Evidence

<!-- Add your screenshots here. -->

### What I Learned

<!-- Write what Sample 5 taught you here. -->

---

## Sample 6

### Scenario

<!-- Add your Sample 6 context here. -->

### Analysis

<!-- Add what you analyzed here. -->

### Pyramid of Pain

<!-- Explain which Pyramid of Pain level this sample belongs to and why. -->

### Detection and Prevention

<!-- Explain what you configured or blocked here. -->

### Evidence

<!-- Add your screenshots here. -->

### What I Learned

<!-- Write what Sample 6 taught you here. -->
