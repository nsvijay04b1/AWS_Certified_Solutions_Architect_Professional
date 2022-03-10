# AWS Network Firewall:
- A stateful, managed, network firewall and intrusion detection and prevention service that can filter traffic at the perimeter of your VPC.
- Created in the VPC.
- Uses an open source IPS (Suricata) for stateful inspection.
- Deep packet inspection.
- Can be combined with services and components that you use with your VPC, for example an internet gateway, a NAT gateway, a VPN, or a transit gateway.
- To enable the firewall's protection, you modify your Amazon VPC route tables to send your network traffic through the Network Firewall firewall endpoints.
- Needs dedicated subnet in each AZ.
- Does not support VPC peering.

