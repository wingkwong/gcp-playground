# Implement Private Google Access and Cloud NAT

## Goals

- Configure a VM instance that doesn't have an external IP - address
- Connect to a VM instance using an Identity-Aware Proxy (IAP) - tunnel
- Enable Private Google Access on a subnet
- Configure a Cloud NAT gateway
- Verify access to public IP addresses of Google APIs and services and other connections to the internet

## Task 1. Create the VM instance

Create a VPC network with some firewall rules and a VM instance that has no external IP address, and connect to the instance using an IAP tunnel.

### Create a VPC network and firewall rules

First, create a VPC network for the VM instance and a firewall rule to allow SSH access.

In the Cloud Console, on the Navigation menu (Navigation menu), click VPC network > VPC networks.

Click Create VPC network.

For Name, type privatenet

For Subnet creation mode, click Custom.

Specify the following, and leave the remaining settings as their defaults:

```
Property	Value (type value or select option as specified)
Name	privatenet-us
Region	us-central1
IP address range	10.130.0.0/20
Don't enable Private Google access yet!
```

Click Done.

Click Create and wait for the network to be created.

In the left pane, click Firewall rules.

Click Create firewall rule.

Specify the following, and leave the remaining settings as their defaults:

```
Property	Value (type value or select option as specified)
Name	privatenet-allow-ssh
Network	privatenet
Targets	All instances in the network
Source filter	IP ranges
Source IP ranges	35.235.240.0/20
Protocols and ports	Specified protocols and ports
For tcp, specify port 22.
```

Click Create.

In order to connect to your private instance using SSH, you need to open an appropriate port on the firewall. IAP connections come from a specific set of IP addresses (35.235.240.0/20). Therefore, you can limit the rule to this CIDR range.

### Create the VM instance with no public IP address

In the Cloud Console, on the Navigation menu (Navigation menu), click Compute Engine > VM instances.

Click Create.

Specify the following, and leave the remaining settings as their defaults:

```
Property	Value (type value or select option as specified)
Name	vm-internal
Region	us-central1
Zone	us-central1-c
Machine type	1vCPU (3.75 GB memory, n1-standard-1)
Click Management, security, disks, networking, sole tenancy.
```

Click Networking.

For Network interfaces, click the pencil icon to edit.

Specify the following, and leave the remaining settings as their defaults:

```
Property	Value (type value or select option as specified)
Network	privatenet
Subnetwork	privatenet-us
External IP	None
```

The default setting for a VM instance is to have an ephemeral external IP address. This behavior can be changed with a policy constraint at the organization or project level. To learn more about controlling external IP addresses on VM instances, refer to the external IP address documentation.

Click Done.

Click Create, and wait for the VM instance to be created.

On the VM instances page, verify that the External IP of vm-internal is None.

Create the VM instance

### SSH to vm_internal to test the IAP tunnel

In the Cloud Console, click Activate Cloud Shell (Cloud Shell).

If prompted, click Continue.

To connect to vm-internal, run the following command:

```
gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
```

If prompted about continuing, type Y.

When prompted for a passphrase, press ENTER.

When prompted for the same passphrase, press ENTER.

Did the command prompt change to @vm-internal? 

To test the external connectivity of vm-internal, run the following command:

```
ping -c 2 www.google.com
```

This should not work because vm-internal has no external IP address!

Wait for the ping command to complete.

To return to your Cloud Shell instance, run the following command:

```
exit
```

> When instances do not have external IP addresses, they can only be reached by other instances on the network via a managed VPN gateway or via a Cloud IAP tunnel. Cloud IAP enables context-aware access to VMs via SSH and RDP without bastion hosts. For more information about this, see this blog post.

## Task 2. Enable Private Google Access

VM instances that have no external IP addresses can use Private Google Access to reach external IP addresses of Google APIs and services. By default, Private Google Access is disabled on a VPC network.

### Create a Cloud Storage bucket

Create a Cloud Storage bucket to test access to Google APIs and services.

In the Cloud Console, on the Navigation menu (Navigation menu), click Storage > Browser.

Click Create bucket.

Specify the following, and leave the remaining settings as their defaults:

```
Property	    Value (type value or select option as specified)
Name	        Enter a globally unique name
Location type	Multi-region
```

Click Create.

Note the name of your storage bucket for the next subtask. It will be referred to as [my_bucket].

### Copy an image file into your bucket

Copy an image from a public Cloud Storage bucket to your own bucket.

In Cloud Shell, run the following command, replacing [my_bucket] with your bucket's name:

```
gsutil cp gs://cloud-training/gcpnet/private/access.svg gs://[my_bucket]
```

