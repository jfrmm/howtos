[< Back](./README.md)

How to basic setup Manjaro 18.0.4.

## Welcome

Congrats on you Manjaro system! This distro is based on [Arch](https://www.archlinux.org/), a nice rolling distro launched in 2002, but Manjaro add a bit more of stability to it, by testing the packages for 2 weeks prior to releasing them. Manjaro aims at simplicity and ease of use. A great feature is MHWD, which makes sure that all the drivers needed for your hardware are installed.

Manjaro has two repositories, ABS (Arch Base Repository), which is the official one, and AUR (Arch User Repository), which is non-official and community maintained, but has shown that is typically safe and has every package you can think of.

The [Manjaro Wiki](https://wiki.manjaro.org) has a lot of help to setup your system, but the [Arch Wiki](https://wiki.archlinux.org) can also apply, and is one of the best in the Linux world. Also, the [Manjaro Forum](https://forum.manjaro.org) has a great community, ready to help.

## Post install

It's recommended to run a system update after install. The safest way is to jump to a TTY (`CTRL`+`ALT`+`F2`) and update

```
sudo pacman -Syyu
```

## DNS

This should work by setting the DNS in `Network Settings > Wifi/LAN Settings > IPv4`.

Then, `Disable automatic DNS`, and add the desired DNS in the available input box.

> Bear in mind, when using multiple DNS servers, that Linux does not use DNS servers in the order they are defined in this list!

## VPN

When using OpenVPN, it should be just a matter of using `Network Connections > Add > Import a saved VPN configuration (*.conf)`. You then can add your VPN name, username and password.

Sometimes, the provided `.ovpn` file can't be imported by the `Network Settings` manager. The simplest way to load it is to run in the terminal

```
sudo openvpn --config <config-file.ovpn>
```

## Git

Install Git and Git Flow

```
sudo pacman -Syu git
pamac build gitflow-avh
```

To avoid diffs when sharing code between Linux and Windows, boot to **Linux** and

```
git config --global core.autocrlf input
```

Then, boot to **Windows** and

```
git config --global core.autocrlf true
```

## Docker

Install and enable Docker service

```
sudo pacman -Syu docker docker-compose
sudo systemctl start docker
sudo systemctl enable docker
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
pamac install virtualbox virtualbox-guest-iso
```

> During install, you'll be asked to choose the modules for your kernel version, you can check it with `uname -r`. In the case you are not asked to choose the modules and get an error when starting the virtual machine, try installing the modules manually with `sudo pacman -S linux419-virtualbox-host-modules` (linux419 = linux + kernel version 4.19), followed by `sudo /sbin/rcvboxdrv setup`.

Then add your user to the VirtualBox group

```
sudo gpasswd -a $USER vboxusers
```

Download the [VM](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/). IE11/Win7 is tested and less prone to activation issues.

Unzip the file, and import it with VirtualBox `File > Import Appliance`. Configure the VM as desired (RAM, enable USB2.0, etc). Start it, install Guest Additions (`Devices - Insert Guest Additions CD image`), install the software you need, and create a snapshot.

The license is valid for 90 days, and can be rearmed about 5 times. But having a snapshot around is a good idea, to have a starting point in case the activation fails. Nevertheless, even without being activated, the OS should work and be enough for your needs.

> If you want to store your VM in a different partition, it's a good idea to configure VirtualBox from start to save to that partition, in `File > General > Default Machine Folder`. It will make it easier to re-import the VM if you change your system.

## SSH

You can store a file `~/.ssh/config` with multiple SSH configs, so that you don't need to remember addresses and users, just do `ssh awesome-remote-machine`.

You can find a file with some ASP remotes already added in our [repository](http://gitlab.alter.lan/asp/wikiss/annexes/blob/master/ssh/config)

## Nice to haves

### Enable AUR

Enable AUR in `Package Manager > Preferences > AUR > Enable > Check for updates`

> Learn more about AUR [here](https://wiki.manjaro.org/index.php?title=Arch_User_Repository)

### Backups

Install Timeshift

```
sudo pacman -Syu timeshift
```

and enable cron

```
sudo systemctl start cronie
sudo systemctl enable cronie
```
> Typically choose the defaults, set the periodicity and number of snapshots to store

### Disks

You can use `gnome-disks` to auto-mount your `data` partition automatically at boot

### Developer tools

#### Chromium

An OSS-version of Google Chrome, available on ABS

```
pamac install chromium
```

Or, if you need Google Chrome's proprietary stuff (like Widevine for Netflix)

```
pamac build google-chrome
```

#### VS Code

Available on AUR

```
pamac build visual-studio-code-bin
```

#### PHP Storm

Available on AUR

```
pamac build phpstorm
```

#### MySQL Workbench

Available on ABS

```
pamac install mysql-workbench
```

#### GitKraken

Available on AUR

```
pamac build gitkraken
```

### Fonts

If you don't like the default fonts, install the arguably best Linux fonts

```
pamac install noto-fonts noto-fonts-emoji noto-fonts-extra
```

Install Fira Code, a nice ligature compatible font to use with your IDE

```
sudo pacman -Syu ttf-fira-code
```

### DisplayLink

DisplayLink does not provide official Arch-based distros support, but the driver is open-source and it's available in AUR.

First, check your kernel version

```
uname -r
```

And then install the kernel headers (4.19 in this example)

```
sudo pacman -Syu linux419-headers
```

Build the DisplayLink from AUR

> Choose `evdi-git` when prompted

```
pamac build displaylink
```

Create/edit `/usr/share/X11/xorg.conf.d/20-evdidevice.conf` and add these lines

```
Section "OutputClass"
	Identifier "DisplayLink"
	MatchDriver "evdi"
	Driver "modesetting"
	Option  "AccelMethod" "none"
EndSection
```

Create/edit `/usr/share/X11/xorg.conf.d/20-displaylink.conf` and add these lines

```
Section "Device"
  Identifier "DisplayLink"
  Driver "modesetting"
  Option "PageFlip" "false"
EndSection 
```

Enable the service

```
sudo systemctl enable displaylink
```

and reboot the system. Check if the service started with no errors

```
sudo systemctl status displaylink
```

You can now setup your displays with the inbuilt tool, or with `arandr`

```
sudo pacman -Syu arandr
```

> You may encounter trailing/flickering mouse cursor issues, still no definitive solution, but there's a workaround, `sudo systemctl restart lightdm` after login.

### Performance

In modern machines with SSDs and 8GB of RAM or more, some tweaks can be made, especially for machines that will run with high memory loads. This guide assumes you have some swap (file or partition) enabled.

First, tweak your `swappiness` and `vfs_cache_pressure` values. Edit/touch this file

```
sudo nano /etc/sysctl.d/99-sysctl.conf
```

and add the lines.

```
vm.swappiness = 10
vm.vfs_cache_pressure = 50
```

Then enable [zswap](https://wiki.archlinux.org/index.php/Zswap). Install it first with

```
pamac install systemd-swap
```

and enable it with

```
sudo systemctl enable systemd-swap.service
```

Finally, reboot your system.

### Disable system bell

If you find the system bell annoying, do this

```
sudo nano /etc/modprobe.d/nobeep.conf
```

and add this line

```
blacklist pcspkr
```

If you don't want to reboot for the changes to make effect, run

```
sudo rmmod pcspkr
```

> More info [here](https://wiki.archlinux.org/index.php/PC_speaker#Globally)

### Multimedia

Typically the built-in media players work well.

If you have a Spotify account, it's available on AUR

```
pamac build spotify
```

### Screenshots

A nice tool for screenshots is Flameshot

```
pamac install flameshot
```

Then, configure Flameshot as your preferred screenshot app, in `Settings > Keyboard > Shortcuts > Custom Shortcuts`, and add a custom shortcut called Flameshot with the command `flameshot gui`. Assign it to the `PrintScr` key. Then, when using the Flameshot app, just follow the on-screen instructions.

A good is example is to press `PrintScr`, select a screen area with the mouse, and press `CTRL+C` to copy it to your clipboard.

### Oh My ZSH

You can install this cool ZSH shell framework, with autocomplete and other nice features. It will improve your terminal interactions

First, install a Manjaro tailored Oh My ZSH package

```
pamac build oh-my-zsh-git
```

Copy the base config file to your home

```
cp /usr/share/oh-my-zsh/zshrc ~/.zshrc
```

Now you need to logout/login, or change the current shell explicitly

```
chsh -s $(which zsh)
```

You edit your config in `~/.zshrc`, and by example, change to different [theme](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)

If you want to apply Oh My ZSH to the root account, run

```
sudo chsh -s $(which zsh) root && sudo ln -s ~/.zshrc /root/.zshrc
```

### MS fonts

If you don't want to miss out on the nice fonts from MS, install

```
pamac build ttf-ms-fonts
```

# Troubleshooting

## System journal

A good way to start troubleshooting is to check your system journal

```
journalctl -xe
```

Google the error codes and you may find a solution

## Free disk space

Clear your package cache with

```
sudo paccache -r
```

> A more "blunt" alternative would be `sudo pacman -Sc`

> If you use an alternative package manager like `yay`, run `yay -Sc`

Remove orphan packages

```
sudo pacman -Rns $(pacman -Qtdq)
```

> If you use an alternative package manager like `yay`, run `yay -Yc`

Clean up your `systemd` logs with

```
sudo journalctl --vacuum-time=1weeks
```

> Or clean up to a size limit with `sudo journalctl --vacuum-size=100M`

---

Copyright 2020 jfrmm.
