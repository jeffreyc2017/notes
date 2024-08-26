# minecraft

# setup

- AWS

  security group
  inbound rules for 25565 for both TCP and UDP

- minecraft
  [server setup](https://minecraft.wiki/w/Tutorials#Server_setup)
  [server startup script](https://minecraft.wiki/w/Tutorials/Server_startup_script)
  [download](https://www.minecraft.net/en-us/download/server)

```sh
sudo apt update
sudo apt install openjdk-8-jre-headless curl screen nano bash grep
sudo apt install openjdk-21-jdk-headless

cd /opt
sudo mkdir minecraft

sudo groupadd minecraft
sudo useradd --system --shell /usr/sbin/nologin --home /opt/minecraft -g minecraft minecraft
```

start.sh

```text
#!/bin/sh
cd "$(dirname "$0")"
exec java -Xms1G -Xmx1G -jar server.jar --nogui
```

```sh
chmod a+x start.sh
```
