[< Back](./README.md)

# Manjaro 18.0.4

## Nice to haves

Install Timeshift

Enable cron

```
systemctl start cronie
systemctl enable cronie
```

Install VS Code

Install Fira Code fonts

## Git

To avoid "ghost changes" when dual-booting Windows/Linux, boot to Linux and

```
git config --global core.autocrlf input
```

Then, boot to Windows and

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

Copyright 2019 jfrmm.
