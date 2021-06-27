******** Setup new host server (the machine that will act as the Docker/Download server) and client control node (your laptop) *********

1. Install Ubuntu 20.04 server with SSH and Samba services, IMPORTANT! These options are provided during Ubuntu install. Use username ‘serveradmin’
2. Add ‘serveradmin’ to sudo group: 

# sudo usermod -aG sudo serveradmin

3. Generate and copy SSH keys to the machine that will act as the Ainsible control node, this will allow for password-less login (source: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604):

On remote host (future download server):

# ssh-keygen (follow the steps and select empty passphrase)

Now we will copy the SSH keys to the client machine (the one you are installing Ainsible on). NOTE: you may need to create a key-pair on the local machine first.

On client machine:

# ssh-keygen
[enter]
[enter]
[enter]

Now copy the ssh key of the remote host, by typing (on the client machine):

# ssh-copy-id serveradmin@remote_host

You should now be able to login with ssh to the server without password:

# ssh serveradmin@<server-ip>

********** Install ansible on macOS: **************

Install ansible, on macOS Homebrew is the preferred method:

First install Homebrew by pasting the following into a terminal window:

# /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Now install ansible:

# brew install ansible

******** Setup inventory (client machine) ************

The inventory specifies all of the servers we want to manage, with aliases.

Create an inventory file anywhere, for example in the home directory. The only important part is that you then run sensible commands from the same directory where you have your inventory file:

# sudo mkdir ansible
# cd ansible
# sudo nano inventory

ansible_test ansible_host=192.168.0.119 ansible_user=serveradmin

Test the inventory by running the following command:

# ansible all -i /path/to/inventory -m ping

******* Run playbook *********

Playbook can be found at https://github.com/goodyear77/usenet-via-ansible/blob/main/playbook_Usenet_MediaServer_Complete.yaml

Copy the code from GitHub and create a new file locally where you paste the content:

#sudo nano playbook_install.yaml

Paste the content, ctrl+X to exit, enter ‘y” when asked to modify the file and enter.

Now run the ainsible playbook:

# ansible-playbook -i inventory playbook_install.yaml -K

Optional: run the separate playbooks on Github in order 1-4
