Below is a **focused, interview-ready Q&A set** for **Networking + Cybersecurity** roles (SOC Analyst, Security Analyst, Junior Pentester). Answers are concise but technically correct—aim to speak them in your own words.

---

# 🎯 Core Networking (Must-Know)

### 1) What is the OSI model?

**Answer:**
A 7-layer conceptual model that standardizes network communication: Physical, Data Link, Network, Transport, Session, Presentation, Application. It helps isolate issues (e.g., cabling vs. routing vs. application errors).

---

### 2) TCP vs UDP?

**Answer:**

* **TCP**: connection-oriented, reliable (SYN/SYN-ACK/ACK), ordered delivery, congestion control.
* **UDP**: connectionless, faster, no guarantees (used by DNS, VoIP, streaming).

---

### 3) What is a port?

**Answer:**
A logical endpoint identifying a service on a host (e.g., 80 HTTP, 443 HTTPS, 22 SSH). Used by the Transport layer to multiplex traffic.

---

### 4) What is subnetting?

**Answer:**
Dividing a network into smaller segments using a subnet mask (CIDR like `/24`) to improve routing efficiency, security segmentation, and IP utilization.

---

### 5) What is DNS and why is it important?

**Answer:**
Resolves domain names to IPs. Critical for connectivity and often abused (e.g., DNS tunneling, domain generation algorithms).

---

### 6) What is ARP?

**Answer:**
Maps IP addresses to MAC addresses within a LAN. Vulnerable to ARP spoofing/poisoning.

---

### 7) What is NAT?

**Answer:**
Translates private IPs to public IPs (and back). Conserves IP space and hides internal addressing; can complicate inbound connectivity.

---

# 🔐 Cybersecurity Fundamentals

### 8) What is the CIA triad?

**Answer:**

* **Confidentiality** (no unauthorized access)
* **Integrity** (no unauthorized modification)
* **Availability** (systems/data accessible when needed)

---

### 9) What is a vulnerability vs exploit?

**Answer:**

* **Vulnerability**: a weakness
* **Exploit**: code/technique that takes advantage of it

---

### 10) What is a firewall?

**Answer:**
Enforces traffic policy (allow/deny) based on rules. Types include stateless, stateful, and next-gen (application-aware).

---

### 11) IDS vs IPS?

**Answer:**

* **IDS**: detects and alerts
* **IPS**: detects and blocks inline

---

### 12) What is a VPN?

**Answer:**
Encrypted tunnel over an untrusted network (e.g., IPsec/SSL) for secure remote access or site-to-site connectivity.

---

# 🔍 Tools & Practical Knowledge

### 13) What is Nmap used for?

**Answer:**
Host discovery, port scanning, service/version detection, OS fingerprinting, and scripting (NSE) for vuln checks.

---

### 14) What is Wireshark?

**Answer:**
A packet analyzer to capture and inspect network traffic at a granular level (headers, payloads), useful for troubleshooting and forensics.

---

### 15) What is Metasploit Framework?

**Answer:**
A framework for developing/executing exploits and payloads, plus post-exploitation (Meterpreter).

---

### 16) What is Netcat?

**Answer:**
A “Swiss army knife” for TCP/UDP—used for banner grabbing, port listening, and simple reverse shells.

---

# ⚔️ Attack Concepts

### 17) What is a DDoS attack?

**Answer:**
Distributed traffic floods a target to exhaust resources and deny service. Mitigations include rate limiting, CDNs, scrubbing centers.

---

### 18) What is a Man-in-the-Middle (MitM)?

**Answer:**
Attacker intercepts/modifies communication (e.g., via ARP spoofing, rogue APs). Mitigate with TLS, HSTS, cert pinning.

---

### 19) What is phishing?

**Answer:**
Social engineering to trick users into revealing credentials or executing malicious actions.

---

### 20) What is SQL Injection?

**Answer:**
Injection of malicious SQL into inputs to manipulate backend queries. Prevent with parameterized queries and input validation.

---

# 🛡️ Defensive / SOC-Oriented

### 21) What is SIEM?

**Answer:**
Centralized logging and correlation system that aggregates events and generates alerts (e.g., Splunk/ELK).

---

### 22) What is a false positive?

**Answer:**
An alert that flags benign activity as malicious. Tuning rules reduces noise.

---

### 23) How do you detect a port scan?

**Answer:**
Look for many connection attempts across ports/hosts in a short time; IDS signatures or flow logs reveal patterns (SYN bursts, sequential ports).

---

### 24) What is log analysis?

**Answer:**
Reviewing system/app/network logs to detect anomalies, investigate incidents, and establish timelines.

---

# 💻 Practical / Scenario Questions

### 25) How would you approach a penetration test?

**Answer:**
**Recon → Scan → Enumerate → Exploit → Post-exploit → Report.** Validate scope/authorization first, prioritize risks, and document findings with remediation.

---

### 26) You find port 22 open—what next?

**Answer:**
Enumerate SSH version, check for weak credentials (authorized methods), attempt key/credential attacks in scope, review configs.

---

### 27) Suspicious traffic—what do you do?

**Answer:**
Capture with Wireshark, identify source/destination, protocol, indicators (beacons, unusual DNS), correlate with logs, contain if confirmed.

---

### 28) Difference between symmetric and asymmetric encryption?

**Answer:**

* **Symmetric**: one shared key (fast; e.g., AES)
* **Asymmetric**: key pair (public/private; e.g., RSA) used for key exchange and signatures

---

### 29) What is a reverse shell?

**Answer:**
Target initiates a connection back to the attacker (useful behind NAT/firewalls). Listener runs on attacker side.

---

### 30) Why is networking critical in cybersecurity?

**Answer:**
All attacks/defenses traverse networks—understanding protocols, flows, and anomalies is essential for both offense and defense.

