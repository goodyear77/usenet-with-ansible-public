******** Setup new host server (the machine that will act as the Docker/Download server) and client control node (your laptop) *********

1. Install Ubuntu 20.04 server with SSH and Samba services, IMPORTANT! These options are provided during Ubuntu install. Use username ‘serveradmin’
2. Add ‘serveradmin’ to sudo group: 

# sudo usermod -aG sudo serveradmin

3. Generate and copy SSH keys to the machine that will act as the Ainsible control node, this will allow for password-less login (source: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604):

On remote host (future download server):

# ssh-keygen (follow the steps and select empty passphrase)

Now we will copy the SSH keys to the client machine (the one you are installing Ainsible on). NOTE: you may need to create a key-pair on your local client machine first.

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

Playbook can be found at https://github.com/goodyear77/usenet-with-ansible-public/blob/main/playbook_Usenet_MediaServer_Complete_anon.yaml

Copy the code from GitHub and create a new file locally where you paste the content:

#sudo nano playbook_install.yaml

Paste the content, ctrl+X to exit, enter ‘y” when asked to modify the file and enter.

Now run the ainsible playbook:

# ansible-playbook -i inventory playbook_install.yaml -K

Optional: run the separate playbooks on Github in order 1-4

************ Post installation tasks ***********

* OPTIONAL if you are replacing a server: change DHCP-server so the new MAC-address gets the old IP, that way all bookmarks still work.
    * Move the old machine to a new IP
    * Add the new MAC and lock it to the old IP
    * On first old and then new machine (release and renew IP on old machine then the same on new machine): # /etc/init.d/networking restart
    * Optional: you can also release the old IP and then renew it, but ONLY IF YOU ARE ON A TERMINAL since you will lose IP connectivity: # sudo dhclient -r # sudo dhclient

* Access all containers to make sure they are up:
    - Sabnzbd is available at: <server-ip>:8080
    - Sonarr is available at: <server-ip>:8989
    - Radarr is available at: <server-ip>:7878
    - Plex is available at: <server-ip>:32400/web
    - Portainer is available at: <server-ip>:9000
        * You need to create an admin account and connect to docker as a first step

* Plex: make sure the following is setup:
    - Turn on automatic scan and trash can
    - Change backup location to /backups (under settings->schedualed tasks)
    - Change transcode location to /transcode under settings (show advanced)->Transcode->Transcoder temporary directory
* Radarr:
    - Restore from backup zip-file, found under the backup files on MedaiStation: ESXi->usenet_linux_config_files->radarr->manual (under ‘automatic’ if automatic backup was used to create the backupp)
* Sonarr:
    - Restore from backup zip-file, found under the backup files on MedaiStation: ESXi->usenet_linux_config_files->sonarr->manual (under ‘automatic’ if automatic backup was used to create the backup)

