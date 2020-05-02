## Introduction

This task creates 3 EC2 instances, among which 2 instances (web and jumphost) are in a public subnet and 1 private host placed in the private subnet.

The target is users from Internet are able to SSH to the web server and the jumpost as well as access the web page hosted on the web server. However, only the jumphost can be used to access the private server.

#### Step 1

Create a VPC including a public subnet, a private subnet, an IGW, an explicit route table.  Also, Adjust the default security group to allow HTTP, HTTPS and SSH

```
[root@localhost 4 Networking in Public Clouds]# ansible-playbook create-vpc.yml --ask-vault-pass
Vault password: 

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

TASK [Adjust the default security group to allow HTTP, HTTPS and SSH] **********************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=6    changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

#### Step 2

Create 3 ec2 instances. An EIP is associated with an ENI which then is attached to the Jumphost.

```
[root@localhost 4 Networking in Public Clouds]# ansible-playbook create-ec2-instances.yml --ask-vault-pass 
Vault password: 

PLAY [create and an EIP and associate with an ENI] *****************************************************************

TASK [get default security group information] **********************************************************************
ok: [localhost]

TASK [get subnet information] **************************************************************************************
ok: [localhost]

TASK [get subnet information] **************************************************************************************
ok: [localhost]

TASK [create an elastic network interface in the public subnet] ****************************************************
changed: [localhost]

TASK [allocate a new elastic IP] ***********************************************************************************
changed: [localhost]

PLAY [Deploy an SSH jumphost, a web server and another vm in private subnet] ***************************************

TASK [Deploy a jumphost] *******************************************************************************************
changed: [localhost]

TASK [Deploy a web server] *****************************************************************************************
changed: [localhost]

TASK [Deploy a private server] *************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=8    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0    
```

#### Step 3

Provision the web server by installing apache2 package.

```
[root@localhost 4 Networking in Public Clouds]# ansible-playbook provision-web-server.yml --ask-vault-pass 
Vault password: 

PLAY [Create a web server group] ***********************************************************************************

TASK [get web EC2 info] ********************************************************************************************
ok: [localhost]

TASK [add web server to the group] *********************************************************************************
skipping: [localhost] => (item={u'network_interfaces': [], u'root_device_type': u'ebs', u'private_dns_name': u'', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'state_transition_reason': u'User initiated (2020-04-16 09:36:54 GMT)', u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'', u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'security_groups': [], u'block_device_mappings': [], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'pending', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'state_reason': {u'message': u'Client.UserInitiatedShutdown: User initiated shutdown', u'code': u'Client.UserInitiatedShutdown'}, u'product_codes': [], u'monitoring': {u'state': u'disabled'}, u'ami_launch_index': 0, u'ena_support': True, u'ebs_optimized': False, u'launch_time': u'2020-04-16T06:50:40+00:00', u'instance_id': u'i-0bd378976c5baf86d', u'instance_type': u't2.micro', u'state': {u'code': 48, u'name': u'terminated'}, u'root_device_name': u'/dev/sda1', u'hypervisor': u'xen', u'client_token': u'web', u'virtualization_type': u'hvm', u'architecture': u'x86_64'}) 
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'web-host', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'52.62.124.242', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:17+00:00', u'volume_id': u'vol-05f42cc6eddcada5e'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'association': {u'public_ip': u'52.62.124.242', u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'network_interface_id': u'eni-09a1253c12396f5ab', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-072854140e5cd99c1', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:16+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.44', u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'52.62.124.242', u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:42:6a:fe:04:38', u'private_ip_address': u'192.168.1.44', u'vpc_id': u'vpc-0a5e972de8ede7756', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-16T10:16:16+00:00', u'instance_id': u'i-0f81d4db31ccc999f', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.44', u'vpc_id': u'vpc-0a5e972de8ede7756', u'product_codes': []})

PLAY [provision the apache server] *********************************************************************************

TASK [update and upgrade apt packages] *****************************************************************************
[WARNING]: The value True (type bool) in a string field was converted to 'True' (type string). If this does not
look like what you expect, quote the entire value to ensure it does not change.
changed: [ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com]

TASK [install apache on ubuntu] ************************************************************************************
changed: [ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com]

PLAY RECAP *********************************************************************************************************
ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

#### Step 4

Test the web server to ensure users from Internet can SSH and HTTP to the server.

