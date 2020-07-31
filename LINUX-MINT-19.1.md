[< Back](./README.md)

How to basic setup Linux Mint 19.1.

> This how-to should also apply to Ubuntu 18.04 LTS.

## DNS

This should work by setting the DNS in `Network Settings > Wifi/LAN Settings > IPv4`.

Then, `Disable automatic DNS`, and add the desired DNS in the available input box.

> Bear in mind, when using multiple DNS servers, that Linux does not use DNS servers in the order they are defined in this list!

## VPN

When using OpenVPN, it should be just a matter of using `Network Connections > Add > Import a saved VPN configuration`. You then can add your VPN name, username and password.

## Git

Install Git and Git Flow

```
sudo apt install git git-flow
```

To avoid "ghost changes" when dual-booting Windows/Linux, boot to Linux and

```
git config --global core.autocrlf input
```

Then, boot to Windows and

```
git config --global core.autocrlf true
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

## Nice to haves

### Backups

Linux Mint should point you towards using the included backup tool, as soon as you use the software update tool. Just follow the wizard with the default settings.

> In Ubuntu, an alternative could be to install Timeshift, with `sudo apt install timeshift`. It may be needed to enable `cronie` system service.

### Hibernate

Ubuntu based distros have hibernation disabled by default, since 16.04 LTS, because... problems? At your own risk, [enable hibernate.](https://ubuntu-mate.community/t/hibernate-resume-from-hibernation-ubuntu-mate-18-04/16924)

### VS Code

Install VS Code

```
sudo snap install code --classic
```

### Fonts

Install some MS compatible fonts

```
sudo apt install fonts-crosextra-carlito fonts-crosextra-caladea
```

Install Fira Code, a nice ligature compatible font

```
sudo apt install fonts-firacode
```

Install Source Code Pro, a nice monospaced font

First, create a `install_source_code_pro.sh` file with this code

```
#!/bin/sh
# Userland mode (~$USER/), (~/).

# ~/.fonts is now deprecated and that
#FONT_HOME=~/.fonts
# ~/.local/share/fonts should be used instead
FONT_HOME=~/.local/share/fonts

echo "installing fonts at $PWD to $FONT_HOME"
mkdir -p "$FONT_HOME/adobe-fonts/source-code-pro"
# find "$FONT_HOME" -iname '*.ttf' -exec echo '{}' \;

(git clone \
   --branch release \
   --depth 1 \
   'https://github.com/adobe-fonts/source-code-pro.git' \
   "$FONT_HOME/adobe-fonts/source-code-pro" && \
fc-cache -f -v "$FONT_HOME/adobe-fonts/source-code-pro")
```

Then run

```
sh install_source_code_pro.sh
```

### DisplayLink

DisplayLink provides official support up to Ubuntu 18.04 LTS. So, just head to the [download link](https://www.displaylink.com/downloads/ubuntu), and install their package. It should work out of the box.

> You must have **Secure Boot** disabled in your BIOS.

> If you're having issues setting up the displays, you may use `arandr`, which is a GUI for `xrandr` for helping with multiple display setup. Install with `sudo apt install arandr`.

> You may encounter trailing/flickering mouse cursor issues, still no definitive solution, but there's a workaround, `sudo systemctl restart lightdm` after login.

### Swappiness

The Linux kernel handles modern day SSDs well. But you can read about swap [here](https://chrisdown.name/2018/01/02/in-defence-of-swap.html), and tweak it if you want.

Edit/touch this file

```
sudo nano /etc/sysctl.d/99-sysctl.conf
```

Add the line

```
vm.swappiness=<value-between-0-and-100>
```

Restart and check if swappiness is correctly set

```
cat /sys/fs/cgroup/memory/memory.swappiness
```

---

Copyright 2020 jfrmm.
