# SoulMask - Wekod Docker Dedicated Server

![Discord](https://img.shields.io/discord/360026109561274368?logo=discord&logoColor=%237289da&label=Discord&link=https%3A%2F%2Fdiscord.gg%2FUVwA74F4pH)

## Table of Contents

- [SoulMask - Docker Dedicated Server](#docker---palworld-dedicated-server)
	- [Table of Contents](#table-of-contents)
	- [Requirements](#requirements)
	- [Getting Started](#getting-started)
	- [Environment Variables](#environment-variables)
	- [Faq](#faq)

## Requirements

You'll need to have a docker account install on your machine. either windows or linux. but docker need to be able to execute linux containers.

>> I'll extends this section later.

## Getting Started

1. Create a folder for your game (Ex : SoulMaskServer)
2. Place the file docker-compose.yaml inside it.
3. Modify property of the docker-compose.yaml to be convenient with your needs.
4. Execute docker compose => `docker compose up -d` (-d is the option to detach directly from the container)
5. if you didn't add the -d option, then your server is started, but you're attached to it. You'll have to detached from it.

>> I'll extends this section later.

## Environment variables

| Environment Variable	| Value					| Default |
| -------				| ------------------	| ------------------ |
| SERVER_HOSTNAME		| The name of your server. It will be displayed in the server list (no " needed) | Wekod SoulMask Docker Dedicated Server |
| MAX_PLAYER			| The maximum amount of player allowed on your server | 20 |
| SERVER_PORT			| The port to join your server, /!\ need to be open to public on your server. (udp) | 8777 |
| SERVER_QUERYPORT		| The port to use to contact a master list (the server list) /!\ need to be open to public on your server. (udp) | 27015 |
| SERVER_ECHOPORT		| EchoPort (not needed to be open to public) (tcp) | 18888 |

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