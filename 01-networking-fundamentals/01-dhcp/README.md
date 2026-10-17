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

Observation: The virtual machine's interface previously showed "10.0.2.15/24".

Command 2: Inspect routing information

ip route

Purpose: Displays the system's routing table.

Observation: The default route previously used gateway "10.0.2.2".

Command 3: Inspect DNS configuration

resolvectl status

Purpose: Displays DNS configuration for the system and its network links.

Observation: The DNS server previously shown for the virtual machine was "10.0.2.3".

Command 4: Test default gateway connectivity

ping -c 4 10.0.2.2

Purpose: Sends four ICMP echo requests to the virtual machine's default gateway.

Latest result: All four packets received replies, with 0% packet loss.

Interpretation: The default gateway was reachable from the virtual machine during the latest test.

Command 5: Test connectivity to Google's public DNS IP

ping -c 4 8.8.8.8

Purpose: Tests ICMP connectivity to Google's public DNS IP address.

Latest result: All four packets received replies, with 0% packet loss.

Interpretation: The virtual machine successfully received ICMP replies from this external IP address during the latest test.

Command 6: Test connectivity to Cloudflare's public DNS IP

ping -c 4 1.1.1.1

Purpose: Tests ICMP connectivity to Cloudflare's public DNS IP address.

Latest result: All four requests failed to receive replies.

Interpretation: Cloudflare's IP address did not respond to the ICMP requests during this test. This does not, by itself, establish the cause. Possible explanations include network filtering, routing issues, or destination-side ICMP handling.

6. Security Relevance

DHCP is important to network security because devices often rely on its configuration to communicate with other systems.

Potential attack: Rogue DHCP server

An unauthorized DHCP server could offer incorrect network settings, such as a malicious default gateway or DNS server.

If a device accepts those settings, its traffic could be redirected or exposed to further attacks.

Defensive measure: DHCP snooping

On supported managed switches, DHCP snooping can help prevent unauthorized DHCP server messages from reaching clients.

It uses trusted and untrusted switch ports and can maintain a DHCP binding table.

DHCP snooping was not configured or tested in this lab. It will be investigated in a later practical.

7. Findings and Limitations

The latest connectivity tests established that:

- The virtual machine's default gateway responded to all four ICMP requests.
- Google's public DNS IP ("8.8.8.8") responded to all four ICMP requests.
- Cloudflare's public DNS IP ("1.1.1.1") did not respond to any of the four ICMP requests.
- The earlier external connectivity results differed from the latest results.

These observations demonstrate that connectivity test results can change between testing sessions and that different destinations can produce different results.

The successful ping to "8.8.8.8" confirms ICMP reachability to that IP at the time of testing. It does not, by itself, prove that DNS name resolution is working.

Likewise, the failed ping to "1.1.1.1" does not prove that Cloudflare's DNS service is unavailable, because a service may be operational even when ICMP requests receive no replies.

The cause of the difference between the two external connectivity tests has not been conclusively identified.

A DHCP packet capture has not yet been documented. A future exercise will capture and inspect DHCP traffic to examine the DORA process directly.

8. Evidence

Screenshots will be added to support the observations in this report.

Planned evidence:

1. "ip addr" output showing the assigned IPv4 address.
2. "ip route" output showing the default gateway.
3. "resolvectl status" output showing DNS configuration.
4. Successful gateway ping.
5. Successful ping to "8.8.8.8".
6. Unsuccessful ping to "1.1.1.1".

Only genuine screenshots from the practical will be included.

9. Learning Outcomes

This exercise helped develop practical understanding of:

- IPv4 network configuration.
- Default gateways and routing tables.
- DNS server configuration.
- ICMP connectivity testing using "ping".
- The security implications of unauthorized DHCP servers.
- The importance of distinguishing observed facts from unconfirmed causes.
- Why a failed ping does not necessarily mean that a destination's service is unavailable.

10. Next Steps

- Capture DHCP packets and identify the DORA process.
- Study DNS resolution and its security implications.
- Investigate why connectivity results may differ between destinations.
- Investigate DHCP snooping and related switch security controls.

Disclaimer: This is an educational lab performed in a virtual environment. The observations and conclusions are limited to the tests documented above.
