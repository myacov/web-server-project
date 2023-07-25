# web-server-project
## Objective: Running a web server in a Linux VM with Vagrant 
Every company has a website. As a DevOps engineer, we should be able to provision a web server with automation and publish a website onto it.
## Desired Learning outcomes
-	Creating and using a virtual machine.
-	Get familiar with Linux command line.
-	finding and installing programs in Ubuntu.
-	Using a text editor.
-	Understanding the basics of web servers and networking.
-	Use automation to perform a task predictably and repeatably.
## The tools (the stack)
| CHOSEN TOOL  | USE | WHY |
| ------------- | ------------- | ------------- |
| 🧊 VirtualBox  | Virtualisation   | Easy to use  |
| **v** Vagrant  | Automation   | Lightweight  |
| 🐧 Linux (Ubuntu) | OS | popular Linux distribution |
| 🌏 Apache HTTP Server | Web server  | popular web server |

## Prerequisites:
-	Computer with 8GB RAM and approximately 10GB free disk space, (for Linux in a virtual machine).
-	VirtualBox installation. (Instructions on the VirtualBox website to download and install VirtualBox for your platform).
-	Vagrant installation.

## Creating a virtual machine using vagrant
1.	We’re going to use VirtualBox to create and manage a virtual machine, and Vagrant will help us do this from the terminal.
2.	open terminal. Create a new directory and cd into it:
```bash
mkdir myproject
cd myproject
```
initialize a new vagrant machine using *vagrant init* and we name the “box” we want to use:
```bash
vagrant init ubuntu/jammy64
```
3.boot up the virtual machine with: 
```bash
vagrant up
```

## Install the web server
using  Ubuntu’s package manager **apt**.
1.  search for Apache HTTP Server:
```bash
apt search apache 
```
2. Before we install Apache, we should do a package update.
```bash
sudo apt update
```
3. Install Apache:
```bash
sudo apt install apache2 -y
```
4.	check the status of the Apache HTTP Server “service”:
```bash
systemctl status apache2
```
5.	make a test request using the curl program. connect to localhost and port 80 (default port for HTTP):
```bash
curl -v localhost:80
```
## Set up networking
To access our web server from outside the VM, we’ll need to give the VM an IP address that we can reach from outside the VM.
### How to see your VM’s network configuration
```bash
ip addr
```

1. Back on the terminal In Vagrantfile, add this line to set the network configuration
 specify an IP address manually :
  ```bash
  config.vm.network "private_network", ip: "192.168.33.10"
  ```
2. reload the configuration and restart the VM:
   ```bash
   vagrant reload
   ```
3. SSH into it again and run ip addr
4. 4.	SSH into it again: vagrant ssh
5.	Now if you re-run the IP addr command:
   ```bash
   ip addr
   ```
6. A new network interface has appeared
## Access the website from a web browser


## Add some website content

### Copy across the new website
1.	Create a directory called tmp underneath the location where you 
mkdir tmp

2.	Inside the tmp directory, download a wesite template.

wget https://www.tooplate.com/zip-templates/2136_kool_form_pack.zip
3.	Unzip the file
unzip 2136_kool_form_pack.zip 
(if needed install unzip: sudo apt install unzip -y)

4.	copy the website files to /var/www/html.:
(the target directory is owned by root.)
sudo cp -r tmp/2136_kool_form_pack/* /var/www/html/
5.	Now when we reload the web browser, the web server now shows our web site, instead of the default page.

## Automating this process - Provisioning
Vagrant has its own built-in provisioner which will run scripts and automation for us.
It can run commands, copy files, and replace the manual work of setting up the VM.
If you’ve created your Vagrantfile with vagrant init then you might see an example provisioning section at the bottom.
In Vagrant, a block of provisioning commands begins with config.vm.provision.
Follow these steps below to add some provisioning steps, so that Apache HTTP Server is installed, and the website files are copied to the correct location.
### Add some provisioning steps to the virtual machine
1.	In the Vagrantfile, add the provisioning code like this, just before the end command in the Vagrantfile:

   ```bash
   config.vm.provision "shell", inline: <<-SHELL
     sudo apt update
     sudo apt install apache2 unzip -y
     mkdir -p /tmp/data
     cd /tmp/data
     wget https://www.tooplate.com/zip-templates/2136_kool_form_pack.zip
     sudo unzip -o 2136_kool_form_pack.zip
     sudo cp -r /tmp/data/2136_kool_form_pack/* /var/www/html/
     systemctl restart apache2
     rm -rf /tmp/data
   SHELL
```
2. run vagrant destroy and vagrant up again.

## Future Improvements:
- Introducing variables to make code more general
- configure DNS to make the website publicly accessible
