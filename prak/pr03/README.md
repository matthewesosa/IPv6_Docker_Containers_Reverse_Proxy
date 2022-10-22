# Task 1 - Installation+Configuration+Testing of Ansible
## (1b.i) Ping your local computer with an Ansible command that is as simple as possible. Make a note of the Ansible command you use. 
* ansible localhost -m ping

## (1b.11) Which Ansible module are you using for this? 
 Ansible ping module

## (1b.ii) What exactly happens when this command is executed? 
Ansible returns the success or failure of its requirement check on the target machine in the form of a defined/default string or error, respectively. This does not work on ICMP, rather it works by default on SSH or any other defined connection method.

## (1b.iii) What does this command check? Does this command also work for your remote server serv-ws22 by replacing localhost with the IPv6 address of serv-ws22 ? Interpret the error message.
The command checks for the requirements listed below on the target host machine and returns a value based on success or failure.
- Network Connectivity
- Usable Python Availability
- Current User Login

The command did not work for my remote server serv-ws22  because my remote server is not listed in the 'hosts' file (Inventory file)

## (1c) Go into the directory ansibleand then run an ansible ping against your server serv-ws22  . What's your command?
* ansible VM_server -m ping

# Task 2 - Ansible role: connection check
## What command do you use to call your playbook?
* ansible-playbook pb-remoteserver-1.yml
  
 # Task 3 - Ansible Role: Basics
 Find the playbook pb-remoteserver-2.yml in the ansible folder.