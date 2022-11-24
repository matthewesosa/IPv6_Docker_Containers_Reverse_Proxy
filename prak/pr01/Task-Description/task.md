# Internship Sheet 1 - Home Laboratory
Our home laboratory consists of the following components:

Linux desktop system
editor system
GIT
Remote VM
configurable domain
For comfortable work in the internship, these components must be coordinated and meet the necessary performance requirements.

 

Linux desktop system
For the internship you need a Linux desktop system so that the software used in the internship ( ansible ) runs.

If you only have a Windows computer, you can install the Linux desktop system as a virtual system. Your Windows computer should have at least 8 GB of RAM. First install a virtualization environment (e.g. VMware , or VirtualBox ). In this environment you can then install Ubuntu desktop on a virtual machine.

Also, your Linux desktop system should be connected to the IPv6 network.

 

editor system
Everyone has their own preferences here. I am using Visual Studio Code .

 

GIT
There are quite a number of GIT providers, e.g.:

GitHub - "The world's leading software development platform",
GitLab - "200% Faster DevOps Lifecycle",
Bitbucket - "Built for professional teams" or
Free Software Laboratory Gitlab - by H-BRS.
We will use the free software laboratory Gitlab in the internship , since every H-BRS student has access to it. If you do not yet have an account in the Free Software Labor Gitlab , create one and familiarize yourself with the system. Create a project servmgmt-ws22that you will use to code your lab assignments. Add me as another owner to your project servmgmt-ws22 .

 

Please solve the following practical exercises and record your solution, where it makes sense, in a file /servmgmt-ws22/prak/pr01/README.md. In the case of this Lab Sheet 1, this only applies to Task 2(b), 3(c), 3(d) and 3(e). Proceed in the same way for all subsequent internship sheets.

 

# Task 1 - Installing Ubuntu 22.04 on the VM
You have received a virtual machine and an IPv6 address for your VM and an IPv6 address space from the network laboratory ( https://netlab.inf.hochschule-bonn-rhein-sieg.de/ ).

a)Install Ubuntu 22.04 on your virtual machine. To do this, use the iso ubuntu-22.04.1-live-server-amd64.isoprovided on the Proxmox server.
b)Choose as the server name serv-ws22and your email abbreviation as the user name (eg dmeier2s).
c)Please make sure you choose a good password for the user name.
d)Activate the installation of the OpenSSH server package (openssh-server package).
e)After the restart, remove the iso from the CD/DVD drive.
 

# Task 2 - VM with IPv6
a)Configure the IPv6 interface of your VM via the Proxmox console or via the university VPN  with Netplan. You can find your IPv6 address in LEA under the following link . Configure as IPv6 router:
 gateway6: "2001:638:408:200::1" .
b)Specify the content of your configuration file /etc/netplan/00-installer-config.yamland interpret it?
c)Test (after setting up ssh, see Task 3) whether you can access your machine remotely over IPv6 using ssh.
d)You can use the taskchecker (LEA: https://lea.hochschule-bonn-rhein-sieg.de/goto.php?target=webr_1305668&client_id=db_040811 ) to check that your VM can be reached from outside via IPv6 .
 

# Task 3 - VM with ssh
a)Create an RSA key pair with openssh and the command . ssh-keygen(Modern alternative: ed25519 key pair.)
b)Bring the public key for the user migbin2s  into the file ~/.ssh/authorized_keys. on your server serv-ws22 
c)Test ssh access via public key authentication. How did you do that?
d)Turn off password authentication for openssh . You have /etc/ssh/sshd_configto adapt the file for this. Which line or lines did you have to change for this?
e)Test that password authentication is actually turned off. What error message do you get when you try to log in with a password?
 

# Task 4 - domain
a)Set up your domain to point to your server.
b)Test access to your server via the domain using the ping command.