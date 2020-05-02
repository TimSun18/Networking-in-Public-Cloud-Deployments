## Introduction
- ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `#f03c15`
This task creates 3 IPv4/IPv6 dual-stack EC2 instances, among which 2 instances (web and jumphost) are in a public subnet and 1 private host is placed in the private subnet.

The target is users from Internet are able to SSH to the IPv6 addresses of web server and the jumpost as well as access the web page hosted on the web server. However, only the jumphost can be used to access the private server.

#### Step 1
Create a VPC.
```
[timsun@controller 5 IPv6 in Cloud]$ ansible-playbook vpc.yml 

PLAY [localhost] ***************************************************************************************************

TASK [create a VPC] ************************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

#### Step 2
Because ansible module ec2_vpc_net cannot be used to enable IPv6 CIDR block when creating the VPC, need to manually enable the Amazon provided IPv6 CIDR block.

#### Step 3
Configure the network for VPC, create the EC2 instances and provision the web server. Because ansible module ec2_vpc_route_table cannot add IPv6 default route, add a pause during the ansible play in order to take time manually adding the IPv6 default route.
```
[timsun@controller 5 IPv6 in Cloud]$ ansible-playbook main.yml 

PLAY [Create EC2 instances] ****************************************************************************************

TASK [create a public subnet] **************************************************************************************
ok: [localhost]

TASK [create a public subnet] **************************************************************************************
changed: [localhost]

TASK [create a private subnet] *************************************************************************************
changed: [localhost]

TASK [create an Internet gateway] **********************************************************************************
changed: [localhost]

TASK [create a public route table] *********************************************************************************
[WARNING]: Skipping purging route {u'GatewayId': 'local', u'Origin': 'CreateRouteTable', u'State': 'active',
u'DestinationIpv6CidrBlock': '2406:da1c:711:6300::/56'} because it has no destination cidr block. To remove VPC
endpoints from route tables use the ec2_vpc_endpoint module.
changed: [localhost]

TASK [create default security group to allow HTTP, HTTPS and SSH] **************************************************
changed: [localhost]

TASK [Because ansible ec2_vpc_route_table doesn't support IPv6, need to manually add IPv6 default route] ***********
Pausing for 300 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [localhost]

TASK [Query publuc subnet information] *****************************************************************************
ok: [localhost]

TASK [Query private information] ***********************************************************************************
ok: [localhost]

TASK [Deploy a jumphost] *******************************************************************************************
changed: [localhost]

TASK [Deploy a web server] *****************************************************************************************
changed: [localhost]

TASK [Deploy a private server] *************************************************************************************
changed: [localhost]

TASK [Query web EC2 info] ******************************************************************************************
ok: [localhost]

TASK [Add web servers to the group] ********************************************************************************
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01fc50dcc434f20c8', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'0af719359798493a94f3664fda32efa3', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'13.238.194.174', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:24:51+00:00', u'volume_id': u'vol-033fc4e2248525b22'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [{u'ipv6_address': u'2406:da1c:711:6300:e5e7:47a7:d064:e792'}], u'groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'13.238.194.174', u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01fc50dcc434f20c8', u'network_interface_id': u'eni-0fc018c62637d15b3', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-0ab3cad9a284dc670', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:24:50+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.141', u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'13.238.194.174', u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:c9:e3:88:c1:f8', u'private_ip_address': u'192.168.1.141', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'owner_id': u'687251871473'}], u'launch_time': u'2020-05-02T11:24:50+00:00', u'instance_id': u'i-0bd1450cd8c22209c', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.141', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'product_codes': []})

PLAY [Provision web server] ****************************************************************************************

TASK [update and upgrade apt packages] *****************************************************************************
[WARNING]: The value True (type bool) in a string field was converted to 'True' (type string). If this does not
look like what you expect, quote the entire value to ensure it does not change.
changed: [ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com]

TASK [install apache on ubuntu] ************************************************************************************
changed: [ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com]

PLAY RECAP *********************************************************************************************************
ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=14   changed=9    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
          : ok=8    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0    
```

#### Step 4

Test IPv6 connectivity to each host. Please note SSH from Internet to the private host was failed, which is an expected result. The failure was ignored during the play.
```
[timsun@controller 5 IPv6 in Cloud]$ ansible-playbook tests.yml 

PLAY [localhost] ***************************************************************************************************

TASK [get jumphost EC2 info] ***************************************************************************************
ok: [localhost]

