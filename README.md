# Icarus dedicated game server
# Warning from Version 10 on, some changes where made to USER & Paths  
This dedicated server will automatically download/update to the latest available server version when started. The dedicated server runs in Ubuntu 22.04 and wine.  
Can also used for pterodactyl (still in DEV STATE!)  

[Docker Hub](https://hub.docker.com/r/mastermnb/icarus-dedicated-server)  
[Repo](https://github.com/MasterMNB/icarus-dedicated-server)  
[Based on Repo](https://gitlab.com/fred-beauch/icarus-dedicated-server)  


### Pterodactyl config
[Help here](https://pterodactyl.io/community/config/eggs/creating_a_custom_egg.html)  

### Environment Vars
- SERVERNAME : The name of the server on the server browser (You must specify this, the SessionName in the ServerSettings.ini file is always ignored)
- SERVER_PORT : The game port (not specifying it will default to 17777)
- QUERY_PORT : The query port (not specifying it will default to 27015)
- ASYNC_TIMEOUT: Sets the Async timeout to this value in the Engine.ini on server start (not specifying it will default to 60)
- BRANCH: Version branch (public or experimental, not specifying it will default to public)
- All the other, see in Docker Compose Example and the Config section (all are optional)  

### Ports
The server requires 2 UDP Ports, the game port (Default 17777) and the query port (Default 27015)
They can be changed by specifying the PORT and QUERYPORT env vars respectively.

### Volumes
- The server binaries are stored at /home/container/game/icarus
- The server saves are stored at /home/container/icarus/drive_c/icarus

## IF you want to start it without Pterodactyl

### Example Docker Run
```
docker run -p 17777:17777/udp -p 27015:27015/udp -v data:/home/container/icarus/drive_c/icarus -v game:/home/container/game/icarus -e SERVERNAME=AmazingServer mastermnb/icarus-dedicated-server:latest
```
### Example Docker Compose
```
version: "3.8"

services:
 
  icarus:
    container_name: icarus-dedicated
    image: mastermnb/icarus-dedicated-server:latest
    hostname: icarus-dedicated
    init: true
    restart: "unless-stopped"
    ports:
      - 17777:17777/udp
      - 27015:27015/udp
    volumes:
      - data:/home/container/icarus/drive_c/icarus
      - game:/home/container/game/icarus
    environment:
      - ASYNC_TIMEOUT=60
      - BRANCH=public
      - SERVERNAME=AmazingServer
      - SERVER_PORT=17777
      - QUERY_PORT=27015
      - JOIN_PASSWORD=password
      - MAX_PLAYERS=5
      - ADMIN_PASSWORD=AdminPassword123
      #- RESUME_PROSPECT=
      #- LAST_PROSPECT_NAME=
      #- LOAD_PROSPECT=
      #- CREATE_PROSPECT=
      #- SHUTDOWN_NOT_JOINED_FOR=
      #- SHUTDOWN_EMPTY_FOR=
      #- NON_ADMINS_LAUNCH=
      #- NON_ADMINS_ABORT=
      
volumes:
  data: {}
  game: {}
 
```

### Config  
Under the data volume in the file (gets overwritten from the environment variables) are the settings stored:  
Saved\Config\WindowsServer\ServerSettings.ini  
To change launch parameters you can set your own environment variable "STARTUP", look into the Dockerfile for the default value.  
More Infos how to configure:  
https://github.com/RocketWerkz/IcarusDedicatedServer/wiki/Server-Config-&-Launch-Parameters  


## License
MIT License