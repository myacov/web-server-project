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
