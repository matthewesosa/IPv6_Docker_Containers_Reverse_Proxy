# Internship Sheet 3 - Ansible
 

# Task 1 - Installation+Configuration+Testing of Ansible
a) Install the Ansible provisioning tool version 2.10.8 or higher on your Linux desktop system . Check your installation with the command ansible --version.

b) Ping your local computer with an Ansible command that is as simple as possible. Make a note of the Ansible command you use. Which Ansible module are you using for this? What exactly happens when this command is executed? What does this command check? Does this command also work for your remote server serv-ws22by replacing localhostwith the IPv6 address of serv-ws22 ? Interpret the error message.

c) In your GIT project , create servmgmt-ws22 a subdirectory ansiblethat contains all your Ansible code. Create an inventory file there in YAML that contains hosts.ymlthe local machine (Linux desktop system) and your remote server serv-ws22. Go into the directory ansibleand then run an ansible ping against your server serv-ws22  . What's your command?

d) If you haven't already done so, configure your GIT project's working environment to work comfortably with Ansible. Which points should be considered or useful in this regard?
Hints : set alias , directory structure , ansible.cfg, SSH keys, ...

e) Use a suitable Ansible command to determine which user you are   working as under Ansible on your remote server serv-ws22. What's your command?

f) Modify the command to work with Ansible as root on your server serv-ws22. What's your command? Explain your command.
 

# Task 2 - Ansible role: connection check
Develop a role conn-checkthat tests connectivity to a host. It should be determined whether the host can be reached under the user name used and whether the sudo escalation works. Test this role for your server serv-ws22in a playbook pb-remoteserver-1.yml.

What does your playbook look like and what command do you use to call your playbook?

 

# Task 3 - Ansible Role: Basics
Develop a role basicsthat handles basic configuration of your server. Examples of basic configurations are:

a) Hardening of SSH so that it is no longer possible to log in with a password.

b) Customize bash prompt colors: red = root, green = user.

c) Installing additional base packages, such as the packages qemu-guest-agent, bridge-utils or curl.

d) Adapting the editor used, eg vim, to your personal needs.


Test this role for your server serv-ws22in the playbook pb-remoteserver-2.yml.

 

# Task 4 - Ansible-Role: Docker-CE
Develop an Ansible role docker-cethat installs Docker-CE and Docker-Compose and includes the installing user in the group docker .

Test this role with your server serv-ws22in the playbook pb-remoteserver-3.yml.

What version of Docker-CE and Docker-Compose do you have installed? Are these the current versions?

 

# Task 5 - Questions about Ansible
a) Discuss the use of sudo passwords in terms of necessity, practicality, and security when provisioning with Ansible.

b) Come up with a good concept of how to provide encrypted sudo password in Ansible. Explain your concept.