```
[root@localhost 4 Networking in Public Clouds]# ansible-playbook test_web.yml --ask-vault-pass 
Vault password: 

PLAY [localhost] ***************************************************************************************************

TASK [get web EC2 info] ********************************************************************************************
ok: [localhost]

TASK [ssh to web server] *******************************************************************************************
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'web-host', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'52.62.124.242', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:17+00:00', u'volume_id': u'vol-05f42cc6eddcada5e'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'association': {u'public_ip': u'52.62.124.242', u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'network_interface_id': u'eni-09a1253c12396f5ab', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-072854140e5cd99c1', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:16+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.44', u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'52.62.124.242', u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:42:6a:fe:04:38', u'private_ip_address': u'192.168.1.44', u'vpc_id': u'vpc-0a5e972de8ede7756', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-16T10:16:16+00:00', u'instance_id': u'i-0f81d4db31ccc999f', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.44', u'vpc_id': u'vpc-0a5e972de8ede7756', u'product_codes': []})

TASK [check if able to connect (GET) to a page and it returns a status 200] ****************************************
ok: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'web-host', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'52.62.124.242', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:17+00:00', u'volume_id': u'vol-05f42cc6eddcada5e'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'association': {u'public_ip': u'52.62.124.242', u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'network_interface_id': u'eni-09a1253c12396f5ab', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-072854140e5cd99c1', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:16+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.44', u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'52.62.124.242', u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:42:6a:fe:04:38', u'private_ip_address': u'192.168.1.44', u'vpc_id': u'vpc-0a5e972de8ede7756', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-16T10:16:16+00:00', u'instance_id': u'i-0f81d4db31ccc999f', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.44', u'vpc_id': u'vpc-0a5e972de8ede7756', u'product_codes': []})

PLAY RECAP *********************************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
#### Step 5

Test the jumphost to ensure users from Internet can SSH to the host.

```
[root@localhost 4 Networking in Public Clouds]# ansible-playbook test_jumphost.yml --ask-vault-pass 
Vault password: 

PLAY [localhost] ***************************************************************************************************

TASK [get jumphost EC2 info] ***************************************************************************************
ok: [localhost]

