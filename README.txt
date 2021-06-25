******** Setup new host server with SSH keys (the machine that will act as the Docker/Download server on ESXi) *********

1. Install Ubuntu server with SSH and Samba services. User username ‘serveradmin’
2. Add ‘serveradmin’ to sudo group:  # sudo usermod -aG sudo serveradmin
3. Generate and copy SSH keys to the machine that will act as the Ainsible control node, this will allow for password-less login (source: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604):  # ssh-keygen (follow the steps and select empty passphrase)  Copy the SSH keys to the client machine (the one you are installing Ainsible on). NOTE: you may need to create a key-pair on the local machine first:  On client machine: # ssh-keygen [enter] [enter] [enter]  Now copy the ssh key of the remote host, by typing (on the client machine): #ssh-copy-id serveradmin@remote_host

You should now be able to login with ssh to the server without password 

******** Setup inventory (client machine) ************

Install ansible if you haven't done so.

The inventory specifies all of the servers we want to manage, with aliases.

Create an inventory file anywhere, for example in the home directory (change IP to the IP of your remote host):

#sudo mkdir ansible
# cd ansible
#sudo nano inventory

ansible_test ansible_host=<server-ip> ansible_user=serveradmin

Test the inventory by running the following command:

# ansible all -i /path/to/inventory -m ping

Test uptime for a specific host:

# ansible ansible_test -i /path/to/inventory -m ping

******* Run playbook *********

Playbooks can be found at https://github.com/goodyear77/usenet-via-ansible

# ansible-playbook -i inventory playbook_Usenet_MediaServer_Complete.yaml -K 
Optional: run the separate playbooks in order 1-4