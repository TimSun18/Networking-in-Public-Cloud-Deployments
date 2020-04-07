## Introduction

This task creates an EC2 with ubuntu AMI and then installs apache2 web server. After the initialization, the server copies an html file from the S3 bucket. The html file (index.html) is a static page containing an image pointing to S3 bucket as well. 

#### Preparation

Create an S3 bucket and upload the index.html and the static image to it.

![Image of S3 Bucket](https://github.com/TimSun18/Networking-in-Public-Cloud-Deployments/blob/master/3%20Compute%20Infrastructure/images/S3_Bucket.png?raw=true)

#### Step 1

Create a VPC including a public subnet, an IGW, an explicit route table and a security group. 

```
[root@localhost 3 Compute Infrastructure]# ansible-playbook create-vpc.yml --ask-vault-pass
Vault password: 

PLAY [localhost] ***************************************************************************************************

TASK [Create a VPC] ************************************************************************************************
changed: [localhost]

TASK [create Public subnet] ****************************************************************************************
changed: [localhost]

TASK [Internet Gateway setup] **************************************************************************************
changed: [localhost]

TASK [Public Route Table setup] ************************************************************************************
changed: [localhost]

TASK [Add Inbound Access To Web Server Security Group] *************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=5    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

#### Step 2

Create an EC2 instance with ubuntu AMI placing in the public subnet created in setp 1.

```
[root@localhost 3 Compute Infrastructure]# ansible-playbook create-ec2.yml --ask-vault-pass
Vault password: 

PLAY [localhost] ***************************************************************************************************

TASK [Get Security Group information] ******************************************************************************
ok: [localhost]

TASK [Get Subnet information] **************************************************************************************
ok: [localhost]

TASK [Provision instance(s)] ***************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0     
```

#### Step 3

Provision the web server by installing apache2 package and replacing the default index.html with the one stored in S3 bucket.

```
[root@localhost 3 Compute Infrastructure]# ansible-playbook provision-apache.yml --ask-vault-pass
Vault password: 

PLAY [Create a host group] *****************************************************************************************

TASK [Get EC2 info] ************************************************************************************************
ok: [localhost]

TASK [Add EC2 instances to the group] ******************************************************************************
skipping: [localhost] => (item={u'network_interfaces': [], u'root_device_type': u'ebs', u'private_dns_name': u'', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'state_transition_reason': u'User initiated (2020-04-07 09:51:09 GMT)', u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'', u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'security_groups': [], u'block_device_mappings': [], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'pending', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'state_reason': {u'message': u'Client.UserInitiatedShutdown: User initiated shutdown', u'code': u'Client.UserInitiatedShutdown'}, u'product_codes': [], u'monitoring': {u'state': u'disabled'}, u'ami_launch_index': 0, u'ena_support': True, u'ebs_optimized': False, u'launch_time': u'2020-04-07T09:47:19+00:00', u'instance_id': u'i-006f5a8b627a66dd0', u'instance_type': u't2.micro', u'state': {u'code': 48, u'name': u'terminated'}, u'root_device_name': u'/dev/sda1', u'hypervisor': u'xen', u'client_token': u'apache', u'virtualization_type': u'hvm', u'architecture': u'x86_64'}) 
skipping: [localhost] => (item={u'network_interfaces': [], u'root_device_type': u'ebs', u'private_dns_name': u'', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'state_transition_reason': u'User initiated (2020-04-07 10:16:53 GMT)', u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'', u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'security_groups': [], u'block_device_mappings': [], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'pending', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'state_reason': {u'message': u'Client.UserInitiatedShutdown: User initiated shutdown', u'code': u'Client.UserInitiatedShutdown'}, u'product_codes': [], u'monitoring': {u'state': u'disabled'}, u'ami_launch_index': 0, u'ena_support': True, u'ebs_optimized': False, u'launch_time': u'2020-04-07T10:07:17+00:00', u'instance_id': u'i-01de25fa9a794a93c', u'instance_type': u't2.micro', u'state': {u'code': 48, u'name': u'terminated'}, u'root_device_name': u'/dev/sda1', u'hypervisor': u'xen', u'client_token': u'apache-server', u'virtualization_type': u'hvm', u'architecture': u'x86_64'}) 
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-248.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0dae45c4ba2901bba', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-0f7f5f9d793e9b6ab', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'apache-web', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'3.24.124.100', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-07T10:43:47+00:00', u'volume_id': u'vol-05919e3e3bede60d2'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0dae45c4ba2901bba', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'3.24.124.100', u'public_dns_name': u'ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-248.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-0f7f5f9d793e9b6ab', u'network_interface_id': u'eni-0d691b5cb3aa66f8e', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-078eb4c56275c9e05', u'delete_on_termination': True, u'attach_time': u'2020-04-07T10:43:46+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.248', u'private_dns_name': u'ip-192-168-1-248.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'3.24.124.100', u'public_dns_name': u'ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:ae:df:89:cf:76', u'private_ip_address': u'192.168.1.248', u'vpc_id': u'vpc-0317319cc2c2a8168', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-07T10:43:46+00:00', u'instance_id': u'i-0f3dc712639daaa66', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.248', u'vpc_id': u'vpc-0317319cc2c2a8168', u'product_codes': []})
skipping: [localhost] => (item={u'network_interfaces': [], u'root_device_type': u'ebs', u'private_dns_name': u'', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'state_transition_reason': u'User initiated (2020-04-07 10:35:11 GMT)', u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'', u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'security_groups': [], u'block_device_mappings': [], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'pending', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'state_reason': {u'message': u'Client.UserInitiatedShutdown: User initiated shutdown', u'code': u'Client.UserInitiatedShutdown'}, u'product_codes': [], u'monitoring': {u'state': u'disabled'}, u'ami_launch_index': 0, u'ena_support': True, u'ebs_optimized': False, u'launch_time': u'2020-04-07T10:31:06+00:00', u'instance_id': u'i-021ccbe9c8440ad9c', u'instance_type': u't2.micro', u'state': {u'code': 48, u'name': u'terminated'}, u'root_device_name': u'/dev/sda1', u'hypervisor': u'xen', u'client_token': u'apache2-server', u'virtualization_type': u'hvm', u'architecture': u'x86_64'}) 

