[< Back](./README.md)

How to basic setup Pop OS 20.04 LTS.

## Git

Install Git and Git Flow

```
sudo apt install git git-flow
```

Configure your user information

```
git config --global user.name <YOUR NAME>
git config --global user.email <YOUR@EMAIL.com>
```

## Docker

Install Docker, and enable it

```
sudo apt install docker.io docker-compose
sudo systemctl start docker.service
sudo systemctl enable docker.service
```

Add your user to the `docker` group, and restart

```
sudo usermod -aG docker $USER
```

> Permissions won't be applied immediately, logoff or reboot

## Windows VM

Microsoft makes available a set of VM's with Internet Explorer. These are legal for testing purposes.

First, install VirtualBox

```
sudo apt install virtualbox virtualbox-qt virtualbox-ext-pack
```

Then add your user to the VirtualBox group

```
sudo adduser $USER vboxusers
```

Download the [VM](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/). IE11/Win7 is tested and less prone to activation issues.

Unzip the file, and import it with VirtualBox `File > Import Appliance`. Configure the VM as desired (RAM, enable USB2.0, etc). Start it, install Guest Additions (`Devices - Insert Guest Additions CD image`), install the software you need, and create a snapshot. The license is valid for 90 days, and can be rearmed about 5 times. But having a snapshot around is a good idea, to have a starting point in case the activation fails.

## PHP

To have multiple PHP versions available on the system, we'll need a PPA. In this example, we'll install PHP 5.6, run

```
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php5.6
```

If you need to add CURL for this specific PHP version, run

```
sudo apt install php5.6-curl
```

> Typically, to install another extension, just add it to `php5.6-`

To change between PHP versions, use `update-alternatives`. In the following example, we'll be changing PHP, PHP-CGI and PHAR versions

```
sudo update-alternatives --config php
sudo update-alternatives --config php-cgi
sudo update-alternatives --config phar
sudo update-alternatives --config phar.phar
```

Check the available version with `php -v`

## Nice to haves

### Backups

Install Timeshift

```
sudo apt install timeshift
```

Start the app and follow the wizard. It should be simple, select RSYNC method and the target partition where you want to store your backup. Select the periodicity of the backups, and how many backups you want to store.

### Hibernate

First, you need to have the correct amount of swap. Check [here](https://help.ubuntu.com/community/SwapFaq#How_much_swap_do_I_need.3F) the size needed for your swap partition.

Then, it should be a matter of just adding the following file

```
sudo nano /etc/polkit-1/localauthority/50-local.d/com.popos.enable-hibernate.pkla
```

with this contents

```
[Enable hibernate by default in power]
Identity=unix-user:*
Action=org.freedesktop.upower.hibernate
ResultActive=yes

[Enable hibernate by default in logind]
Identity=unix-user:*
Action=org.freedesktop.login1.hibernate;org.freedesktop.login1.handle-hibernate-key;org.freedesktop.login1;org.freedesktop.login1.hibernate-multiple-sessions;org.freedesktop.login1.hibernate-ignore-inhibit
ResultActive=yes
```

Logout and login, go to `Settings` > `Power`, and you should be able to change the `Power Button Action` to `Hibernate`

### VS Code

Install VS Code

```
sudo apt install code
```

### Fonts

Install some MS fonts

```
sudo apt install ttf-mscorefonts-installer
sudo fc-cache -f -v
```

Install Fira Code, a nice ligature compatible font

```
sudo apt install fonts-firacode
```

### ZSWAP

Enabling ZSWAP can help prevent system hangups when transfering massive loads of data from RAM to SWAP (especially when using VMs)

```
sudo kernelstub -a zswap.enabled=1
```

Reboot and check the status with

```
dmesg | grep -i zswap
```

To disable

```
sudo kernelstub -d zswap.enabled=1
```

---

Copyright 2020 jfrmm.
