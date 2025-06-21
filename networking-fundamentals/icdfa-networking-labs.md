# ICDFA Internship Lab Portfolio: Networking Fundamentals

**Intern:** Oluwasola Adebayo Godwin
**Student ID:** 2025/INT/8916
**Internship Organization:** International Cybersecurity and Digital Forensic Academy (ICDFA)
**Course:** INT303: Networking Fundamentals
**Platform:** Kali Linux / OWASP BWA

---

## Lab 1: Simulating Network Routing and VLAN Configuration in Linux

### Objectives

* Configure static routing in Linux
* Simulate VLANs using network namespaces
* Perform IP addressing and subnetting
* Test connectivity with ping and traceroute
* Implement firewall rules with iptables
* Monitor traffic with tcpdump

### Tools Used

* `ip route`, `ip netns`, `ping`, `traceroute`, `iptables`, `tcpdump`

### Methodology

**Static Routing:**

```bash
sudo ip route add 192.168.XX.X/24 via 192.168.XX.XXX dev eth0
ping 192.168.25X.XXX
```

**Network Namespaces & VLANs:**

```bash
sudo ip netns add vlan1
sudo ip netns add vlan2
sudo ip link add veth1 type veth peer name veth2
sudo ip link set veth1 netns vlan1
sudo ip link set veth2 netns vlan2
```

**IP Assignment:**

```bash
sudo ip netns exec vlan1 ip addr add 192.168.XX.X/24 dev veth1
sudo ip netns exec vlan2 ip addr add 192.168.XX.X/24 dev veth2
```

**Firewall Setup:**

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
sudo iptables -A FORWARD -s 192.168.XX.x -d 192.168.XX.X -j ACCEPT
sudo iptables -A FORWARD -s 192.168.XX.X -d 192.168.XX.XXX -j DROP
```

**Traffic Monitoring:**

```bash
sudo tcpdump -i veth1 host 192.168.XX.X
```

### 🔍 Key Insights

* Namespaces simulate VLANs effectively
* iptables enables custom traffic control
* tcpdump shows live ICMP, ARP, and blocked packets

---

## Lab 2: Exploring Network Interfaces and Packet Transmission

### Objectives

* View and configure interfaces
* Capture traffic with tcpdump and Wireshark
* Monitor bandwidth with `iftop`
* Analyze active connections and protocol activity

### Methodology

**Interface Check:**

* `lo`: 127.0.0.1 (loopback)
* `eth0`: 192.168.XXX.XXX

**tcpdump Traffic Analysis:**

* Captured ARP, DNS, TCP/HTTPS, NetBIOS
* Traffic to OWASP VM (192.168.XXX.XXX) confirmed

**netstat Output:**

* Active TCP sessions to internet
* One DHCP (UDP) session

**Wireshark Observation:**

* HTTP GET, TCP SYN/ACK, ARP, DHCP
* TCP flags observed: SYN, ACK, FIN, PSH

**Bandwidth Monitoring (iftop):**

* Total usage \~2 Kb/s to/from OWASP
* No congestion

**Filters:**

* Applied HTTP-only and TCP-only filters in Wireshark

### 🔍 Key Insights

* Effective for troubleshooting active connections
* Interfaces show real-time packet stats
* Filters improve packet-level analysis focus

---

## Lab 3: TCP/IP Protocol Stack and Packet Inspection

### Objectives

* Study TCP/IP layers and OSI comparison
* Analyze TCP flags, retransmissions
* Inspect HTTP and SSH traffic
* Simulate interface failure

### Methodology

**TCP/IP vs OSI Layers:**

* TCP/IP (4 layers) maps to OSI’s 7 layers

**TCP 3-Way Handshake:**

* SYN → SYN-ACK → ACK
* Verified using packet captures in Wireshark

**IP Header Fields Observed:**

* Source, Destination, TTL, Protocol
* TTL unchanged → confirms no routing hops

**HTTP/SSH Differences:**

* HTTP payload readable (status codes, headers)
* SSH encrypted; only metadata visible

**Error Handling in TCP:**

* Retransmissions observed (packet 2007)
* TCP sequence numbers ensure reliability

**Interface Down Test:**

```bash
sudo ifconfig eth0 down
sudo ifconfig eth0 up
```

* Loss of IP and traffic confirmed
* Reconnection after re-enable

### 🔍 Key Insights

* TCP ensures data reliability via ACKs, retries
* TCP/IP model clearly observable with Wireshark
* Interface toggling simulates real-world downtime

---

## 🔎 Reflection

These labs enhanced my skills in:

* Network design and simulation (namespaces, routing)
* Security analysis with tcpdump, Wireshark
* Protocol behavior understanding (TCP, ARP, DHCP)
* Troubleshooting using CLI tools

I now feel more confident approaching Linux-based network engineering tasks and applying core cybersecurity monitoring techniques.

---

👨‍💻 *Authored by:* **xploitpborgs (Oluwasola Adebayo Godwin)**
🌐 GitHub Portfolio: [github.com/xploitpborgs](https://github.com/xploitpborgs)
