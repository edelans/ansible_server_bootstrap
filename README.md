This repo is an ansible playbook intended to automate the initial configuration of a fresh ubuntu box.
Tested with :
+ 14.04
+ 16.04

#What does it do ?
This playbook is inspired and largely based on [Bryan Kennedy](http://plusbryan.com)'s excellent post [My First 5 Minutes On A Server; Or, Essential Security for Linux Servers](http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers).

To know exactly what it does, check the comments in the [main task file](roles/bootstrap/tasks/main.yml). Every task performed is centralized there.

#How to use this playbook

##Assumptions

+ Your are targeting a fresh install of Ubuntu 14.04 server.
+ `root` login has not been disabled yet (the playbook is configured to login as `root` and to ask you for your password).
+ You have installed the packages `sshpass` and `ansible` on your desktop system :
```
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install sshpass ansible
```

##Clone this repository

```
$ git clone https://github.com/edelans/server_bootstrap
```

##Generate configuration files based on sample config files

```
$ cp hosts.ini.sample hosts.ini
$ cp group_vars/server.yml.sample group_vars/server.yml
```

##Edit configuration files to your needs
Edit configuration files (`hosts.ini` and `group_vars/server.yml`) with your own configuration. You can change the defaut hostname (```server```) to whatever you want.


##Execute the playbook

```
ansible-playbook server_bootstrap.yml --extra-vars="hosts=server"
```

##Reboot your server
After successfully bootstrapping and securing your server, reboot server for eventual kernel updates.


#Troubleshooting

##How to generate an SSH Key

On Linux run the command:

    ssh-keygen

Follow the onscreen instructions to generate your SSH key pairs on your desktop computer. By default, the ssh-keygen utility will save your private key to `~/.ssh/id_rsa` and the public key to `~/.ssh/id_rsa.pub`. If you don't want to use a passphrase simply press 'Enter' when prompted. Of course, it is wiser to use a passphrase...

Copy the the public key file (copy the string in the file with default name : `~/.ssh/id_rsa.pub`) to the group_vars file :

```
ssh_users:
  - name: a name to identify this user/key
    user: login on remote server for which the following key enables access
    key: "ssh-rsa AAAAB.....== email@address.com"
```

Don't forget to add the newly created key or you will experience a `Permission denied (publickey)` error later on. If you entered a passphrase in the previous step you will need to enter it to perform the operation:

    ssh-add ~/.ssh/id_rsa

#Contributing
Issues and PRs wellcome :wink: !
