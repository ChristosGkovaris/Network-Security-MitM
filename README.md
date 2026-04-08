# Network-Security-MitM

This project implements a **Man-in-the-Middle (MitM) attack** on the **SSH protocol** using **ARP Cache Poisoning**, featuring real-time packet analysis and credential interception. It was developed as part of the **Security of Computer and Communication Systems** (MYE007) course at the **University of Ioannina**.

---

## TABLE OF CONTENTS
1. [Overview](#overview)  
2. [Features](#features)  
3. [Input Data](#input-data)  
4. [Algorithms Implemented](#algorithms-implemented)  
5. [Implementation Details](#implementation-details)  
6. [Usage](#usage)  
7. [License](#license)  
8. [Contact](#contact)

---

## OVERVIEW

The **Network-Security-MitM** project consists of two primary modules:

### **1. Network Interception (ARP Poisoning)**
A robust networking implementation designed to redirect traffic between two target systems.
- Utilizes **Ettercap** for automated ARP cache poisoning to redirect traffic.
- Manipulates the MAC-to-IP association of the victims to position the attacker as a gateway.
- Supports monitoring of unencrypted control traffic via **Wireshark**.

### **2. Protocol Interception (SSH-MITM Proxy)**
An algorithmic tool for intercepting and decrypting secure communication.
- Produces **plaintext logs** (`shell_session_0.txt`) of captured credentials and session data.
- Includes a proxy service (`ssh-mitm`) that handles independent encrypted connections with the client and server.
- Generates a detailed **Network Bridge** analysis for active packet tracking.

---

## FEATURES

### **Network Security Features**
- ARP Poisoning: Redirects traffic by sending forged ARP replies to targets c1 and c2.
- Packet Forwarding: Enables kernel-level IP forwarding to maintain connectivity between victims.
- Port Redirection: Employs `iptables` to reroute SSH traffic from port 22 to the local proxy port 2222.
- Credential Harvesting: Captures authentication passwords in cleartext by terminating the encrypted tunnel.

### **Attack Mechanics**
- Hybrid Cryptosystem Analysis: Exploits the initial key exchange phase of SSH.
- Traffic Analysis: Uses Wireshark filters (e.g., `arp`, `tcp.port == 22`) to isolate specific attack phases.
- Active Interception: Acts as a transparent proxy, appearing as the server to the client and vice versa.

---

## INPUT DATA

### **Environment Configuration (.zip files)**
- Format: Virtual machine disk images running 64-bit Linux kernel 6.12.57.
- Network: A virtual bridge (`virbr0`) connecting three containers: mye007, c1, and c2.
- Constraints: The virtual environment requires at least 11GB of disk space for proper execution.

### **Network Traces**
- Structure: Packet captures (.pcapng) containing Ethernet frames, IP packets, and TCP segments.
- Usage: Acts as the evidentiary bridge between the automated attack tools and the final verification report.

---

## ALGORITHMS IMPLEMENTED

### **1. Layer 2 Attack Analysis**
- ARP Cache Poisoning: Scanning the network for hosts, grouping targets, and broadcasting malicious ARP advertisements.
- MAC Spoofing Logic: Mapping the attacker's MAC address to the victims' IP addresses in the local ARP table.
- Packet Filtering: Identifying and isolating specific protocols like ARP and SSH from the general traffic stream.

### **2. Traffic Redirection**
- Iptables Manipulation: Creating NAT rules to intercept outbound traffic and redirect it to a local proxy.
- Connection Proxying: Managing two separate encrypted sessions (Key 1 and Key 2) to maintain a transparent connection.
- Log Mapping: Translation of intercepted session data into readable text for credential harvesting.

---

## IMPLEMENTATION DETAILS

- Tools: Pure **Linux Networking Tools** (Ettercap, Wireshark, Iptables) and the **ssh-mitm** service.
- Validation: Successful authentication triggers the creation of log files containing the root password.
- Outputs: Automatically creates `/home/ssh-mitm/log/shell_session_*.txt` containing the harvested credentials.
- Architecture: Modular design separating logic for ARP spoofing, traffic routing, and protocol-specific interception.

---

## USAGE

### **Run the ARP Poisoning Attack**
1. Navigate to the project directory on the `mye007` host.
2. Ensure the virtual bridge (`virbr0`) is active.
3. Run: `ettercap -G &` and select victims c1 and c2 as Group 1 and Group 2.

### **Run the SSH Interceptor**
1. Navigate to the `/root/src/ssh-mitm-master` directory.
2. Ensure the firewall rules are initialized via the provided scripts.
3. Run: `./start.sh` to begin listening for redirected SSH connections.
4. Intercepted data will be generated in `/home/ssh-mitm/log/`.

---

## LICENSE

This project was developed for academic purposes as part of the **MYE007 - Security of Computer and Communication Systems** course at the University of Ioannina.

Final implementation by the project creators.

---

## CONTACT

**Christos-Grigorios Gkovaris** University of Ioannina – Computer Science and Engineering  
GitHub: [ChristosGkovaris](https://github.com/ChristosGkovaris)