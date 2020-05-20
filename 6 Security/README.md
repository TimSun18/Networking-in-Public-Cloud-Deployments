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
Create two filter condition referenced by a WAF rule that is then called by a WAF ACL.
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

### 4. Session logging
#### Requirements
Log all sessions to and from SSH jump host.

#### Solution
Create an IAM role with permission of creating cloudwatch log groups and writing logs. Then create a flow log binding associating with the network interface of the Jumphost 
```
[timsun@controller 6 Security]$ ansible-playbook logging.yml 

PLAY [localhost] ***************************************************************************************************

TASK [Create an IAM role with custom trust relationship] ***********************************************************
[WARNING]: Module did not set no_log for update_password
changed: [localhost]

TASK [Assign a policy called cloudwatch to the new role] ***********************************************************
changed: [localhost]

TASK [Query publuc subnet information] *****************************************************************************
ok: [localhost]

TASK [Deploy a jumphost] *******************************************************************************************
changed: [localhost]

TASK [create a network interface flow log for the jumphost] ********************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************
localhost                  : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
