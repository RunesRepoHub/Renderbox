# Renderbox

#### Project Plan: Setting Up Linux Folder Sharing, Automated Video Rendering, and YouTube Video Download Docker Container

## Project 1: Setting Up Linux Folder Sharing

### Objective:

ftp is a network protocol for transferring files between a client and a server. We can use the ftp command-line client to connect to an ftp server and perform file transfers over the internet.

Firstly, before we install the vsftpd package, we need to update our packages:

```
sudo apt-get update
sudo apt-get install vsftpd
```

Next, to allow the ftp server to communicate via the internet, let’s allow the following ports:

```
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
```

Thirdly, to connect to a remote computer, we use the following syntax:

```
ftp [ip address or domain name]
```

Following, we’ll be prompted for a username and password:

```
ftp 192.168.9.103
Connected to localhost.
220 (vsFTPd 3.0.3)
Name (localhost:nairobi): 
331 Please specify the password.
Password: 
ftp>
```

To get files from the remote computer, we use get or mget:

```
ftp> get file.py
local: file.pdf remote: file.py
150 Opening BINARY mode data connection for file.py (180103 bytes).
226 Transfer complete.
180103 bytes received in 0.00 secs (109.0537 MB/s)
ftp>
```

To transfer files to the remote computer, we can use put or mput:

```
ftp> put error.txt
200 PORT command successful
226-File successfully transferred
226 0.849 seconds (measured here), 111.48 Kbytes per second
96936 bytes sent in 0.322 seconds (225 kbytes/s)
ftp>
```

## Project 2: Automated Video Rendering System

### Objective:

- Create an automated rendering system for video editing projects placed in a specified folder using Kdenlive.

### Steps:

```
---
version: "2.1"
services:
  kdenlive:
    image: lscr.io/linuxserver/kdenlive:latest
    container_name: kdenlive
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - SUBFOLDER=/ #optional
    volumes:
      - /home/rune:/config
    ports:
      - 3000:3000
      - 3001:3001
    devices:
      - /dev/dri:/dev/dri #optional
    shm_size: "1gb" #optional
    restart: unless-stopped
```

## Project 3: Docker Container for YouTube Video Downloads

### Objective:

- Create a Docker container for downloading YouTube videos using youtube-dl and save them to the shared folder created in Project 1 on the Linux system.

### Steps:

If you’ve tried to download youtube videos while trying not to download malware, this is for you.

Prerequisites

You will need to have Docker installed, and know if your terminal uses bash or zsh.

All you have to do is use this incantation

```
alias yt-dl='docker run \
                  --rm -i \
                  -e PGID=$(id -g) \
                  -e PUID=$(id -u) \
                  -v "$(pwd)":/workdir:rw \
                  mikenye/youtube-dl'
```
Add this to your ~/.bash_aliases file or ~/.bashrc or ~/.zshrc file.

Once you’ve done that (and used source ~/.zshrc , or whatever the name of your rc file you updated) you can download videos by using yt-dl <youtube-url>

Further reading: https://hub.docker.com/r/mikenye/youtube-dl#quick-start

Disclaimer: this might work for apple and linux devices only. Post a comment if you need help.

Also, we’re using docker here so we don’t run into any problems in terms of yt-download not being compatible with your machine (this is what happened with me).

Example usage:

```
yt-dl YOUTUBELINK
```