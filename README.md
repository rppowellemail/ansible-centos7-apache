# ansible-centos7-apache

Demonstration of using ansible to install apache on centos7

Done on Mac OSX 10.12.6

## Setup - localhost

Using:
 * python 3.6.1
 * virtualenv 15.1.0 (ansible-centos7-apache-env)
 * ansible 2.3.2.0

## Setup - target

### Create CentOS7 instance

AWS ami - CentOS 7 (x86\_64) - with Updates HVM

Tags - Name: ansible-centos7-apache

Security
 * SSH
 * HTTP
 * HTTPS

### Launch & verify target

Get the instance infomation from AWS EC2 console:

```
ec2-13-59-243-60.us-east-2.compute.amazonaws.com
13.59.243.60
```

Use `ssh` to connect:

```
ssh -i ~/Desktop/kp/2017-08-31-ansible-centos7-apache.pem centos@ec2-13-59-243-60.us-east-2.compute.amazonaws.com
```

Should be logged into target instance.

## Run Ansible

Update the instance info into `hosts` file:

```
[webservers]
ec2-13-59-243-60.us-east-2.compute.amazonaws.com
```


Run ansible playbook to install apache into instances in `hosts` file:

```
ansible-playbook -i hosts install_apache.yml --user=centos --private-key=ansible-centos7-apache.pem 
```

To manually install on specific instance `ec2-13-59-243-60.us-east-2.compute.amazonaws.com`:

```
ansible-playbook -i 'ec2-13-59-243-60.us-east-2.compute.amazonaws.com,' install_apache.yml --user=centos --extra-vars "variable_host=ec2-13-59-243-60.us-east-2.compute.amazonaws.com" --private-key=ansible-centos7-apache.pem 
```

Script runs - verify output:

```
PLAY [yum install apache on CentOS7] *********************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************
changed: [ec2-13-59-243-60.us-east-2.compute.amazonaws.com]

TASK [yum install apache] ********************************************************************************************************
changed: [ec2-13-59-243-60.us-east-2.compute.amazonaws.com]

TASK [enable apache service] *****************************************************************************************************
changed: [ec2-13-59-243-60.us-east-2.compute.amazonaws.com]

TASK [restart apache] ************************************************************************************************************
changed: [ec2-13-59-243-60.us-east-2.compute.amazonaws.com]

PLAY RECAP ***********************************************************************************************************************
ec2-13-59-243-60.us-east-2.compute.amazonaws.com : ok=4    changed=4    unreachable=0    failed=0   
```

Verify server is running by going to webpage:

* http://ec2-13-59-243-60.us-east-2.compute.amazonaws.com/

----

References:
 * https://stackoverflow.com/questions/33222641/override-hosts-variable-of-ansible-playbook-from-the-command-line
 * https://cloudacademy.com/blog/building-ansible-playbooks-step-by-step/

