## Introduction

This task creates an EC2 with ubuntu AMI and then installs apache2 web server. After the initialization, the server grabs an html file from the S3 bucket. The html file (index.html) is a static website containing an image pointing to S3 bucket as well. 

#### Preparation

Create an S3 bucket and upload the index.html and an image to it.

![Image of S3 Bucket](https://github.com/TimSun18/Networking-in-Public-Cloud-Deployments/blob/master/3%20Compute%20Infrastructure/images/S3_Bucket.png?raw=true)

##### Step 1

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