TASK [ssh to jumphost IPV6 address] ********************************************************************************
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-50.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01fc50dcc434f20c8', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'43e9f3ad35fc42ba848eb24174e1ba2b', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'13.55.25.161', u'tags': {u'Name': u'jumphost'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-13-55-25-161.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:22:16+00:00', u'volume_id': u'vol-0efbad0b3e08012c2'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [{u'ipv6_address': u'2406:da1c:711:6300:9c1c:476d:bc9c:206a'}], u'groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'13.55.25.161', u'public_dns_name': u'ec2-13-55-25-161.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-50.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01fc50dcc434f20c8', u'network_interface_id': u'eni-0dbb2f059b095e7f3', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-0959efd1bb6b3356a', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:22:15+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.50', u'private_dns_name': u'ip-192-168-1-50.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'13.55.25.161', u'public_dns_name': u'ec2-13-55-25-161.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:84:46:12:54:4e', u'private_ip_address': u'192.168.1.50', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'owner_id': u'687251871473'}], u'launch_time': u'2020-05-02T11:22:15+00:00', u'instance_id': u'i-019000d30f05bd06f', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.50', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'product_codes': []})

TASK [print the result] ********************************************************************************************
ok: [localhost] => {
    "msg": "SSH from the Internet to jumphost is successful."
}

TASK [get web EC2 info] ********************************************************************************************
ok: [localhost]

TASK [ssh to web server] *******************************************************************************************
changed: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01fc50dcc434f20c8', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'0af719359798493a94f3664fda32efa3', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'13.238.194.174', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:24:51+00:00', u'volume_id': u'vol-033fc4e2248525b22'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [{u'ipv6_address': u'2406:da1c:711:6300:e5e7:47a7:d064:e792'}], u'groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'13.238.194.174', u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01fc50dcc434f20c8', u'network_interface_id': u'eni-0fc018c62637d15b3', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-0ab3cad9a284dc670', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:24:50+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.141', u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'13.238.194.174', u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:c9:e3:88:c1:f8', u'private_ip_address': u'192.168.1.141', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'owner_id': u'687251871473'}], u'launch_time': u'2020-05-02T11:24:50+00:00', u'instance_id': u'i-0bd1450cd8c22209c', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.141', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'product_codes': []})

TASK [check if able to connect (GET) to a page and it returns a status 200] ****************************************
ok: [localhost] => (item={u'root_device_type': u'ebs', u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'cpu_options': {u'threads_per_core': 1, u'core_count': 1}, u'security_groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'monitoring': {u'state': u'disabled'}, u'subnet_id': u'subnet-01fc50dcc434f20c8', u'ebs_optimized': False, u'state': {u'code': 16, u'name': u'running'}, u'source_dest_check': True, u'client_token': u'0af719359798493a94f3664fda32efa3', u'virtualization_type': u'hvm', u'root_device_name': u'/dev/sda1', u'public_ip_address': u'13.238.194.174', u'tags': {u'Name': u'web'}, u'key_name': u'web-keypair', u'image_id': u'ami-02a599eb01e3b3c5b', u'ena_support': True, u'hibernation_options': {u'configured': False}, u'capacity_reservation_specification': {u'capacity_reservation_preference': u'open'}, u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'block_device_mappings': [{u'ebs': {u'status': u'attached', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:24:51+00:00', u'volume_id': u'vol-033fc4e2248525b22'}, u'device_name': u'/dev/sda1'}], u'metadata_options': {u'http_endpoint': u'enabled', u'state': u'applied', u'http_tokens': u'optional', u'http_put_response_hop_limit': 1}, u'placement': {u'availability_zone': u'ap-southeast-2a', u'tenancy': u'default', u'group_name': u''}, u'ami_launch_index': 0, u'hypervisor': u'xen', u'network_interfaces': [{u'status': u'in-use', u'description': u'', u'interface_type': u'interface', u'ipv6_addresses': [{u'ipv6_address': u'2406:da1c:711:6300:e5e7:47a7:d064:e792'}], u'groups': [{u'group_id': u'sg-052a8edfc855c9d8d', u'group_name': u'WEB_SG'}], u'association': {u'public_ip': u'13.238.194.174', u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'source_dest_check': True, u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'subnet_id': u'subnet-01fc50dcc434f20c8', u'network_interface_id': u'eni-0fc018c62637d15b3', u'attachment': {u'status': u'attached', u'device_index': 0, u'attachment_id': u'eni-attach-0ab3cad9a284dc670', u'delete_on_termination': True, u'attach_time': u'2020-05-02T11:24:50+00:00'}, u'private_ip_addresses': [{u'private_ip_address': u'192.168.1.141', u'private_dns_name': u'ip-192-168-1-141.ap-southeast-2.compute.internal', u'association': {u'public_ip': u'13.238.194.174', u'public_dns_name': u'ec2-13-238-194-174.ap-southeast-2.compute.amazonaws.com', u'ip_owner_id': u'amazon'}, u'primary': True}], u'mac_address': u'02:c9:e3:88:c1:f8', u'private_ip_address': u'192.168.1.141', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'owner_id': u'687251871473'}], u'launch_time': u'2020-05-02T11:24:50+00:00', u'instance_id': u'i-0bd1450cd8c22209c', u'instance_type': u't2.micro', u'architecture': u'x86_64', u'state_transition_reason': u'', u'private_ip_address': u'192.168.1.141', u'vpc_id': u'vpc-0f86e616bb2bf3238', u'product_codes': []})

