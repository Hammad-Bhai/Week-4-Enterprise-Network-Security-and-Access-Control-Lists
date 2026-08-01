Week 4 Enterprise Network Security & Access Control Lists
===================

Author Information:
-------------------
Name: Hammad Zia

Reg No: NETB01-4259 

Domain: Network Administration

Organization: IT-Simplera Solutions

Date of Submission: 24th July, 2026

------------------------------------------------------------------------

## 📌 Project Overview
This repository contains the complete implementation, configuration files, and verification documentation for an enterprise network security baseline engineered in **GNS3**. Developed as part of the **Network Administration Internship Program at IT-SIMPLERA Solutions**, the project focuses on protecting network resources through Inter-VLAN access controls, encrypted remote management, and Layer 2 infrastructure hardening.

---

## 🔒 Security Implementations

### 1. Inter-VLAN Access Control Lists (ACLs)
* **Unidirectional Policy:** Configured Extended Named ACLs to allow IT (`192.168.33.0/24`) full reachability to departmental VLANs while denying unauthorized inbound probes.
* **ICMP Asymmetric Inspection:** Explicitly permitted `echo-reply` messages to ensure stateful two-way ping return traffic functions for IT-initiated connections.

### 2. Device Hardening & Management (SSH v2)
* Disabled plaintext Telnet on VTY lines (`transport input ssh`).
* Generated 1024-bit RSA keys and enforced **SSH Version 2** for all administrative connections.
* Configured local AAA authentication (`username admin privilege 15 secret admin`).

### 3. Layer 2 Infrastructure Defense
* **Port Security:** Bound authorized MAC addresses to switch ports with `restrict` violation actions.
* **DHCP Snooping:** Filtered rogue DHCP offers and built dynamic binding tables.
* **STP Protection:** Deployed `PortFast`, `BPDU Guard`, and `Root Guard` to block unauthorized switch insertions and root bridge spoofing.

---

## 📂 Repository Contents

```text
├── Report.pdf               # Full 10+ Page Technical Documentation
├── GNS3 Project/            # GNS3 Project Workspace Files
├── Configuration Files/     # Running Configs (R1, R2, SW1-SW4)
└── Screenshots/             # High-Resolution Verification Outputs
```

---

## Key Learning Outcomes & Challenges Solved

### 1. Stateless ACL Traffic Handling
* **Challenge:** Deploying a standard `deny ip` rule from departmental VLANs (Sales/Finance) to IT (`VLAN 30`) blocked return ping traffic initiated by IT hosts.
* **Resolution:** Learned that Cisco IOS ACLs are inherently stateless. By placing `permit icmp <subnet> <wildcard> 192.168.33.0 0.0.0.255 echo-reply` above the blanket `deny` statement, IT hosts could successfully receive return ICMP Echo Replies while untrusted subnets remained blocked from initiating new connections toward IT.

### 2. GNS3 Terminal Client Adjustments
* **Challenge:** Default VPCS nodes in GNS3 lack built-in SSH binaries, making direct command-line SSH verification impossible from host PCs.
* **Resolution:** Provisioned a dedicated Management SVI on Cisco Switch `SW4` (HR - `VLAN 20`) with IP `192.168.20.250` to initiate and test encrypted SSH management sessions to core routers without modifying the core topology.

---

## 🚀 How to Run / Replicate in GNS3

1. **Import Project:** Open GNS3 and select `File -> Import portable project`, then select the `.gns3project` file from the `GNS3 Project/` directory.
2. **Launch Nodes:** Power on core routers (`R1`, `R2`) followed by access switches (`SW1` through `SW4` and ‘DSW1’ and ‘DSW2’).
3. **Load Configurations:** If running from scratch, apply the startup scripts located in the `Configuration Files/` folder to each respective device via console access.
4. **Test & Verify:** Use the CLI commands provided in the Verification section to test access controls, SSH sessions, and Layer 2 port security states.

---

## 🤝 Acknowledgments & References

* **Program:** Network Administration Internship Program
* **Organization:** IT-SIMPLERA Solutions
* **Supervisor:** Jawad Qayum (Senior Network Administrator)
* **Documentation Standards:** Cisco IOS Security Configuration Guide & IEEE 802.1Q Specifications

---
