# Internship Sheet 1 - Home Laboratory

## TASK 2 - VM with IPv6

 `sudo nano /etc/netplan/00-installer-config.yaml`
  
```
 This is the network config written by 'subiquity'

    network:
      ethernets:
        ens18:
          dhcp6: false
	      addresses:
	        - 2001:638:408:200::FF6C::1/64
	      gateway6: 2001:638:408:200::1
        ens19:
          dhcp4: true
      version: 2
      renderer: networkd

```

`sudo netplan apply`


### Explanation:

-'dhcp6:false' - For the network interface ens18, IPv6 is assigned manually hence dhcp6 is set to false.

-'dhcp4:true'- For the network interface ens19, I want IPv4 to be dynamically assigned; hence dhcp4 is set to true.

-'renderer:networkd' - The network configuration abstraction renderer. 'Networkd' can manage both static and dynamic connections.


## TASK 3 - VM with SSH
### (3c) Test ssh access via public key authentication. How did you do that?
`ssh migbin2s@2001:638:408:200:FF6C::1`
  
### (3d) Turn off password authentication for openssh. Which line or lines did you have to change for this?
`sudo nano /etc/ssh/sshd_config`

```
-PasswordAuthentication no

-UsePAM no

-KbdInteractiveAuthentication no

```
`sudo systemctl restart ssh`


### (3d) Test that password authentication is actually turned off. What error message do you get when you try to log in with a password?
--Permission denied (publickey).