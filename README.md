# (Re)Take control of your mails with Mailinabox on Raspberry Pie 4

## Installation guide

1. [Requirements](#1-requirements)
    * [ISP requirements](#isp-requirements)
    * [Registrar requirements](#registrar-requirements)
    * [Hardware requirements](#hardware-requirements)
2. [OS installation](#2-os-installation)
3. [Hardware installation](#3-hardware-installation)
4. [Initial Ubuntu setup](#4-initial-ubuntu-setup)
    * [Step 1: set up appropriate keyboard layout](#step-1-set-up-appropriate-keyboard-layout)
    * [Step 2: restart your machine to enable changes](#step-2-restart-your-machine-to-enable-changes)
    * [Step 3: allow root login](#step-3-allow-root-login)
    * [Step 4: change username and password](#step-4-change-username-and-password)
    * [Step 5: disallow root login](#step-5-disallow-root-login)
5. [Upgrade your system & enable automatic updates](#5-upgrade-your-system--enable-automatic-updates)
6. [Local network access](#6-local-network-access)
    * [Step 1: display the MAC address of your Pie connected network](#step-1-display-the-mac-address-of-your-pie-connected-network)
    * [Step 2: login to your router admin panel](#step-2-login-to-your-router-admin-panel)
    * [Step 3: register a static address](#step-3-register-a-static-address)
    * [Step 4: SSH access](#step-4-ssh-access)
7. [Remote network access](#7-remote-network-access)
    * [Step 1: set up port forwarding](#step-1-set-up-port-forwarding)
    * [Step 2: disable SMTP blocking](#step-2-disable-smtp-blocking)
    * [Step 3: configure your reverse DNS](#step-3-configure-your-reverse-dns)
8. [Restrict SSH access](#8-restrict-ssh-access)
    * [Step 1: create an SSH key](#step-1-create-an-ssh-key)
    * [Step 2: add your public key to your machine's authorized keys](#step-2-add-your-public-key-to-your-machines-authorized-keys)
    * [Step 3: disallow SSH password authentication](#step-3-disallow-ssh-password-authentication)
9. [Set up Mailinabox](#9-set-up-mailinabox)
    * [Step 1: install Mailinabox](#step-1-install-mailinabox)
    * [Step 2: login to your admin panel](#step-2-login-to-your-admin-panel)
    * [Step 3: disable backups](#step-3-disable-backups)
10. [Configure your DNS zone](#10-configure-your-dns-zone)
    * [Step 1: access your external DNS configuration](#step-1-access-your-external-dns-configuration)
    * [Step 2: replicate this configuration in your DNS zone](#step-3-replicate-this-configuration-in-your-dns-zone)
11. [Request TLS certificates from Let's Encrypt](#11-request-tls-certificates-from-lets-encrypt)

## Maintenance guide

* [Maintenance: backup your data manually](#maintenance-backup-your-data-manually)
  * [Step 1: disable access to the machine](#step-1-disable-access-to-the-machine)
  * [Step 2: trigger a manual backup](#step-2-trigger-a-manual-backup)
  * [Step 3: re-enable access to the machine](#step-3-re-enable-access-to-the-machine)
* [Maintenance: restore your data manually](#maintenance-restore-your-data-manually)
  * [Step 1: ensure Mailinabox is running](#step-1-ensure-mailinabox-is-running)
  * [Step 2: disable access to the machine before restoring the data](#step-2-disable-access-to-the-machine-before-restoring-the-data)
  * [Step 3: restore last backup](#step-3-restore-last-backup)
  * [Step 4: re-enable access to the machine](#step-4-re-enable-access-to-the-machine)
  * [Step 5: reconfigure Mailinabox](#step-5-reconfigure-mailinabox)

## 1. Requirements

### ISP requirements

[Back to top ↑](#installation-guide)

In order to host your emails, your Internet Service Provider (ISP)
needs to match some requirements:

* Your ISP must give you a static IP address
* Your ISP must allow you to configure your reverse DNS
* Your ISP must not block ports 25 and 465 (SMTP)

In France, the ISP called "[Free](https://free.fr/assistance/54.html)"
matches these requirements.

### Registrar requirements

[Back to top ↑](#installation-guide)

In order to host your emails, you'll need a domain name that
you can buy from a domain name registrar.
Your domain name registrar needs to match some requirements:

* Your registrar must offer you to host your DNS zone
* Your registrar must allow you to set up
NS, A, AAAA, SPF, TXT, DKIM, TLSA, SSHFP, SRV, and DMARC records in your DNS zone

The registrar called "[OVH](https://www.ovh.com/fr/order/domain/)"
matches these requirements.

*Note: prefer a domain name that will be dedicated to this usage
(do not use it for other things like web hosting). This is better to
control your sender reputation that will prevent your emails from
being flagged as SPAM.*

### Hardware requirements

[Back to top ↑](#installation-guide)

* 1 × [Raspberry Pie 4 (4 GB RAM)](https://www.kubii.fr/les-cartes-raspberry-pi/2772-nouveau-raspberry-pi-4-modele-b-4gb-kubii-0765756931182.html)
* 1 × [Raspberry Pie 4 case](https://www.kubii.fr/boitiers-et-supports/2681-boitier-officiel-pour-raspberry-pi-4-kubii-3272496298583.html)
* 1 x [Raspberry Pie 4 power supply](https://www.kubii.fr/14-chargeurs-alimentations-raspberry/2678-alimentation-officielle-153w-usb-c-pour-raspberry-pi-4-kubii-3272496300002.html)
* 1 x [SanDisk microSD card 16 Go class 10](https://www.kubii.fr/raspberry-pi-microbit/2587-carte-micro-sd-sandisk-16go-classe10-taux-de-transfert-80mb-kubii-619659161613.html)
* 1 × [Ugreen ethernet cable Cat 7 10Gbps](https://www.amazon.fr/UGREEN-11260-Ethernet-Nintendo-Consoles/dp/B00QV1F1C4)
* 1 × [Ugreen USB 3.0 SD card reader 30333](https://www.amazon.fr/UGREEN-Lecteur-M%C3%A9moire-CompactFlash-Compatible/dp/B01ANDA8GE/)
(optional if you already have a SD card reader)
* 1 × [Ugreen micro HDMI to HDMI adapter](https://www.amazon.fr/UGREEN-Femelle-Adaptateur-Supporte-Ethernet/dp/B00B2HORKE/)
* 1 × [Ugreen HDMI cable 0,9 m](https://www.amazon.fr/UGREEN-Ethernet-18Gbps-Supporte-Compatible/dp/B07DBYDJQF/)
* 1 × keyboard
* 1 × screen

## 2. OS installation

[Back to top ↑](#installation-guide)

1. Download the [Ubuntu 18.04 64 bits image](https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.4&architecture=arm64+raspi3)
for Raspberry Pie 4.
2. Put your microSD card in your SD card reader and connect it to your computer.
3. Follow [instructions](https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.4&architecture=arm64+raspi3)
in order to flash the downloaded image onto the microSD card.
4. Disconnect everything when the process is finished.

## 3. Hardware installation

[Back to top ↑](#installation-guide)

1. Put your microSD card containing the installed OS in your Raspberry Pie.
2. Put your Raspberry Pie into its case.
3. Connect your Raspberry Pie to your router with the ehernet cable.
4. Connect your keyboard and screen to your Raspberry Pie.
7. Connect the power adaptators of your Rasberry Pie and screen.

Your Ubuntu machine will boot up!

## 4. Initial Ubuntu setup

[Back to top ↑](#installation-guide)

You can login with "ubuntu" as default login and password. You may
experienced login errors if you try to login directly as soon as the
prompt is displayed. This is because some background installations
processes are not completed yet. Wait until SSH keys are displayed on
the screen then press "Enter". You will be prompted to change your
password immediately after login.

*Note: Ubuntu 18.04 for Raspberry Pie 4 is by default using a "qwerty"
keyboard layout which might not be your layout. To prevent loosing access
to your account, I suggest you to set up something universal like
"hellohello" for now, set up appropriate keyboard layout and change
the password later.*

### Step 1: set up appropriate keyboard layout

[Back to top ↑](#installation-guide)

```bash
sudo dpkg-reconfigure keyboard-configuration
```

### Step 2: restart your machine to enable changes

[Back to top ↑](#installation-guide)

```bash
sudo reboot
```

### Step 3: allow root login

[Back to top ↑](#installation-guide)

Because you might want something more personal than "ubuntu" as a
username, you can change them. We will need the
root account for that so we will temporary allow root login:

```bash
sudo passwd root
logout
```

### Step 4: change username and password

[Back to top ↑](#installation-guide)

Login as root and use these commands to rename your username and
change your password to something more meaningful to you:

```bash
# Change username
usermod -l <newUserName> ubuntu

# Rename home directory
usermod -d /home/<newUserName> -m <newUserName>

# Change password
passwd <newUserName>
```

### Step 5: disallow root login

[Back to top ↑](#installation-guide)

When it's done, you can disallow root login. For security reasons,
you should never leave your root account accessible.

```bash
# Disable root account
passwd -l root

# Logout from root user
logout
```

## 5. Upgrade your system & enable automatic updates

[Back to top ↑](#installation-guide)

Upgrading the system will ensure that all your softwares are
using latest security fixes.

```bash
sudo apt update && sudo apt dist-upgrade -y
```

Then, we'll enable automatic updates to be sure that all
futures security fixes are installed as soon are they are released:

<!-- markdownlint-disable MD013 -->
```bash
# Make a backup of the config files
sudo cp /etc/apt/apt.conf.d/10periodic /etc/apt/apt.conf.d/.10periodic.backup
sudo cp /etc/apt/apt.conf.d/50unattended-upgrades /etc/apt/apt.conf.d/.50unattended-upgrades.backup

# Download updates when available
sudo sed -i'.tmp' -e 's,APT::Periodic::Download-Upgradeable-Packages "0";,APT::Periodic::Download-Upgradeable-Packages "1";,g' /etc/apt/apt.conf.d/10periodic

# Clean apt cache every week
sudo sed -i'.tmp' -e 's,APT::Periodic::AutocleanInterval "0";,APT::Periodic::AutocleanInterval "7";,g' /etc/apt/apt.conf.d/10periodic

# Enable automatic updates once downloaded
sudo sed -i'.tmp' -e 's,//\s"${distro_id}:${distro_codename}-updates";,        "${distro_id}:${distro_codename}-updates";,g' /etc/apt/apt.conf.d/50unattended-upgrades

# Ask for email
read -r -p 'Enter your email for notifications on update errors: ' email

# Enable email notifications
sudo sed -i'.tmp' -e "s,//Unattended-Upgrade::Mail \"root\";,Unattended-Upgrade::Mail \"${email}\";,g" /etc/apt/apt.conf.d/50unattended-upgrades

# Enable email notifications only on failures
sudo sed -i'.tmp' -e 's,//Unattended-Upgrade::MailOnlyOnError "true";,Unattended-Upgrade::MailOnlyOnError "true";,g' /etc/apt/apt.conf.d/50unattended-upgrades

# Remove unused kernel packages when needed
sudo sed -i'.tmp' -e 's,//Unattended-Upgrade::Remove-Unused-Kernel-Packages "false";,Unattended-Upgrade::Remove-Unused-Kernel-Packages "true";,g' /etc/apt/apt.conf.d/50unattended-upgrades

# Remove unused dependencies when needed
sudo sed -i'.tmp' -e 's,//Unattended-Upgrade::Remove-Unused-Dependencies "false";,Unattended-Upgrade::Remove-Unused-Dependencies "true";,g' /etc/apt/apt.conf.d/50unattended-upgrades

# Reboot when needed
sudo sed -i'.tmp' -e 's,//Unattended-Upgrade::Automatic-Reboot "false";,Unattended-Upgrade::Automatic-Reboot "true";,g' /etc/apt/apt.conf.d/50unattended-upgrades

# Set reboot time to 5 AM
sudo sed -i'.tmp' -e 's,//Unattended-Upgrade::Automatic-Reboot-Time "02:00";,Unattended-Upgrade::Automatic-Reboot-Time "05:00";,g' /etc/apt/apt.conf.d/50unattended-upgrades

# Remove temporary files
sudo rm /etc/apt/apt.conf.d/10periodic.tmp
sudo rm /etc/apt/apt.conf.d/50unattended-upgrades.tmp
```
<!-- markdownlint-enable MD013 -->

## 6. Local network access

[Back to top ↑](#installation-guide)

If you want to access your machine from another computer on your local
network instead of directly with a keyboard and a screen, you'll need
to reserve a static IP address for it. If not, the attributed IP address
inside your network will change each time your router starts up, so it's quite annoying.

### Step 1: display the MAC address of your Pie connected network

[Back to top ↑](#installation-guide)

```bash
ifconfig | grep -i ether
```

### Step 2: login to your router admin panel

[Back to top ↑](#installation-guide)

Login to your router according to your ISP and/or router documentation.

*Note: for the "Free" ISP, the URL of the admin panel is: [https://subscribe.free.fr/login/](https://subscribe.free.fr/login/).*

### Step 3: register a static address

[Back to top ↑](#installation-guide)

Register the static IP address according to your ISP and/or router documentation.

The IP address you choose must not be in the DHCP server range.
You can start with something like "192.168.0.101".

*Note: for the "Free" ISP, once logged in, go under
"Ma Freebox" > "Paramétrer mon routeur Freebox" > "Redirections / Baux DHCP"
and fill the form like bellow.*

![register-static-ip](https://user-images.githubusercontent.com/6952638/76153321-a0767700-60ca-11ea-8816-c548047064e4.png)

### Step 4: SSH access

[Back to top ↑](#installation-guide)

With this, you should now be able to access your Raspberry Pie from
your computer (which must be connected to the same network as your Pie)
through SSH with this command:

```bash
ssh <yourUserName>@<yourIpAddress>
```

## 7. Remote network access

### Step 1: set up port forwarding

[Back to top ↑](#installation-guide)

For now, your router is the target of all requests made to your public
IP address, and it does not do anything with them.

We need to instruct it to redirect the traffic to the Pie so that we can
access it from outside the local network, from the Internet.

According to your ISP/router documentation, redirect the traffic from
ports 80, 443, 22, 25, 587, 993, 4190 and 53 to your static local IP address.

*Note: for the "Free" ISP, once logged in, go under
"Ma Freebox" > "Paramétrer mon routeur Freebox" > "Redirections / Baux DHCP"
and fill the form like bellow.*

![port-forwarding](https://user-images.githubusercontent.com/6952638/76295516-0c1c3800-62b5-11ea-98ef-f40c1b95e18f.png)

### Step 2: disable SMTP blocking

[Back to top ↑](#installation-guide)

Your ISP can also block SMTP ports by default to prevent
hijacked computers from sending SPAM.

If it's your case, this will prevent you from sending emails.

According to your ISP/router documentation, disable SMTP blocking.

*Note: for the "Free" ISP, once logged in, go under
"Ma Freebox" > "Blocage du port SMTP sortant".*

![smtp-block](https://user-images.githubusercontent.com/6952638/76679230-08532300-65df-11ea-8a10-2971d531f588.png)

### Step 3: configure your reverse DNS

[Back to top ↑](#installation-guide)

![reverse-dns](https://user-images.githubusercontent.com/6952638/76679325-e6a66b80-65df-11ea-8506-9da1c45869a5.png)

The reverse DNS is the way we can retreive your domain name from your
IP address. You can view the full explanation of what is it on [this blog post](https://www.leadfeeder.com/blog/what-is-reverse-dns-and-why-you-should-care/).

This process is used by anti-spam systems to check if an IP address
associated with a sender address for example (john@example.com)
is related to its domain name (example.com).

The Mailinabox software that we'll install later will use a specific
subdomain to install its stuffs: `box.<yourdomainname>`
and this will be your reverse DNS.

For example, you want to send mail from "john@example.com",
your reverse DNS will be `box.example.com`.

According to your ISP/router documentation, configure your reverse DNS.

*Note: for the "Free" ISP, once logged in, go under
"Ma Freebox" > "Personnaliser mon reverse DNS".*

![reverse-dns](https://user-images.githubusercontent.com/6952638/76679855-2d966000-65e4-11ea-87a5-8c5fe4a8beff.png)

## 8. Restrict SSH access

[Back to top ↑](#installation-guide)

The root account is disabled but now, anybody can potentially access
your machine through your user account if they found your password.

Your user account is not root but have some sudo privileges. So if it's
compromised, an attacker can do pretty much everything he want with your
machine, including accessing your datas.

To protect your account from being accessed by another person that you,
we will disable SSH password authentication and only let your authorized
computers to login with your user account (note that this will not
disable password authentication direcly with a keyboard and a screen connected).

On each computer you want to access your Raspberry Pie with, follow these steps:

### Step 1: create an SSH key

[Back to top ↑](#installation-guide)

If you don't have an SSH key (look for "`~/.ssh/id_rsa`" and
"`~/.ssh/id_rsa.pub`" files), use this command to generate one:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

At the prompts, you can press "Enter" to use default settings.

### Step 2: add your public key to your machine's authorized keys

[Back to top ↑](#installation-guide)

From your computer, run:

```bash
ssh <yourUserName>@<yourIpAddress> "echo '$(cat ~/.ssh/id_rsa.pub)' | tee -a ~/.ssh/authorized_keys > /dev/null"
```

If you try to reconnect to your machine through SSH, you should now be
able to login without being asked for a password. SSH will automatically
log you if your local SSH key matches one indicated in the remote
"~/.ssh/authorized_keys" file.

### Step 3: disallow SSH password authentication

[Back to top ↑](#installation-guide)

Now that you have an passwordless SSH access to your Raspberry Pie,
we will disallow password authentication. This will prevent all non
authorized computers from being able to access it through SSH.

I recommend you to backup your "~/.ssh/id_rsa" and "~/.ssh/id_rsa.pub"
files in a safe place, for example in a password manager app protected
by a master password.

This will prevent you from loosing access to your Pie if your only
authorized computer dies (in that case, you only have to copy these
files in your next computer to allow connections from it).

To disable SSH password authentication, connect to your Pie and run:

```bash
# Update the config and save the original in a "/etc/ssh/sshd_config.backup" file
sudo sed -i'.backup' -e 's/PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config

# Restart SSH
sudo service ssh restart
```

## 9. Set up Mailinabox

### Step 1: install Mailinabox

[Back to top ↑](#installation-guide)

```bash
# Install some dependencies
sudo apt install -y libffi-dev python-paramiko

# Install Mailinabox v0.45
git clone https://github.com/mail-in-a-box/mailinabox
cd ~/mailinabox
git checkout v0.45
sudo ./setup/start.sh
```

During the install process, you will be asked for your domain name and
the main email address that will be set up as the admin account of the system.

### Step 2: login to your admin panel

[Back to top ↑](#installation-guide)

When the install process ends, you will be prompted to access your
Mailinabox admin panel through your public IP address:

```text
https://<yourIP>/admin
```

Accept the security warning and login with your credentials.

### Step 3: disable backups

[Back to top ↑](#installation-guide)

Then go to "System" > "Backup status" and disable backups:

![disablebackup](https://user-images.githubusercontent.com/6952638/79106306-889dad00-7d72-11ea-9eed-e0b557eeece6.png)

We need to disable Mailinabox backups because they are stored on
the machine itself and can rapidly take a lot of space as your data grows.
We will configure an external backup system later.

## 10. Configure your DNS zone

If you go now in the "Status Checks" tab, you will see red issues everywhere.

We did instruct our router to redirect the traffic from our public
IP address to the local IP address of the Pie. But we did not instruct
anybody to redirect traffic from our domain name to our public IP address.

Let's do that.

### Step 1: access your external DNS configuration

[Back to top ↑](#installation-guide)

By default, Mailinabox configure everything to host your DNS
configuration directly on your machine.

This can be an issue in case of a breakdown, because there is no redundancy.
If your Pie dies prematurely, all the instructions regarding to where
your domain name should send your traffic is lost. And reset
everything is not as easy as it sounds.

By experience, I found it safer to host the DNS configuration directly on
the registrar (which has redundancy). If your machine dies or if you want
to host your datas elsewhere, having the configuration hosted on the
registrar side allows you to do this smoothly.

To do that, go in your Mailinabox admin panel, go under
"System > External DNS" to display your external DNS configuration.

![external-dns](https://user-images.githubusercontent.com/6952638/76702663-c94ecb80-66cb-11ea-94a3-496370c4682e.png)

### Step 2: replicate this configuration in your DNS zone

[Back to top ↑](#installation-guide)

Now you need to configure your DNS zone extacly like this. Go into your
registrar admin panel and add all these records according to its
documentation.

This looks like this with OVH:
![dns-zone](https://user-images.githubusercontent.com/6952638/76681053-d34ecc80-65ee-11ea-9232-cdcfe8efaa4a.png)

Even if the modifications are made instantly in the interface, the DNS
configuration can make several hours (up to 24 hours) to be fully
propagated around the world, so wait few hours before continue.

## 11. Request TLS certificates from Let's Encrypt

[Back to top ↑](#installation-guide)

Once your DNS configuration is propagated and OK, you can ask TLS
certificates in order to access your machine with your own domain over HTTPS.

Go under "System > TLS (SSL) Certificates" and hit the "Provision" button
to automatically get a TLS certificates for your domains.

![ssl-certs](https://user-images.githubusercontent.com/6952638/76702693-221e6400-66cc-11ea-9512-626e755d187b.png)

After that, you may see an error. You just need to access your admin
panel directly with your domain name instead of the IP address:

```text
https://box.<yourDomainName>/admin
```

Now, if you go to "Status Checks", you should have green lines everywhere:

![status-checks](https://user-images.githubusercontent.com/6952638/76681225-127e1d00-65f1-11ea-8476-ec94783a4821.png)

*Note: I have one red line on the reverse DNS check because Mailinabox
checks that the reverse DNS is set for both IPV4 and IPV6 but my ISP
only allow me to set up reverse DNS for IPV4 yet. It's not yet and
issue because IPV6 is almost unused for now.*


## Maintenance: backup your data manually

### Step 1: disable access to the machine

[Back to top ↑](#maintenance-guide)

To ensure you have a final backup, first block
access to your machine to all services besides SSH.

```bash
# Reset all firewall rules
sudo ufw reset

# Only allow SSH (if not, we loose access to the machine)
sudo ufw allow 22

# Reactive the firewall
sudo ufw enable
```

### Step 2: trigger a manual backup

[Back to top ↑](#maintenance-guide)

You can trigger a manual backup with this:

<!-- markdownlint-disable MD013 -->
```bash
# Login as root
sudo su root

# Ask for backup machine username
read -r -p 'Enter your backup machine username: ' backupusername

# Ask for backup machine IP or hostname
read -r -p 'Enter your backup machine IP address or hostname: ' backuphost

# Run backup
/root/rsync-time-backup/rsync_tmbackup.sh /mnt/md0/user-data ${backupusername}@${backuphost}:/home/${backupusername}/backup-data

# Exit root
exit
```
<!-- markdownlint-enable MD013 -->

### Step 3: re-enable access to the machine

[Back to top ↑](#maintenance-guide)

```bash
# Reset all firewall rules
sudo ufw reset

# Allow all services
sudo ufw allow 22/tcp
sudo ufw allow 53
sudo ufw allow 25/tcp
sudo ufw allow 587/tcp
sudo ufw allow 993/tcp
sudo ufw allow 995/tcp
sudo ufw allow 4190/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Reactive the firewall
sudo ufw enable
```

## Maintenance: restore your data manually

### Step 1: ensure Mailinabox is running

If you are restoring your data on a new machine,
ensure that you've performed all [installation steps](#installation-guide).

### Step 2: disable access to the machine before restoring the data

[Back to top ↑](#maintenance-guide)

To ensure there will be no conflict, block
access to your machine to all services besides SSH.

```bash
# Reset all firewall rules
sudo ufw reset

# Only allow SSH (if not, we loose access to the machine)
sudo ufw allow 22

# Reactive the firewall
sudo ufw enable
```

### Step 3: restore last backup

[Back to top ↑](#maintenance-guide)

<!-- markdownlint-disable MD013 -->
```bash
# Login as root
sudo su root

# Ask for backup machine username
read -r -p 'Enter your backup machine username: ' backupusername

# Ask for backup machine IP or hostname
read -r -p 'Enter your backup machine IP address or hostname: ' backuphost

# Get last backup path
last_backup_path=$(ssh "${backupusername}@${backuphost}" "ls -td /home/${backupusername}/backup-data/*/ | head -1")

# Replace existing data with the ones from the last backup
rsync -aP --delete "${backupusername}@${backuphost}:${last_backup_path}" /mnt/md0/user-data

# Logout from root
exit
```
<!-- markdownlint-enable MD013 -->

### Step 4: re-enable access to the machine

[Back to top ↑](#maintenance-guide)

```bash
# Reset all firewall rules
sudo ufw reset

# Allow all services
sudo ufw allow 22/tcp
sudo ufw allow 53
sudo ufw allow 25/tcp
sudo ufw allow 587/tcp
sudo ufw allow 993/tcp
sudo ufw allow 995/tcp
sudo ufw allow 4190/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Reactive the firewall
sudo ufw enable
```

### Step 5 reconfigure Mailinabox

[Back to top ↑](#maintenance-guide)

```bash
sudo mailinabox
```
