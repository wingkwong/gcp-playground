# Networking

- A network
    - has no IP address range
    - is global and spans all available regions
    - contains subnetworks 
    - is available as default, auto, or custom

## VPC
Google Cloud VPC networks are global; subnets are regional

- Use its route table to forward traffic within the network, even across subnets
- Use its firewall to control what network traffic is allowed
- Use Shared VPC to share a network or individual subnets, with other GCP projects
- Use VPC Peering to interconnect networks in GCP Projects

Network Types
- Default 
    - Every Project
    - One subnet per region
    - Default firewall rules
- Auto Mode
    - Default Network 
    - One subnet per region
    - Regional IP allocation
    - Fixed /20 subnetwork per region 
    - Expandable up to /16
- Custom Mode
    - No default subnets created
    - Full control of IP ranges
    - Regional IP allocation
    - Expandable to any RFC 1918 size

Network Isolated Systems

Network #1: VM-A(us-east1), VM-B(us-west1)
Network #3: C (us-east1)
Network #4: D (us-east1)

- A and B can communicate over internal IPs even thought they are not in the same region
- C and D must communicate over external IPs even though they are in the same region

**Google's VPC is global**

Subnetwork cross zones

- VMs can be on the same subnet but in different zones
- A single firewall rule can apply to both VMs

Expand subnets without re-creating instances

- Cannot overlap with other subnets
- Must be inside the RDC 1918 address spaces
- Can expand but not shrink
- Auto mode can be expanded from /20 to /16
- Avoid large subnets

VMs can have internal and external IP addresses

- Internal IP
    - Allocated from subnet range to VMs by DHCP
    - DHCP lease is renewed every 24 hours
    - VM name + IP is registered with network-scoped DNS

- External IP
    - Assigned from pool (ephemeral)
    - Reserved (static)
    - VM doesn't know external IP; it is mapped to the internal IP
        - use ``sudo /sbin/ifconfig`` for more details

DNS resolution for internal addresses

Each instance has a hostname that cna be resolved to an internal IP address:

- The hostname is the same as the instance name
- FQDN is [hostname].[zone].c.[project-id].internal

Example: my-server.us-central-a.c.guestbook-123456.internal

Name resolution is handled by internal DNS resolver:

- Provided as part of Compute Engine (169.254.169.254)
- Configured for use on instance via DHCP
- Provides answer for internal and external addresses

Instances with external IP address can allow connections from hosts outside the project

- Users connect directly using external IP address
- Admins can also publish public DNS records pointing to the instance
    - Public DNS records are not published automatically
- DNS records for external addresses can be published using existing DNS servers (outside of GCP)
- DNS zones can be hosted using Cloud DNS

## Cloud Load Balancing 

- Users get a single, global anycast IP address
- Traffic goes over the Google backbone from the closest point-of-presence to the user
- Backends are selected based on load
- Only healthy backends receive traffic
- No pre-warming is required
- A suite of load-balancing options:
    ![image](https://user-images.githubusercontent.com/35857179/81463109-8dc31000-91e9-11ea-98f2-56fd74625ed4.png)

## Cloud DNS

- Google's DNS service
- Translate domain names into IP address
- Low latency; High availability (100% uptime SLA)
- Create and update millions of DNS records
- UI, CLI or API

## Cloud CDN 

- Use Google's globally distributed edge caches to cache content close to your users
- Or use CDN Interconnect if you'd prefer to use a different CDN

## Interconnect options

- VPN
    - Secure multi-Gbbps connection over VPN tunnels

- Direct Peering
    - Private connection between you and Google for your hybrid cloud workloads

- Carrier Peering
    - Connection through the largest partner network of service providers

- Dedicated Interconnect
    - Connect N X 10G transport circuits for private cloud traffic to Google Cloud at Google POPs

## Routes and firewall rules

- VPC network functions as a distributed firewall
- Firewall rules are applied to the network as a whole
- Connections are allowed or denied at the instance level
- Firewall rules are stateful
- Implied deny all ingress and allow all egress

Use case: Ingress/Egress

Conditions:

- Destination CIDR ranges
- Protocols
- Ports

Action:

- Allow: permit the matching ingress/egress connection
- Deny: block the matching ingress/egress connection