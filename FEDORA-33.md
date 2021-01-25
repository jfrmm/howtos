[< Back](./README.md)

How to basic setup Fedora 33.

## RPM Fusion

Enable [RPM Fusion repository](https://rpmfusion.org/Configuration), so that you have access to **nonfree** repositories

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

## Docker

Install Docker, and enable it

```
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io
sudo systemctl enable docker.service
```

Install Docker Compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Add your user to the `docker` group, and restart

```
sudo usermod -aG docker <your-user>
```

## PHP

To have multiple PHP versions available on the system, we'll need [Remi's repository](https://rpms.remirepo.net/wizard/). In this example, we'll install PHP 5.6, run

```
sudo dnf install https://rpms.remirepo.net/fedora/remi-release-32.rpm
sudo dnf --enablerepo=remi install php56
```

If you need to add CURL for this specific PHP version, run

```
sudo dnf install php56-php-curl
```

Check the installed PHP version with `php56 -v`

## Nice to haves

### Fonts

Fedora's font rendering is not the best, but can be improved

Enable better_fonts COPR repository:

```
sudo dnf copr enable dawid/better_fonts
```

Install packages

```
sudo dnf install fontconfig-enhanced-defaults fontconfig-font-replacements
```

Log out and log in

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
