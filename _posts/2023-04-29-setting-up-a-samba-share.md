---
layout: post
title: Setting Up a Samba Share
date: 2023-04-29 22:57 +0200
categories: [Tutorials, Samba Setup]
tags:
- Samba
- SMB
---

# Setting Up a Samba Share with Port Forwarding

## Introduction {#introduction}

In this beginner-friendly tutorial, I will guide you through setting up a Samba share with port forwarding. By the end, you will be able to install and configure Samba on your Linux server, configure port forwarding on your router, and access the Samba share from your client device.

## Prerequisites {#prerequisites}

Before we begin, ensure that you have the following:

A Linux server (e.g., Ubuntu, CentOS, or Debian) with root or sudo access.
A router with port forwarding capabilities.
A client device (e.g., Windows, macOS, or Linux) to access the shared files.
## Step 1: Installing Samba {#step1}

First, we need to install Samba on the Linux server. Depending on your Linux distribution, the installation command may vary.

For Ubuntu/Debian:

```bash 
sudo apt-get update && sudo apt-get install samba 
```

For CentOS:

```bash
sudo yum install epel-release && sudo yum install samba 
```

## Step 2: Configuring Samba Share {#step2}

First, create a new directory for our Samba share, and set the appropriate permissions:

```bash 
sudo mkdir /srv/samba/share 
sudo chown mickhat:mickhat /srv/samba/share 
sudo chmod 777 /srv/samba/share 
```

Now, let's create a backup of the original Samba configuration file and then edit the file:

```bash 
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak 
sudo vi /etc/samba/smb.conf 
```

Add the following configuration at the end of the file:

```bash 
[MyShare] 
path = /srv/samba/share 
read only = no 
browsable = yes 
valid users = mickhat 
create mask = 0777 
directory mask = 0777 
```

Save and close the file. Then, restart the Samba service:

```bash 
sudo systemctl restart smbd 
```

## Step 3: Configuring Port Forwarding {#step3}

To configure port forwarding on your router, follow these general steps:

Log in to your router's admin panel.
Locate the "Port Forwarding" or "Virtual Servers" settings.
Add a new rule with the following parameters:
External Port: 445
Internal Port: 445
Protocol: TCP
Internal IP Address: [Your Linux server's local IP address]
Save the configuration and reboot the router if required.

## Step 4: Accessing the Samba Share {#step4}

For Windows:

Open File Explorer and enter the following in the address bar: 

`\\[Your Linux server's public IP address]\MyShare`

For macOS:

Open Finder, click on "Go" in the menu bar, and select "Connect to Server."
Enter the following: 

`smb://[Your Linux server's public IP address]/MyShare`

For Linux:

Open your file manager and connect to the share using the following address: 

`smb://[Your Linux server's public IP address]/MyShare`

## Conclusion {#conclusion}

Congratulations! You have successfully set up a Samba share with port forwarding. Now you can easily share files between your devices, even when they are not on the same local network.