In the Cloud Console, click Refresh Bucket to verify that the image was copied.

You can click on the name of the image in the Cloud Console to view an example of how Private Google Access is implemented.

In Cloud Shell, to try to copy the image from your bucket, run the following command, replacing [my_bucket] with your bucket's name:

```
gsutil cp gs://[my_bucket]/*.svg .
```

This should work because Cloud Shell has an external IP address!

To connect to vm-internal, run the following command:

gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap

If prompted, type Y to continue.

To try to copy the image to vm-internal, run the following command, replacing [my_bucket] with your bucket's name:

```
gsutil cp gs://[my_bucket]/*.svg .
```

This should not work: vm-internal can only send traffic within the VPC network because Private Google Access is disabled (by default).

Press Ctrl+C to stop the request.

### Enable Private Google Access

Private Google Access is enabled at the subnet level. When it is enabled, instances in the subnet that only have private IP addresses can send traffic to Google APIs and services through the default route (0.0.0.0/0) with a next hop to the default internet gateway.

In the Cloud Console, on the Navigation menu (Navigation menu), click VPC network > VPC networks.

Click privatenet-us to open the subnet.

Click Edit.

For Private Google access, select On.

Click Save.

> Yes, enabling Private Google Access is as simple as selecting On within the subnet!

In Cloud Shell for vm-internal, to try to copy the image to vm-internal, run the following command, replacing [my_bucket] with your bucket's name:

```
gsutil cp gs://[my_bucket]/*.svg .
```

This should work because vm-internal's subnet has Private Google Access enabled!

To return to your Cloud Shell instance, run the following command:

```
exit
```

> To view the eligible APIs and services that you can use with Private Google Access, see supported services in the Private Google Access overview.

## Task 3. Configure a Cloud NAT gateway

Although vm-internal can now access certain Google APIs and services without an external IP address, the instance cannot access the internet for updates and patches. Configure a Cloud NAT gateway, which allows vm-internal to reach the internet.

### Try to update the VM instances

In Cloud Shell, to try to re-synchronize the package index, run the following:

sudo apt-get update

The output should finish like this (do not copy; this is example output):

```
...
Reading package lists... Done
```

This should work because Cloud Shell has an external IP address!

To connect to vm-internal, run the following command:

```
gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
```

If prompted, type Y to continue.

To try to re-synchronize the package index of vm-internal, run the following command:

```
sudo apt-get update
```

This should only work for Google Cloud packages because vm-internal only has access to Google APIs and services!

Press Ctrl+C to stop the request.

### Configure a Cloud NAT gateway

Cloud NAT is a regional resource. You can configure it to allow traffic from all ranges of all subnets in a region, from specific subnets in the region only, or from specific primary and secondary CIDR ranges only.

In the Cloud Console, on the Navigation menu (Navigation menu), click Network services > Cloud NAT.

Click Get started to configure a NAT gateway.

Specify the following:

```
Property	        Value (type value or select option as specified)
Gateway name	    nat-config
VPC network	        privatenet
Region	            us-central1
```

For Cloud Router, select Create new router.

For Name, type nat-router

Click Create.

> The NAT mapping section allows you to choose the subnets to map to the NAT gateway. You can also manually assign static IP addresses that should be used when performing NAT. Do not change the NAT mapping configuration in this lab.

Click Create.

Wait for the gateway's status to change to Running.

### Verify the Cloud NAT gateway

It may take up to 3 minutes for the NAT configuration to propagate to the VM, so wait at least a minute before trying to access the internet again.

In Cloud Shell for vm-internal, to try to re-synchronize the package index of vm-internal, run the following command:

```
sudo apt-get update
```

The output should finish like this (do not copy; this is example output):

```
...
Reading package lists... Done
```

This should work because vm-internal is using the NAT gateway!

> The Cloud NAT gateway implements outbound NAT, but not inbound NAT. In other words, hosts outside of your VPC network can only respond to connections initiated by your instances; they cannot initiate their own, new connections to your instances via NAT.

## Task 4. Review

You created vm-internal, an instance with no external IP address, and connected to it securely using an IAP tunnel. Then you enabled Private Google Access, configured a NAT gateway, and verified that vm-internal can access Google APIs and services and other public IP addresses.

VM instances without external IP addresses are isolated from external networks. Using Cloud NAT, these instances can access the internet for updates and patches, and in some cases, for bootstrapping. As a managed service, Cloud NAT provides high availability without user management and intervention.

IAP uses your existing project roles and permissions when you connect to VM instances. By default, instance owners are the only users that have the IAP Secured Tunnel User role. If you want to allow other users to access your VMs using IAP tunneling, you need to grant this role to those users.