PLAY [Provision Apache Server] *************************************************************************************

TASK [Update and upgrade apt packages] *****************************************************************************
[WARNING]: The value True (type bool) in a string field was converted to 'True' (type string). If this does not
look like what you expect, quote the entire value to ensure it does not change.
changed: [ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com]

TASK [Install apache on ubuntu] ************************************************************************************
changed: [ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com]

TASK [Rename the existing html] ************************************************************************************
changed: [ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com]

TASK [Copy new index html from S3 bucket] **************************************************************************
changed: [ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com]

PLAY RECAP *********************************************************************************************************
ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com : ok=4    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost   
```

#### Step 4

Open the EC2 URL and check the webpage result. 

![Image of Result](https://github.com/TimSun18/Networking-in-Public-Cloud-Deployments/blob/master/3%20Compute%20Infrastructure/images/Apache_web.png?raw=true)

#### Clean-up

Delete all resource created previously.

```
[root@localhost 3 Compute Infrastructure]# ansible-playbook clean-up.yml --ask-vault-pass
Vault password: 

PLAY [localhost] ***************************************************************************************************

TASK [Get EC2 info] ************************************************************************************************
ok: [localhost]

TASK [Terminate instances that were previously launched] ***********************************************************
skipping: [localhost] => (item={u'network_interfaces': [], u'root_device_type': u'ebs', u'private_dns_name': u'', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'state_transition_reason': u'User initiated (2020-04-07 09:51:09 GMT)', u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'', u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'security_groups': [], u'block_device_mappings': [], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'pending', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'state_reason': {u'message': u'Client.UserInitiatedShutdown: User initiated shutdown', u'code': u'Client.UserInitiatedShutdown'}, u'product_codes': [], u'monitoring': {u'state': u'disabled'}, u'ami_launch_index': 0, u'ena_support': True, u'ebs_optimized': False, u'launch_time': u'2020-04-07T09:47:19+00:00', u'instance_id': u'i-006f5a8b627a66dd0', u'instance_type': u't2.micro', u'state': {u'code': 48, u'name': u'terminated'}, u'root_device_name': u'/dev/sda1', u'hypervisor': u'xen', u'client_token': u'apache', u'virtualization_type': u'hvm', u'architecture': u'x86_64'}) 
skipping: [localhost] => (item={u'network_interfaces': [], u'root_device_type': u'ebs', u'private_dns_name': u'', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'state_transition_reason': u'User initiated (2020-04-07 10:16:53 GMT)', u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'', u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'security_groups': [], u'block_device_mappings': [], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'pending', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'state_reason': {u'message': u'Client.UserInitiatedShutdown: User initiated shutdown', u'code': u'Client.UserInitiatedShutdown'}, u'product_codes': [], u'monitoring': {u'state': u'disabled'}, u'ami_launch_index': 0, u'ena_support': True, u'ebs_optimized': False, u'launch_time': u'2020-04-07T10:07:17+00:00', u'instance_id': u'i-01de25fa9a794a93c', u'instance_type': u't2.micro', u'state': {u'code': 48, u'name': u'terminated'}, u'root_device_name': u'/dev/sda1', u'hypervisor': u'xen', u'client_token': u'apache-server', u'virtualization_type': u'hvm', u'architecture': u'x86_64'}) 
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-248.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0dae45c4ba2901bba', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-0f7f5f9d793e9b6ab', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'apache-web', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'3.24.124.100', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-07T10:43:47+00:00', u'volume_id': u'vol-05919e3e3bede60d2'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0dae45c4ba2901bba', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'3.24.124.100', u'public_dns_name': u'ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-248.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-0f7f5f9d793e9b6ab', u'network_interface_id': u'eni-0d691b5cb3aa66f8e', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-078eb4c56275c9e05', u'delete_on_termination': True, u'attach_time': u'2020-04-07T10:43:46+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.248', u'private_dns_name': u'ip-192-168-1-248.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'3.24.124.100', u'public_dns_name': u'ec2-3-24-124-100.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:ae:df:89:cf:76', u'private_ip_address': u'192.168.1.248', u'vpc_id': u'vpc-0317319cc2c2a8168', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-07T10:43:46+00:00', u'instance_id': u'i-0f3dc712639daaa66', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.248', u'vpc_id': u'vpc-0317319cc2c2a8168', u'product_codes': []})
skipping: [localhost] => (item={u'network_interfaces': [], u'root_device_type': u'ebs', u'private_dns_name': u'', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'state_transition_reason': u'User initiated (2020-04-07 10:35:11 GMT)', u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'', u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'security_groups': [], u'block_device_mappings': [], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'pending', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'state_reason': {u'message': u'Client.UserInitiatedShutdown: User initiated shutdown', u'code': u'Client.UserInitiatedShutdown'}, u'product_codes': [], u'monitoring': {u'state': u'disabled'}, u'ami_launch_index': 0, u'ena_support': True, u'ebs_optimized': False, u'launch_time': u'2020-04-07T10:31:06+00:00', u'instance_id': u'i-021ccbe9c8440ad9c', u'instance_type': u't2.micro', u'state': {u'code': 48, u'name': u'terminated'}, u'root_device_name': u'/dev/sda1', u'hypervisor': u'xen', u'client_token': u'apache2-server', u'virtualization_type': u'hvm', u'architecture': u'x86_64'}) 

TASK [Pause 5 mins until EC2 terminated] ***************************************************************************
Pausing for 300 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [localhost]

TASK [Get VPC information] *****************************************************************************************
ok: [localhost]

TASK [Get Subnet information] **************************************************************************************
ok: [localhost]

TASK [Gather Route Table information] ******************************************************************************
ok: [localhost]

TASK [Delete Public Subnet] ****************************************************************************************
changed: [localhost]

TASK [Delete Route Table] ******************************************************************************************
changed: [localhost]

TASK [Delete Internet Gateway] *************************************************************************************
changed: [localhost]

TASK [Delete Security Group] ***************************************************************************************
changed: [localhost]

TASK [Delete VPC] **************************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=11   changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```
