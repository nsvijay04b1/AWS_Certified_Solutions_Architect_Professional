# Direct Connect (DX):
- links your internal network to an AWS Direct Connect location over a standard Ethernet fiber-optic cable (LX, LR or LR4).
- One end of the cable is connected to your router or the APN partner router in DX location, the other end of the cable is connected to an AWS Direct Connect router. 
- Physical Connection Options:
	- Circuit between the customer premises and the DX location.
	- Circuit between the customer premises and an SP offering DX. Particularly useful for MPLS.
	- Customer router in the same location as the DX router.
- Direct Connect is not redundant by default (one circuit by default).
- Predictable bandwidth and Performance,
- Supports VLAN trunking (802.1Q)
- Connection types: dedicated or hosted.
- Dedicated connection:
	- For a single customer.
	- Single or multiple 1G, 10G or 100G ports.
	- Supports port aggregation with LAG (LACP) for up to 4 x 1/10G ports or 2 x 100G ports.
	- Customers can request a dedicated connection through the AWS Direct Connect console, the CLI, or the API. 
	- After you request the connection, we make a Letter of Authorization and Connecting Facility Assignment (LOA-CFA) available to you to download.
	- The LOA-CFA is the authorization to connect to AWS, and is required by your network provider to order a cross connect for you. 
- Hosted connection:
	- Managed by an AWS partner. 
	- The partner can share a 10G link between several customers.
	- Starts at 50 Mbps.
- Virtual Interfaces (VIFs):
	- A connection of 1 Gbps or more can be partitioned into multiple VIFs.
	- A connection of less than 1 Gbps supports only one VIF.
	- Each VIF is mapped to a VLAN on the 802.1Q trunk between the AWS DX router and the customer/partner router in the DX location.
- You must create one of the following virtual interfaces to begin using your AWS Direct Connect connection:
	- Public VIF: allows connectivity to public IP addresses in AWS over the DX instead of traversing internet: connectivity to an AWS Service, to your public AWS network or those of other AWS customers.
	- Private VIF: access a VPC using private IP addresses.
	- Transit VIF: access one or more Amazon VPC Transit Gateways associated with Direct Connect gateways. You can use transit virtual interfaces with 1/2/5/10/100 Gbps AWS Direct Connect connections. 
	- Hosted VIF: To use your AWS Direct Connect connection with another AWS account, you can create a hosted virtual interface for that account. 

# Direct Connect Gateway (DCG): 
- A Direct Connect gateway is a globally available resource that you use to connect your VPCs to the Private VIFs of your Direct Connect connection.
- You can create the Direct Connect gateway in any Region and access it from all other Regions.
- A DCG can be attached either to a VGW or a Transit Gateway (when you have multiple VPCs in the same region).


To connect your AWS Direct Connect connection to your VPCs in the same or different Regions, you need to:
1. Create a private virtual interface for the Direct Connect connection,
2. Create a Direct Connect gateway (DCG), attach the private VIF to this DCG
3. Then, you can either:
	- Create a Virtual Private Gateway (VGW) in the VPC, and associate this VGW to the DCG. VPCs connected in this way to the same DCG will not be able to connect to each other through these associations.
	- Create a single Transit Gateway that is attached to the VPC(s). Associate this Transit Gateway to the DCG. In this case VPCs that are attached to the same Transit Gateway can connect to each other through the Transit Gateway. The private VIF in this case is called a Transit Virtual Interface.

Not Supported:
- Associate a virtual private gateway with more than one Direct Connect gateway.
- Direct communication between the VPCs that are associated with a single Direct Connect gateway.
- Direct communication between the virtual interfaces that are attached to a single Direct Connect gateway.

AWS Direct Connect + VPN:
- you can combine AWS Direct Connect dedicated network connections with the AWS Site-to-Site VPN.
- Uses Public VIF only.
- An IPSec tunnel is established between your AWS gateway (VGW or TGW) and the on-prem router/firewall.

# Software Site-to-Site VPN:
- offers you the flexibility to fully manage both sides of your Amazon VPC connectivity by creating a VPN connection between your remote network and a software VPN appliance running in your Amazon VPC network. 
- This option is recommended if you must manage both ends of the VPN connection, either for compliance purposes or for leveraging gateway devices that are not currently supported by Amazon VPC’s VPN solution.
