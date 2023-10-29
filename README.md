# Renderbox

## Sharing Videos from Laptop

Install and setup filezilla.

```
sudo apt update
```

```
sudo apt install -y filezilla
```

## Video Rendering System

### Docker Compose:

```
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

## Docker Container for YouTube Video Downloads

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