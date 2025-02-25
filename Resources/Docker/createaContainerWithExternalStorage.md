# Create a container with external storage

Create a volume for the Docker container to use which resides on a separate disk.

This this example I’ll be using Linuxserver:Transmission

Make the directory to be used:

```jsx
sudo mkdir -p /media/usbdrive/dockerPStorage/transmission
```

```bash
sudo docker volume create \
--name transmission \
--opt type=block \
--opt device=/media/usbdrive/dockerPStorage/transmission 
--opt o=bind
```

Create the Linuxserver/transmission container:

```bash
sudo docker run -d \
-v transmission:/config \
-v transmission:/downloads \
-v transmission:/watch \
--name=transmission \
-e TZ=Europe/London \
-e USER=jDoe \
-e PASS=:LamePassword! \
-p 9091:9091 \
lscr.io/linuxserver/transmission:latest
```

Once finished with the container, the below will stop and delete the container and volume.

```bash
sudo docker stop transmission && sudo docker rm transmission && sudo docker volume rm transmission
```
