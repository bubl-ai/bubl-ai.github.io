---
title: Raspberry Pi Setup
date: 2024-02-16 12:00:00 -0
categories: [Raspberry Pi, Setup]
---

# Let's Kick Off the Raspberry Pi Adventure!

## Why Start from Scratch?
Starting from scratch is like starting a video game on easy mode – it's the best way to learn, have some fun, and maybe break a few things along the way - because let's be real, that's where the real learning happens!

I recently got my hands on the shiny new RPi 5 and thought, "Why not turn this setup into a fun experience to share with all of you?" So here we are, diving into the setup needed to kickstart your AI projects on the Raspberry Pi.

## What you will learn here:

1. Connect to your RPi using SSH or VNC
2. Install Docker
3. Install VSCode and relevant extensions
4. Connect to GitHub with SSH


## 1. Connect to your RPi using SSH or VNC

For a more detailed explanation you can use [this link](https://www.raspberrypi.com/documentation/computers/remote-access.html).

### Make Your RPi Discoverable
+ Head to RPi Configuration > Interfaces
+ Turn on the switches for ssh and vnc


### 1.1. Setup RealVNC in RPi
If your RPi is headless (doesn't have a physical montitor), this will allow you to connect from another device.[^fn-nth-2], [^fn-nth-3]
```bash
sudo apt install realvnc-vnc-server
```

Once intalled just expose an IP address that you will be using to connect to it.
```bash
vncserver-virtual
```

### 1.2. Setup your other device used to connect to RPi
I am using an iPad for this, but the next steps shouldn't be very different for other devices.

+ You can use IP Scanner app to scan your local network devices. If you are connected to the same Network as your RPi, the IP number appears there.
+ You can SSH into your RPi using Termius. This is done easily by creating a new connection and using the IP you got on the previous point as the Hostname.
+ If you want remote acces to your desktop you have to make sure to first run `vncserver-virtual` on your RPI and this will give you an IP number. Use this to set a new connection within the RVNC Viewer in you iPad.


## 2. Install docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker [user_name]
```
After executing this, make sure to reboot your RPi to make sure the installation is complete. You can test docker by executing the next commands.[^fn-nth-4]

```bash
docker version
docker info
docker run hello-world
```

## 3. Install VSCode and relevant extensions
```bash
sudo apt install code
```

Relevant Extensions to install:
+ Python, Pylance, and Python Debugger
+ Docker
+ Black Formatter
  - Set `Editor: Default Formatter' as 'Black Formatter'.
  - Go to `Black-formatter: Args` and set `--line-length=79`.
  - Set `editor.formatOnSave` to `true`.
+ autoDocstring
  - You can choose the docstring format of your preference.
+ GitLens
+ Dev Containers
  - Include all the previous extensions by identifier within `Dev Containers: Default Extensions` so all your extensions will always be installed.
  - Update the dev.containers.defaultFeatures and include inside it all the settings you want to propagate to your containers. (e.g. all the black Formatter and autoDocstring settings)

## 4. Connect to GitHub with SSH
+ Generate new ssh key, `ssh-keygen -t ed25519 -C "[your_email@domain.com]"`
+ Start the agent in the background, `eval "$(ssh-agent -s)"`
+ Add your key to the agent, `ssh-add ~/.ssh/id_ed25519`
+ Copy your key by printing it in the terminal, `cat ~/.ssh/id_ed25519.pub`
+ Sign in to your GitHub account and go to Settings > Access > SSH and GPG > New SSH key. Paste the content of your key there.
+ Test your connection, `ssh -T git@github.com`
+ Now you can clone your repo, `git clone git@github.com:[username]/[repo_name][^fn-nth-5]

## References
[^fn-nth-2]: https://stackoverflow.com/questions/66875411/
[^fn-nth-3]: https://help.realvnc.com/hc/en-us/articles/360002249917-VNC-Connect-and-Raspberry-Pi#transferring-files-to-and-from-your-raspberry-pi-0-6
[^fn-nth-4]: https://www.simplilearn.com/tutorials/docker-tutorial/raspberry-pi-docker
[^fn-nth-5]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent