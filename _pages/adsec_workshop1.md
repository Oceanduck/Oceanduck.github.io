---
layout: single
classes: wide smallfont
title:  "Public Presentations and Talks"
permalink: /adsec_workshop1/
---
----
### Introduction  

In this lab we will now use Ansible from our Linux `adsecvm` to configure the Windows Systems.  

We will configure two systems

1. `dc1` - this is the domain controller system 
2. `client1` - this is the client system

### Lab Requirements
We will use the `adsecvm` to remotely configure the domain controller `dc1` and Client system `client1`.   You will need the `adsec` vm downloaded from the link to configure the `dc1` and `client1`.


### Preparing the Domain Controller for Ansible  

Please run the following PowerShell command to enable WinRM on the Domain Controller.   
You will need Internet connectivity from the dc1 system to configure this. IF you have followed the Lab0A you should have this configured. If you need help please reach out to `support@rudrasec.io`.

```
iwr -useb https://raw.githubusercontent.com/ansible/ansible-documentation/refs/heads/devel/examples/scripts/ConfigureRemotingForAnsible.ps1 | iex 
```

### Running Ansible from adsecvm to configure Domain Controller

Now we can use `adsecvm` to configure the domain controller. We have created a playbook called `ansible-playbook dc.yml` to configure the domain controller. Please note: this will only work if the Windows 2019 system is configured with the right user account. 

Run the following commands to invoke the Virtualenv to run Ansible on the `adsecvm`. 

![enable virtualenv](./images/lab0B-configureansible.png)

No we can run the ansible playbook, we will run the `dc.yml` runbook, by running ansible. 

```
ansible-playbook dc.yml
```

![Run dc runbook](./images/lab0B-ansible.png)

Once Ansible Runbook execution is completed, the Windows 2019 system will be renamed and configured as a domain controller. 

### Preparing the Client system for Ansible  

We can run the following PowerShell command to enable WinRM on the Domain Controller.   
You will need Internet connectivity from the client1 system to configure this.

```
iwr -useb https://raw.githubusercontent.com/ansible/ansible-documentation/refs/heads/devel/examples/scripts/ConfigureRemotingForAnsible.ps1 | iex
```

No we can run the ansible playbook, we will run the `client.yml` runbook. 

```
ansible-playbook client.yml
```