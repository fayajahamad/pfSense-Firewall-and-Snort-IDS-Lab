# pfSense Firewall & Snort IDS — Virtual Network Security Lab

> A hands-on network security project demonstrating firewall deployment, network segmentation, routing, NAT, DHCP, DNS, access-control policies, and signature-based intrusion detection in a virtualized lab.

## Project overview

This project uses **pfSense Community Edition** in **Oracle VirtualBox** to create a small security gateway between a simulated WAN and an isolated LAN. A virtual client on the LAN was used to test addressing, reachability, name resolution, internet connectivity, and policy enforcement. **Snort** was configured on the LAN interface for signature-based intrusion detection.

**Environment:** Oracle VirtualBox, pfSense, Windows/Ubuntu client, Snort, ICMP, TCP/IP, DHCP, DNS, NAT.

## Lab topology

Simulated WAN / Upstream network
             |
       [Bridged Adapter]
             |
       pfSense Firewall
       WAN: 10.3.8.166/24
       LAN: 10.1.10.1/24
             |
       [Internal Network]
             |
       LAN client VM
       DHCP: 10.1.10.100–10.1.10.200
       Gateway/DNS: 10.1.10.1


All addresses are from the documented local lab, not a production deployment.

## What I configured

- Deployed pfSense with separate WAN and LAN virtual network interfaces.
- Configured static interface addressing and the LAN default gateway.
- Enabled the LAN DHCP service and tested client address assignment.
- Configured DNS Resolver and upstream DNS settings; tested DNS queries.
- Used pfSense routing/NAT to enable client connectivity through the gateway.
- Created an alias-based LAN firewall policy intended to restrict Facebook-related destinations and placed the block policy before the default allow rule.
- Installed/configured Snort IDS on the LAN interface and added custom local detection signatures.

## Example Snort test rules

These simple **lab-only detection rules** were used to verify alert generation:


alert icmp any any -> $HOME_NET any (msg:"EDU TEST ICMP Ping detected"; sid:1000001; rev:1;)
alert tcp any any -> $HOME_NET 80 (msg:"EDU TEST HTTP Access detected"; sid:1000002; rev:1;)


## Validation and observations

| Test | Observation documented in the lab report |
| DHCP | Client obtained `10.1.10.100` |
| LAN connectivity | Client pinged pfSense gateway at `10.1.10.1` |
| DNS | Client performed `nslookup www.google.com` |
| Internet | Connectivity tested with public IP ping |
| Firewall | Browser test showed Facebook connection timeout after block rule setup |
| IDS | Snort recorded alerts for custom test signatures |

**Scope note:** The browser timeout is evidence of a test outcome, not proof that every Facebook domain, IP address, or application is blocked. FQDN-based restrictions can be incomplete when services use dynamic IP addresses/CDNs. These are educational test signatures and not a production-grade detection policy.

## Troubleshooting lessons

- Firewall rules are evaluated from top to bottom, so specific block policies must precede broader allows.
- DNS caches and changing destination addresses can affect the observed results of domain-related filtering tests.
- Test an access-control policy with multiple independent observations (client output, pfSense logs, and repeated tests), not only a browser error.

## Documentation

The practical report includes setup steps, screenshots and validation evidence:

- [`docs/pfSense-Project-Report.pdf`](docs/pfSense-Project-Report.pdf)


## Skills demonstrated

Network security, pfSense, firewall rule management, WAN/LAN segmentation, virtual networking, DHCP, DNS, NAT, IDS/Snort, connectivity testing, and troubleshooting.

