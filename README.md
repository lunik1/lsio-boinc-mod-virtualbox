# Virtualbox - Docker mod for BOINC

This mod adds VirtualBox to BOINC, installed/updated during container start.

In openssh-server docker arguments, set an environment variable DOCKER_MODS=linuxserver/mods:boinc-virtualbox.

If adding multiple mods, enter them in an array separated by |, such as DOCKER_MODS=linuxserver/mods:boinc-virtualbox|linuxserver/mods:boinc-mod2

## Additional setup

To use VirtualBox within the container, the `virtualbox-dkms` (or equivalent) package must be installed on the host and the container must be given access to the `/dev/vboxdrv` device.

CLI example:

``` bash
docker run -d \
  --name=boinc \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -e GUAC_USER=abc `#optional` \
  -e GUAC_PASS=900150983cd24fb0d6963f7d28e17f72 `#optional` \
  -p 8080:8080 \
  -v /path/to/data:/config \
  --device /dev/vboxdrv:/dev/vboxdrv `#required for VirtualBox` \
  --restart unless-stopped \
  ghcr.io/linuxserver/boinc
```

docker-compose example:

``` yaml
---
version: "2.1"
services:
  boinc:
    image: ghcr.io/linuxserver/boinc
    container_name: boinc
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - GUAC_USER=abc #optional
      - GUAC_PASS=900150983cd24fb0d6963f7d28e17f72 #optional
    volumes:
      - /path/to/data:/config
    ports:
      - 8080:8080
    devices:
      - /dev/vboxdrv:/dev/vboxdrv #required for VirtualBox
    restart: unless-stopped
```
