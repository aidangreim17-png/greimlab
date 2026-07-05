## Plex Media Server

Plex was deployed as a Docker container on the Ubuntu server to provide centralized media streaming for the GreimLab environment.

The container uses host networking so Plex can properly advertise itself on the local network and communicate with client devices. Media is stored outside the container in dedicated host directories for movies, TV shows, and music.

### Container Details

| Item | Value |
|---|---|
| Container Name | plex |
| Image | lscr.io/linuxserver/plex:latest |
| Web Port | 32400 |
| Config Path | ~/docker/plex/config |
| Movies Path | ~/media/movies |
| TV Path | ~/media/tv |
| Music Path | ~/media/music |
| Restart Policy | unless-stopped |

### Access

Local access:

http://SERVER-IP:32400/web

Optional external access:

https://plex.greimlab.net

Plex was not placed behind Authelia because Plex clients and smart TV applications may not work correctly with an additional authentication portal. Plex’s built-in account authentication is used instead.