---
title: Raspberry Pi Setup
date: 2024-02-9 12:00:00 -0
categories: [Raspberry Pi, Setup]
---

# Let's Kick Off the Raspberry Pi Adventure!

## Why Start from Scratch?
Starting from scratch is like starting a video game on easy mode – it's the best way to learn, have some fun, and maybe break a few things along the way - because let's be real, that's where the real learning happens!

I recently got my hands on the shiny new RPi 5 and thought, "Why not turn this setup into a fun experience to share with all of you?" So here we are, diving into the setup needed to kickstart your AI projects on the Raspberry Pi.

## What's on Our To-Do List:

- Connect to your RPi using SSH or VNC
- Install Docker
- Install VSCode and relevant extensions
- Connect to GitHub with SSH
- Install Jekyll to develop a blog


## Connect to your RPi using SSH or VNC

### Make Your RPi Discoverable[^footnote]
+ Head to RPi Configuration > Interfaces
+ Turn on the switches for ssh and vnc


### Setup RealVNC in RPi
If your RPi is headless (doesn't have a physical montitor), this will allow you to connect from another device.[^fn-nth-2], [^fn-nth-3]
```bash
sudo apt install realvnc-vnc-server
```

Once intalled just expose an IP address that you will be using to connect to it.
```bash
vncserver-virtual
```

### Setup your other device used to connect to RPi
I am using an iPad for this, but the next steps shouldn't be very different for other devices.

+ You can use IP Scanner app to scan your local network devices. If you are connected to the same Network as your RPi, the IP number appears there.
+ You can SSH into your RPi using the Termius. This is done easily by creating a new connection and using the IP you got on the previous point as the Hostname.
+ If you want remote acces to your desktop you have to make sure to first run `vncserver-virtual` on your RPI and this will give you an IP number. Use this to set a new connection within the RVNC Viewer in you iPad.


## Install docker
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

## Install VSCode and relevant extensions
```bash
sudo apt install code
```

Relevant Extensions to install:
+ Docker
+ Dev Containers

## Connect to GitHub with SSH
+ Generate new ssh key, `ssh-keygen -t ed25519 -C "[your_email@domain.com]"`
+ Start the agent in the background, `eval "$(ssh-agent -s)"`
+ Add your key to the agent, `ssh-add ~/.ssh/id_ed25519`
+ Copy your key by printing it in the terminal, `cat ~/.ssh/id_ed25519.pub`
+ Sign in to your github account and go to Settings > Access > SSH and GPG > New SSH key. Paste the content of your key there.
+ Test your connection, `ssh -T git@github.com`
+ Now you can clone your repo, `git clone git@github.com:[username]/[repo_name][^fn-nth-5]

## Install Jekyll to develop a blog
+ Install all requirements, `sudo apt-get install ruby-full build-essential`
+ Add env variables
```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
+ Install jekyll and bundler, `gem install jekyll bundler`
+ Check that you satisfy all requirements  
    * Ruby version 2.5.0 or higher, `ruby -v`
    * RubyGems, `gem -v`
    * GCC and Make, `gcc -v`, `g++ -v`, and `make -v`
+ Install dependencies, go to your website root folder and run, `bundle`.
+ Test your website locally, `bundle exec jekyll s`[^fn-nth-6]

## References
[^footnote]: https://www.raspberrypi.com/documentation/computers/remote-access.html
[^fn-nth-2]: https://stackoverflow.com/questions/66875411/
[^fn-nth-3]: https://help.realvnc.com/hc/en-us/articles/360002249917-VNC-Connect-and-Raspberry-Pi#transferring-files-to-and-from-your-raspberry-pi-0-6
[^fn-nth-4]: https://www.simplilearn.com/tutorials/docker-tutorial/raspberry-pi-docker
[^fn-nth-5]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
[^fn-nth-6]: https://youtu.be/F8iOU1ci19Q?feature=shared
