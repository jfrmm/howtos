[< Back](./README.md)

How to basic setup Fedora 33.

## Git

Install Git and Git Flow

```
sudo dnf copr enable elegos/gitflow
sudo dnf install git gitflow
```

Configure your user information

```
git config --global user.name <YOUR NAME>
git config --global user.email <YOUR@EMAIL.com>
```

## PHP

Just run

```
sudo dnf install php
```

## Nice to haves

### Oh My Zsh

First, install Zsh

```
sudo dnf install zsh
chsh -s $(which zsh)
```

And then install Oh My Zsh

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Backups

Install Timeshift

```
sudo dnf install timeshift
```

Start the app and follow the wizard. It should be simple, select RSYNC method and the target partition where you want to store your backup. Select the periodicity of the backups, and how many backups you want to store.

### VS Code

I recommend to install the RPM version directly from their site, [here](/home/jmarc/Pictures/Screenshot_20201230_205834.png)

### Fonts

Install Fira Code, a nice ligature compatible font

```
sudo dnf install fira-code-fonts
```

---

Copyright 2020 jfrmm.