TASK [print SSH result] ********************************************************************************************
ok: [localhost] => {
    "msg": "SSH from the Internet to the web host is successful."
}

TASK [print HTTP result] *******************************************************************************************
ok: [localhost] => {
    "msg": "HTTP from the Internet to the web host is successful."
}

TASK [get jumphost EC2 info] ***************************************************************************************
ok: [localhost]

TASK [get private EC2 info] ****************************************************************************************
ok: [localhost]

TASK [SSH from jumphost to private host] ***************************************************************************
changed: [localhost -> ec2-13-55-25-161.ap-southeast-2.compute.amazonaws.com]


  
    ec2_instance_info:
      region: "{{ region }}"
      filters:
        "tag:Name": jumphost
    register: jumphost_info

  - name: get private EC2 info
    ec2_instance_info:
      region: "{{ region }}"
      filters:
        "tag:Name": private
    register: private_info

  - name: SSH from jumphost to private host
    shell:
       ssh -i /home/ubuntu/web-keypair.pem ubuntu@"{{ private_info.instances[0].network_interfaces[0].ipv6_addresses[0].ipv6_address }}"
       exit
    delegate_to: "{{ jumphost_info.instances[0].public_dns_name }}"

  - name:  SSH from internet to private host
    command: ssh -i "{{ ansible_ssh_private_key_file }} {{ ansible_ssh_user }}@{{ private_info.instances[0].network_interfaces[0].ipv6_addresses[0].ipv6_address }}"
    async: 30
    poll: 5
    ignore_errors: yes
Entering Ex mode.  Type "visual" to go to Normal mode.                                            
:q
TASK [SSH from internet to private host] ***************************************************************************
fatal: [localhost]: FAILED! => {"ansible_job_id": "530609118517.61912", "changed": true, "cmd": ["ssh", "-i", "/home/timsun/ipspace/cloud/5 IPv6 in Cloud/web-keypair.pem ubuntu@2406:da1c:711:6364:2bb0:b3d9:b51b:7ba9"], "delta": "0:00:00.009639", "end": "2020-05-02 23:06:48.398305", "finished": 1, "msg": "non-zero return code", "rc": 255, "start": "2020-05-02 23:06:48.388666", "stderr": "Warning: Identity file /home/timsun/ipspace/cloud/5 IPv6 in Cloud/web-keypair.pem ubuntu@2406:da1c:711:6364:2bb0:b3d9:b51b:7ba9 not accessible: No such file or directory.\nusage: ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]\n           [-D [bind_address:]port] [-E log_file] [-e escape_char]\n           [-F configfile] [-I pkcs11] [-i identity_file]\n           [-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec]\n           [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address]\n           [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]\n           [user@]hostname [command]", "stderr_lines": ["Warning: Identity file /home/timsun/ipspace/cloud/5 IPv6 in Cloud/web-keypair.pem ubuntu@2406:da1c:711:6364:2bb0:b3d9:b51b:7ba9 not accessible: No such file or directory.", "usage: ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]", "           [-D [bind_address:]port] [-E log_file] [-e escape_char]", "           [-F configfile] [-I pkcs11] [-i identity_file]", "           [-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec]", "           [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address]", "           [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]", "           [user@]hostname [command]"], "stdout": "", "stdout_lines": []}
...ignoring

PLAY RECAP *********************************************************************************************************
localhost                  : ok=12   changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1   
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
