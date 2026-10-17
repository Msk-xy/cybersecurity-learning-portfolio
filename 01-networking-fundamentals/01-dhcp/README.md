DHCP Configuration and Network Connectivity Investigation

1. Project Overview

Category: Networking Fundamentals
Topic: Dynamic Host Configuration Protocol (DHCP)
Environment: Ubuntu 24.04.2 LTS running in Oracle VirtualBox
Lab Type: Hands-on networking and troubleshooting
Status: Initial investigation completed; supporting screenshots to be added.

Objective

The objective of this practical was to understand how network configuration works, inspect the network settings assigned to a virtual machine, and test connectivity between the virtual machine and external IP addresses.

2. Tools Used

- Ubuntu 24.04.2 LTS
- Oracle VirtualBox
- Linux terminal
- "ip addr"
- "ip route"
- "resolvectl status"
- "ping"

3. DHCP Fundamentals

DHCP stands for Dynamic Host Configuration Protocol.

It automatically provides devices with network configuration, which may include:

- An IP address
- A subnet mask or network prefix
- A default gateway
- DNS server information

Without DHCP, a network administrator may need to configure these settings manually.

The DORA Process

DHCPv4 commonly follows four steps:

1. Discover: The client searches for a DHCP server.
2. Offer: A DHCP server offers network configuration.
3. Request: The client requests the offered configuration.
4. Acknowledgment: The server confirms the allocation.

DHCPv4 uses UDP port 67 for servers and UDP port 68 for clients.

4. Lab Environment and Observations

The Ubuntu virtual machine was configured to use VirtualBox NAT networking.

The following settings were observed during the investigation:

Setting| Observed value
Virtual machine IP address| "10.0.2.15/24"
Default gateway| "10.0.2.2"
DNS server| "10.0.2.3"
Virtual machine network mode| NAT

These addresses are typical of VirtualBox's default NAT configuration.

The observations show that the virtual machine had an IP configuration and a default route. However, these observations alone do not prove that a DHCP exchange was captured.

5. Commands and Their Purpose

Command 1: Inspect IP configuration

ip addr

Purpose: Displays network interfaces and their assigned IP addresses.

Observation: The virtual machine's interface showed "10.0.2.15/24".

Command 2: Inspect routing information

ip route

Purpose: Displays the system's routing table.

Observation: The default route used gateway "10.0.2.2".

Command 3: Inspect DNS configuration

resolvectl status

Purpose: Displays DNS configuration for the system and its network links.

Observation: The DNS server shown for the virtual machine was "10.0.2.3".

Command 4: Test gateway connectivity

ping -c 4 10.0.2.2

Purpose: Sends four ICMP echo requests to the virtual machine's default gateway.

Result: All four packets received replies.

Interpretation: The gateway was reachable from the virtual machine during this test.

Command 5: Test external connectivity

ping -c 4 8.8.8.8

Purpose: Tests ICMP connectivity to an external IP address.

Result: One of four packets received a reply, resulting in 75% packet loss.

A second test against "1.1.1.1" received no replies.

Interpretation: External ICMP connectivity was unsuccessful or unreliable during these tests. The results do not, by themselves, identify the cause. Possible explanations include connectivity problems or ICMP filtering.

6. Security Relevance

DHCP is important to network security because devices often rely on its configuration to communicate with other systems.

Potential attack: Rogue DHCP server

An unauthorized DHCP server could offer incorrect network settings, such as a malicious default gateway or DNS server.

If a device accepts those settings, its traffic could be redirected or exposed to further attacks.

Defensive measure: DHCP snooping

On supported managed switches, DHCP snooping can help prevent unauthorized DHCP server messages from reaching clients.

It uses trusted and untrusted switch ports and can maintain a DHCP binding table.

DHCP snooping will be investigated in a later practical. It was not configured or tested in this lab.

7. Findings and Limitations

The investigation established that:

- The virtual machine had an IPv4 address.
- A default gateway was configured.
- DNS server information was available.
- The virtual machine could reach its gateway.
- External ICMP connectivity was unreliable during testing.

The cause of the external connectivity problem was not conclusively identified.

A DHCP packet capture has not yet been documented. A future exercise will capture and inspect DHCP traffic to examine the DORA process directly.

8. Evidence

Screenshots will be added to this project to support the observations.

Planned evidence:

1. "ip addr" output.
2. "ip route" output.
3. "resolvectl status" output.
4. Successful gateway ping.
5. External connectivity test results.

Only genuine screenshots from the practical will be included.

9. Learning Outcomes

This exercise helped develop practical understanding of:

- IPv4 network configuration.
- Default gateways and routing tables.
- DNS server configuration.
- Basic connectivity testing with "ping".
- The security implications of unauthorized DHCP servers.
- The importance of distinguishing observed facts from unconfirmed causes.

10. Next Steps

- Capture DHCP packets and identify the DORA process.
- Study DNS resolution and its security implications.
- Continue network troubleshooting exercises.
- Investigate DHCP snooping and related switch security controls.

---

Disclaimer: This is an educational lab performed in a virtual environment. The observations and conclusions are limited to the tests documented above.
