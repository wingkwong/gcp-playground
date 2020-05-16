# Knowledge Check

1. In GCP, what is the minimum number of IP addresses that a VM instance needs?

- One: Only an internal IP address
- Two: One internal and one external IP address
- Three: One internal, one external and one alias IP address

Correct Answer: One: Only an internal IP address.

> That's correct! In GCP, each virtual machine needs to have an internal IP address. The external IP address is optional; therefore, a VM instance only needs one IP address.

2. What are the three types of networks offered in the Google Cloud Platform?

- Zonal, regional, and global
- Gigabit network, 10 gigabit network, and 100 gigabit network
- Default network, auto network and custom network
- IPv4 unicast network, IPv4 multicast network, IPv6 network

Correct Answer: Default network, auto network and custom network

> The default-type network established fixed standard subnetworks with predefined IP ranges and it is fast to setup. The auto-type network uses the same subnet IP ranges as the default-type, with a network name other than default. And custom-type allows you to specify the IP ranges of subnets.

3. What is one benefit of applying firewall rules by tag rather than by address?

- Tags help organizations track firewall billing
- Tags in network traffic help with network sniffing
- Tags on firewall rules control which ephemeral IP addresses VMs will receive
- When a VM is created with a matching tag, the firewall rules apply irrespective of the IP address it is assigned

Correct Answer: When a VM is created with a matching tag, the firewall rules apply irrespective of the IP address it is assigned

> When a VM is created the ephemeral external IP address is assigned from a pool. There is no way to predict which address will be assigned, so there is no way to write a rule that will match that VM's IP address before it is assigned. Tags allow a symbolic assignment that does not depend on order in the IP addresses. It makes for simpler, more general, and easier to maintain, firewall rules.