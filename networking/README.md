# Networking

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

## Cloud Load Balancing 

- Users get a single, global anycast IP address
- Traffic goes over the Google backbone from the closest point-of-presence to the user
- Backends are selected based on load
- Only healthy backends receive traffic
- No pre-warming is required
- A suite of load-balancing options:
    ![image](https://user-images.githubusercontent.com/35857179/81463109-8dc31000-91e9-11ea-98f2-56fd74625ed4.png)

## Cloud DNS

- Create managed zones, then add, edit, delete DNS records

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