TASK [ssh to jumphost] *********************************************************************************************
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-234.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'jump-host', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'52.63.130.142', u'tags': {u'Name': u'jumphost'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-52-63-130-142.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:15:54+00:00', u'volume_id': u'vol-0cf34a054a7ee04ba'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'association': {u'public_ip': u'52.63.130.142', u'public_dns_name': u'ec2-52-63-130-142.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'687251871473'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-234.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'network_interface_id': u'eni-0212836ec53fdbe9a', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-0f49b80869a728683', u'delete_on_termination': False, u'attach_time': u'2020-04-16T10:15:53+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.234', u'private_dns_name': u'ip-192-168-1-234.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'52.63.130.142', u'public_dns_name': u'ec2-52-63-130-142.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'687251871473'}, u'primary': True}], u'mac_address': u'02:60:fe:fa:10:aa', u'private_ip_address': u'192.168.1.234', u'vpc_id': u'vpc-0a5e972de8ede7756', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-16T10:15:53+00:00', u'instance_id': u'i-0f4a6826ecfb8610d', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.234', u'vpc_id': u'vpc-0a5e972de8ede7756', u'product_codes': []})

PLAY RECAP *********************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

#### Step 6

Test the private host to ensure only SSH from the jumphost to the host is successful and SSH from the Internet to the host fails. 

Please note the failed play is expected because the SSH was initiated from Internet.

```
[root@localhost 4 Networking in Public Clouds]# ansible-playbook test_private.yml --ask-vault-pass 
Vault password: 

PLAY [localhost] ***************************************************************************************************

TASK [get jumphost EC2 info] ***************************************************************************************
ok: [localhost]

TASK [get private EC2 info] ****************************************************************************************
ok: [localhost]

TASK [SSH from jumphost to private host] ***************************************************************************
changed: [localhost -> ec2-52-63-130-142.ap-southeast-2.compute.amazonaws.com]

TASK [SSH from internet to private host] ***************************************************************************
fatal: [localhost]: FAILED! => {"changed": true, "cmd": ["ssh", "-i", "/home/timsun/ipspace/cloud/4 Networking in Public Clouds/web-keypair.pem", "ubuntu@ec2-13-238-141-45.ap-southeast-2.compute.amazonaws.com"], "delta": "0:02:07.283561", "end": "2020-04-16 20:45:50.349775", "msg": "non-zero return code", "rc": 255, "start": "2020-04-16 20:43:43.066214", "stderr": "Pseudo-terminal will not be allocated because stdin is not a terminal.\r\nssh: connect to host ec2-13-238-141-45.ap-southeast-2.compute.amazonaws.com port 22: Connection timed out", "stderr_lines": ["Pseudo-terminal will not be allocated because stdin is not a terminal.", "ssh: connect to host ec2-13-238-141-45.ap-southeast-2.compute.amazonaws.com port 22: Connection timed out"], "stdout": "", "stdout_lines": []}

PLAY RECAP *********************************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

#### Clean-up

Delete all resources created previously.

```
[root@localhost 4 Networking in Public Clouds]# ansible-playbook clean-up.yml --ask-vault-pass 
Vault password: 

PLAY [localhost] ***************************************************************************************************

TASK [Get EC2 info] ************************************************************************************************
ok: [localhost]

TASK [Terminate instances that were previously launched] ***********************************************************
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-2-86.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-0ce52e80876a3cf4f', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'private-host', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'13.238.141.45', u'tags': {u'Name': u'private'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-13-238-141-45.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:39+00:00', u'volume_id': u'vol-035c4ee01175af221'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'association': {u'public_ip': u'13.238.141.45', u'public_dns_name': u'ec2-13-238-141-45.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-2-86.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-0ce52e80876a3cf4f', u'network_interface_id': u'eni-010851f86597e7e33', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-095e113a8ec4154f5', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:38+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.2.86', u'private_dns_name': u'ip-192-168-2-86.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'13.238.141.45', u'public_dns_name': u'ec2-13-238-141-45.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:b8:5b:1e:a0:dc', u'private_ip_address': u'192.168.2.86', u'vpc_id': u'vpc-0a5e972de8ede7756', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-16T10:16:38+00:00', u'instance_id': u'i-0a2429b460efe4cd1', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.2.86', u'vpc_id': u'vpc-0a5e972de8ede7756', u'product_codes': []})
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-234.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'jump-host', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'52.63.130.142', u'tags': {u'Name': u'jumphost'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-52-63-130-142.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:15:54+00:00', u'volume_id': u'vol-0cf34a054a7ee04ba'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'association': {u'public_ip': u'52.63.130.142', u'public_dns_name': u'ec2-52-63-130-142.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'687251871473'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-234.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'network_interface_id': u'eni-0212836ec53fdbe9a', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-0f49b80869a728683', u'delete_on_termination': False, u'attach_time': u'2020-04-16T10:15:53+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.234', u'private_dns_name': u'ip-192-168-1-234.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'52.63.130.142', u'public_dns_name': u'ec2-52-63-130-142.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'687251871473'}, u'primary': True}], u'mac_address': u'02:60:fe:fa:10:aa', u'private_ip_address': u'192.168.1.234', u'vpc_id': u'vpc-0a5e972de8ede7756', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-16T10:15:53+00:00', u'instance_id': u'i-0f4a6826ecfb8610d', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.234', u'vpc_id': u'vpc-0a5e972de8ede7756', u'product_codes': []})
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'web-host', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'52.62.124.242', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:17+00:00', u'volume_id': u'vol-05f42cc6eddcada5e'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [], u'groups': [{u'group_id': u'sg-0e4680f4c111c7362', u'group_name': u'default'}], u'association': {u'public_ip': u'52.62.124.242', u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01d6bdc1467f8cb6a', u'network_interface_id': u'eni-09a1253c12396f5ab', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-072854140e5cd99c1', u'delete_on_termination': True, u'attach_time': u'2020-04-16T10:16:16+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.44', u'private_dns_name': u'ip-192-168-1-44.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'52.62.124.242', u'public_dns_name': u'ec2-52-62-124-242.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:42:6a:fe:04:38', u'private_ip_address': u'192.168.1.44', u'vpc_id': u'vpc-0a5e972de8ede7756', u'owner_id': u'687251871473'}], u'launch_time': u'2020-04-16T10:16:16+00:00', u'instance_id': u'i-0f81d4db31ccc999f', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.44', u'vpc_id': u'vpc-0a5e972de8ede7756', u'product_codes': []})

TASK [Pause 5 mins until EC2 instances terminated] *****************************************************************
Pausing for 300 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [localhost]

TASK [Get EIP information] *****************************************************************************************
ok: [localhost]

TASK [Get VPC information] *****************************************************************************************
ok: [localhost]

TASK [Get Public Subnet information] *******************************************************************************
ok: [localhost]

TASK [Gather Route Table information] ******************************************************************************
ok: [localhost]

TASK [Release EIP] *************************************************************************************************
changed: [localhost]

TASK [Delete ENI] **************************************************************************************************
changed: [localhost]

TASK [Delete Public Subnet] ****************************************************************************************
changed: [localhost]

TASK [Delete Private Subnet] ***************************************************************************************
changed: [localhost]

TASK [Delete Route Table] ******************************************************************************************
changed: [localhost]

TASK [Delete Internet Gateway] *************************************************************************************
changed: [localhost]

TASK [Delete VPC] **************************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=14   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
