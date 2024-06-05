# SoulMask - Wekod Docker Dedicated Server

![Discord](https://img.shields.io/discord/360026109561274368?logo=discord&logoColor=%237289da&label=Discord&link=https%3A%2F%2Fdiscord.gg%2FUVwA74F4pH)

Table of Contents
=============

- [SoulMask - Wekod Docker Dedicated Server](#docker---wekod-docker-dedicated-server)
	- [Table of Contents](#table-of-contents)
	- [Requirements](#requirements)
	- [Docker Compose file](#docker-compose-file)
	    - [Structure](#structure)
	        - [Container Name](#container-name)
	        - [Image](#image)
	        - [Restart](#restart)
	        - [Volumes](#volumes)
	        - [Ports](#ports)
	        - [Environment Variables](#environment-variables)
	- [Environment Variables](#environment-variables)
	- [Getting Started](#getting-started)
	- [Faq](#faq)
    - [Troubleshoot](#troubleshoot)

Requirements
=============

You'll need to have a docker service installed on your machine. Docker need to be able to execute linux containers.

You can install docker on a by refering to these tutorials :

[Install Docker Desktop on Linux](https://docs.docker.com/desktop/install/linux-install)

[Install Docker Desktop on Windows](https://docs.docker.com/desktop/install/windows-install)

[Install Docker Desktop on Mac](https://docs.docker.com/desktop/install/mac-install)

>> I'll extends this section later.

Docker Compose file
=============

```yaml
services:
  SoulMaskServer:
    container_name: SoulMaskServer
    image: wekod/soulmask-server:latest
    restart: unless-stopped
    volumes: 
        - ./data:/SoulMaskServer
    ports:
        - target: 8777
        published: 8777
        protocol: udp
        mode: host
        - target: 27015
        published: 27015
        protocol: udp
        mode: host
        - target: 18888
        published: 18888
        protocol: tcp
        mode: host
    environment:
        - SERVER_HOSTNAME=Wekod Docker Server
        - MAX_PLAYER=20
        - SERVER_PORT=8777 
        - SERVER_QUERYPORT=27015 
        - SERVER_ECHOPORT=18888
        - SERVER_PASSWORD=MySuperServerPassword123456!
        - SERVER_ADMIN_PASSWORD=MySuperServerAdminPassword123456*
        - SERVER_PVP=true
        - SERVER_LOGS=false
        - SERVER_MULTIHOME=0.0.0.0
```

### Structure

#### Container Name

``container_name: SoulMaskServer``

This define the container name 

#### Image 

``image: wekod/soulmask-server:latest``

This will define the image which your container will be based on. Here it's my image which contains all the need to start the soulmask dedicated server.

#### Restart

``restart: unless-stopped``

`restart` defines the policy that the platform applies on container termination.

- `no`: The default restart policy. It does not restart the container under any circumstances.
- `always`: The policy always restarts the container until its removal.
- `on-failure`: The policy restarts the container if the exit code indicates an error.
- `unless-stopped`: The policy restarts the container irrespective of the exit code but stops
  restarting when the service is stopped or removed.

```yaml
    restart: "no"
    restart: always
    restart: on-failure
    restart: unless-stopped
```

#### Volumes

[Docker Compose - Volumes](https://github.com/compose-spec/compose-spec/blob/master/07-volumes.md)

```yaml
 volumes: 
        - ./data:/SoulMaskServer
```

In this case, I am mapping a local folder `data` to the distant (inside the container) folder `/SoulMaskServer`
By this way, you'll access to all files from your server, including the WS/Saved folder which will contains all the data regarding to your world and player.

#### Ports

[Docker Compose - Service Ports](https://github.com/compose-spec/compose-spec/blob/master/05-services.md#ports)

```yaml
ports:
    - target: 8777
    published: 8777
    protocol: udp
    mode: host
    - target: 27015
    published: 27015
    protocol: udp
    mode: host
    - target: 18888
    published: 18888
    protocol: tcp
    mode: host
```

Soulmask server need at least 2 udp port to be opened. From inside the container, port will be used as defined using the environment variable.
By default, we are using the 8777 server port and 27015 query port.

So let's go deep in one target block.

```yaml
    - target: 8777
    published: 8777
    protocol: udp
    mode: host
```
`target`: The container port

> [!WARNING]  
> **If you want to change the target value** don't forget to change the environment variable.

`published`: The publicly exposed port. 

`protocol`: The port protocol (`tcp` or `udp`). Defaults to tcp.

`mode`: host: For publishing a `host` port on each node, or ingress for a port to be load balanced. Defaults to `ingress`.

#### Environment Variables

```yaml
environment:
    - SERVER_HOSTNAME=Wekod Docker Server
    - MAX_PLAYER=20
    - SERVER_PORT=8777 
    - SERVER_QUERYPORT=27015 
    - SERVER_ECHOPORT=18888
    - SERVER_PASSWORD=MySuperServerPassword123456!
    - SERVER_ADMIN_PASSWORD=MySuperServerAdminPassword123456*
    - SERVER_PVP=true
    - SERVER_LOGS=false
    - SERVER_MULTIHOME=0.0.0.0
```

Each line correspond to an environment variable and it's value. no " needed.

See the [Environment Variables](#environment-variables) tab to known all available environment variable available and it's default value when not defined.

Environment variables
=============

| Environment Variable	| Default | Value |
| ---------------------	| --------| ----- |
| MAX_PLAYER			| 20 | The maximum amount of player allowed on your server |
| SERVER_HOSTNAME		| Wekod SoulMask Docker Dedicated Server | The name of your server. It will be displayed in the server list (no " needed)   |
| SERVER_PORT			| 8777 | The port to join your server, /!\ need to be open to public on your server. (udp)                                 |
| SERVER_QUERYPORT		| 27015                                    | The port to use to contact a master list (the server list) /!\ need to be open to public on your server. (udp)   |
| SERVER_ECHOPORT		| 18888 | EchoPort (not needed to be open to public) (tcp) |
| SERVER_PASSWORD       | null | The password people have to enter to join your server. |
| SERVER_ADMIN_PASSWORD | null | The admin password you'll have to type to enter in admin mode |
| SERVER_PVP | false | Set the PVP mode of your server `true` = PVP / `false` = PVE |
| SERVER_BACKUP | game default value | Specifies the interval for writing the game database to disk (unit: seconds). |
| SERVER_SAVING | game default value | Specifies the interval for writing game objects to the database (unit: seconds). |
| SERVER_INITBACKUP | null | Backs up game saves (world.db) when the game starts. |
| SERVER_BACKUP_INTERVAL | null | Specifies how often (minutes) to automatically back up the world save (world.db). This will create a copy of the world.db every interval you defined. Location of these files are next to the world.db file (Ex: auto_0_20240605134600.db) |
| SERVER_LOGS | false | Enable or not the log mode on your server `true` / `false` |
| SERVER_MULTIHOME | game default value | Specifies the local listening address. |

> [!TIP]
> `SERVER_SAVING` is used to save the world in memory \
> `SERVER_BACKUP` is used to write that memory into the world.db \
> `SERVER_INITBACKUP` is used to make backup of the world.db file at server start \
> `SERVER_BACKUP_INTERVAL` is used to set the interval of the world.db file saving into backup file

> [!NOTE]  
> All environment parameter are optional. It should works even when you provide none. \
> That being said, if you do not assign the SERVER_HOSTNAME environment variable, your serveur will have the name `Wekod SoulMask Docker Dedicated Server` \
> SERVER_BACKUP_INTERVAL is used to backup your world.db, so it's optional, but recommanded to use if you don't want a loss of data in case of corrupt save |

Getting Started
=============

1. Create a folder for your game (Ex : /home/ubuntu/Games/SoulMaskServer)

> [!WARNING]  
> Be sure to create a folder with write user permission. 
> The docker container has a link between this folder and it's own (if you have setted up the volume link).
> The container will want to download and install all file into this folder.

2. Place the file docker-compose.yaml inside it.
3. Modify the docker-compose.yaml to be convenient with your needs. You can let it as is, it works fine.
4. Execute the command in a shell based at the folder location containing the docker compose file => `docker compose up -d` (-d is the option to detach directly from the container)

> [!WARNING]  
> if you didn't add the -d option, then your server is started, but you're attached to it. You'll have to detached from it, which might stop it.
> So you'll have to restart it using this command : `docker start SoulMaskServer` where `SoulMaskServer` is the container name define in the `container_name` property of the `docker-compose.yaml` file
  
6. Now your server should be downloaded then started. 
    - The download sequence start once you see in the log this line : `>>> Doing a fresh install of the gameserver...` It may takes few minutes on the first start to download all server files.
    - The launch sequence start once you see in the log this line : `>>> Starting the gameserver`, this can takes few minutes too depending on your machine performances.

> I'll details this section later, but for now, it's good enough to make you start. If you have questions, join the discord and ask me.

FAQ
=============

If you have any question not answer here, join our discord (https://discord.gg/UVwA74F4pH) and ask me.

### Why there is nothing else than a docker-compose.yaml

That's normal, yet, I did not finished to implement all things I had in mind. That will come later.
You'll have the DockerFile and all script file.

If you really want to get them now, find them in the folder /Scripts/.sh inside your docker container.

### Will it have other things to be able to be customized soon ?

Yes, I'll implement quickly many things like the possibility to Backup, Restart, Notification, etc.

### Why the english looks bad ?

Cause I'm bad at english. Probably cause I'm french too... Who knows...

Troubleshoot
=============

#### I'm getting this SteamCMD error `ERROR! Failed to install app '3017300' (Missing file permissions)`

That's probably because the main folder you created on your machine which contains the docker-compose.yaml and the map volume folder has no write permission.
Allow process to write in it and it should be fine.

Credits
=============

Thanks to jammsen (https://github.com/jammsen) for the inspiration.