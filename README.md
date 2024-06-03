# SoulMask - Wekod Docker Dedicated Server

![Discord](https://img.shields.io/discord/360026109561274368?logo=discord&logoColor=%237289da&label=Discord&link=https%3A%2F%2Fdiscord.gg%2FUVwA74F4pH)

## Table of Contents

- [SoulMask - Docker Dedicated Server](#docker---wekod-docker-dedicated-server)
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

## Requirements

You'll need to have a docker service installed on your machine. Docker need to be able to execute linux containers.

You can install docker on a by refering to these tutorials :

[Install Docker Desktop on Linux](https://docs.docker.com/desktop/install/linux-install)

[Install Docker Desktop on Windows](https://docs.docker.com/desktop/install/windows-install)

[Install Docker Desktop on Mac](https://docs.docker.com/desktop/install/mac-install)

>> I'll extends this section later.

## Docker Compose file
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

    :warning: **If you want to change this value** don't forget to change the environment variable.

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
```

Each line correspond to an environment variable and it's value. no " needed.

See the [Environment Variables](#environment-variables) tab to known all available environment variable available and it's default value when not defined.

## Environment variables

| Environment Variable	| Default | Value |
| ---------------------	| --------| ----- |
| MAX_PLAYER			| 20 | The maximum amount of player allowed on your server |
| SERVER_HOSTNAME		| Wekod SoulMask Docker Dedicated Server | The name of your server. It will be displayed in the server list (no " needed)   |
| SERVER_PORT			| 8777 | The port to join your server, /!\ need to be open to public on your server. (udp)                                 |
| SERVER_QUERYPORT		| 27015                                    | The port to use to contact a master list (the server list) /!\ need to be open to public on your server. (udp)   |
| SERVER_ECHOPORT		| 18888 | EchoPort (not needed to be open to public) (tcp)

## Getting Started

1. Create a folder for your game (Ex : /home/ubuntu/Games/SoulMaskServer)
2. Place the file docker-compose.yaml inside it.
3. Modify property of the docker-compose.yaml to be convenient with your needs.
4. Execute docker compose => `docker compose up -d` (-d is the option to detach directly from the container)
5. if you didn't add the -d option, then your server is started, but you're attached to it. You'll have to detached from it.

>> I'll extends this section later.                                                                 |

## FAQ

If you have any question not answer here, join our discord (https://discord.gg/UVwA74F4pH) and ask me.

### Why this documentation sucks ?
	
That will be better soon :)

### Why there is nothing else than a docker-compose.yaml

That's normal, yet, I did not finished to implement all things I had in mind. That will come later.
You'll have the DockerFile and all script file.

If you really want to get them now, find them in the folder /Scripts/.sh inside your docker container.

### Will it have other things to be able to be customized soon ?

Yes, I'll implement quickly many things like the possibility to choose the mode (PVE or PVP), Backup, Restart, Notification, etc.

### Why the english looks bad ?

Cause I'm bad at english. Probably cause I'm french too... Who knows...

## Credits

Thanks to jammsen (https://github.com/jammsen) for the inspiration.