## Introduction
This is the assignment for module 6 Secure Your Virtual Network Infrastructure.
### 1. Traffic filters
#### Requirements
Anyone can connect to the Web server over HTTP and HTTPS;<br />
Specified IP addresses can connect to the SSH jump host over SSH;<br />
SSH jump host can connect to any VM within your virtual network over SSH;<br />
Web server(s) can connect to database server(s) over HTTP and MySQL (or any other similar service)<br />
Database server(s) can communicate over HTTP and MySQL
#### Solution
Create 3 security groups (web, jumphost and database) to filter the traffic. Need to instantiate the groups first, then update the rules consisting group references.
```
[timsun@controller 6 Security]$ ansible-playbook traffic_filters.yml 

PLAY [localhost] ***************************************************************************************************

TASK [create a VPC] ************************************************************************************************
changed: [localhost]

TASK [create a public subnet] **************************************************************************************
changed: [localhost]

TASK [create a private subnet] *************************************************************************************
changed: [localhost]

TASK [create an Internet gateway] **********************************************************************************
changed: [localhost]

TASK [create a public route table] *********************************************************************************
changed: [localhost]

TASK [initiate the web server securtiy group] **********************************************************************
changed: [localhost]

TASK [initiate the jumphost securtiy group] ************************************************************************
changed: [localhost]

TASK [initiate the database securtiy group] ************************************************************************
changed: [localhost]

TASK [modify the web server securtiy group] ************************************************************************
changed: [localhost]

TASK [modify the jumphost securtiy group] **************************************************************************
changed: [localhost]

TASK [modify the database securtiy group] **************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=11   changed=11   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

### 2. Identity and Access Management
#### Requirements
Create multiple users within your account (or subscription):<br />
A user that has read-only access - when using those credentials you should be able to see the networking and compute resources, but not modify them.<br />
A user that can modify the storage bucket you created in the third exercise, but not anything else<br />
A user that can view networking resources and modify compute resources. Split your deployment procedure into two parts, and deploy networking and compute resources using two separate users<br />

#### Solution
Configure some IAM users to meet the requirements.
```
[timsun@controller 6 Security]$ ansible-playbook iam.yml             

PLAY [localhost] ***************************************************************************************************

TASK [create a user that has read-only access] *********************************************************************
changed: [localhost]

TASK [create a user that can modify the storage bucke] *************************************************************
changed: [localhost]

TASK [create a user that can view networking resources and modify compute resources] *******************************
changed: [localhost]

TASK [create a user that can view networking resources and modify compute resources] *******************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=4    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

### 3. Application firewall
#### Requirements
Add a web application firewall in front of your web server and block any attempts to access /admin or /login URLs.

#### Solution
Create a condition with two filters referenced by a WAF rule then called by a WAF ACL.
```
[timsun@controller 6 Security]$ ansible-playbook waf.yml

PLAY [localhost] ***************************************************************************************************

TASK [create a condition mathing /admin] ***************************************************************************
changed: [localhost]

TASK [create a condition mathing /login] ***************************************************************************
changed: [localhost]

TASK [create WAF rule] *********************************************************************************************
changed: [localhost]

TASK [create web ACL] **********************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=4    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

#### Clean-up
Delete all resources created previously.
```
[timsun@controller 5 IPv6 in Cloud]$ ansible-playbook clean-up.yml  

PLAY [localhost] ***************************************************************************************************

TASK [Get EC2 info] ************************************************************************************************
ok: [localhost]

TASK [Terminate instances that were previously launched] ***********************************************************
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-50.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01fc50dcc434f20c8', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'43e9f3ad35fc42ba848eb24174e1ba2b', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'13.55.25.161', u'tags': {u'Name': u'jumphost'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-13-55-25-161.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:22:16+00:00', u'volume_id': u'vol-0efbad0b3e08012c2'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [{u'ipv6_address': u'2406:da1c:711:6300:9c1c:476d:bc9c:206a'}], u'groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'13.55.25.161', u'public_dns_name': u'ec2-13-55-25-161.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-50.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01fc50dcc434f20c8', u'network_interface_id': u'eni-0dbb2f059b095e7f3', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-0959efd1bb6b3356a', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:22:15+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.50', u'private_dns_name': u'ip-192-168-1-50.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'13.55.25.161', u'public_dns_name': u'ec2-13-55-25-161.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:84:46:12:54:4e', u'private_ip_address': u'192.168.1.50', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'owner_id': u'687251871473'}], u'launch_time': u'2020-05-02T11:22:15+00:00', u'instance_id': u'i-019000d30f05bd06f', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.50', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'product_codes': []})
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-2-105.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-0f96362a3273f0136', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'6e99ff1345d549dba9cdf6e9d8a6a968', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'52.63.178.178', u'tags': {u'Name': u'private'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-52-63-178-178.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:27:41+00:00', u'volume_id': u'vol-00582f38eeefcc647'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [{u'ipv6_address': u'2406:da1c:711:6364:2bb0:b3d9:b51b:7ba9'}], u'groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'52.63.178.178', u'public_dns_name': u'ec2-52-63-178-178.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-2-105.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-0f96362a3273f0136', u'network_interface_id': u'eni-0a510e92fe158a84b', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-044b476e14491bea5', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:27:40+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.2.105', u'private_dns_name': u'ip-192-168-2-105.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'52.63.178.178', u'public_dns_name': u'ec2-52-63-178-178.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:d9:a5:c9:d4:60', u'private_ip_address': u'192.168.2.105', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'owner_id': u'687251871473'}], u'launch_time': u'2020-05-02T11:27:40+00:00', u'instance_id': u'i-05c2709c835ebce05', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.2.105', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'product_codes': []})
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01fc50dcc434f20c8', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'0af719359798493a94f3664fda32efa3', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'13.238.194.174', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:24:51+00:00', u'volume_id': u'vol-033fc4e2248525b22'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [{u'ipv6_address': u'2406:da1c:711:6300:e5e7:47a7:d064:e792'}], u'groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'13.238.194.174', u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01fc50dcc434f20c8', u'network_interface_id': u'eni-0fc018c62637d15b3', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-0ab3cad9a284dc670', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:24:50+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.141', u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'13.238.194.174', u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:c9:e3:88:c1:f8', u'private_ip_address': u'192.168.1.141', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'owner_id': u'687251871473'}], u'launch_time': u'2020-05-02T11:24:50+00:00', u'instance_id': u'i-0bd1450cd8c22209c', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.141', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'product_codes': []})

TASK [Pause 5 mins until EC2 instances terminated] *****************************************************************
Pausing for 300 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [localhost]

TASK [Get VPC information] *****************************************************************************************
ok: [localhost]

TASK [Get Public Subnet information] *******************************************************************************
ok: [localhost]

TASK [Gather Route Table information] ******************************************************************************
ok: [localhost]

TASK [Delete Public Subnet] ****************************************************************************************
changed: [localhost]

TASK [Delete Private Subnet] ***************************************************************************************
changed: [localhost]

TASK [Delete Route Table] ******************************************************************************************
changed: [localhost]

TASK [Delete Internet Gateway] *************************************************************************************
changed: [localhost]

TASK [Delete security group] ***************************************************************************************
changed: [localhost]

TASK [Delete VPC] **************************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=12   changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
