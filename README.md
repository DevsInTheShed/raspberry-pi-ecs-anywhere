# raspberry-pi-ecs-anywhere
How to run ECS anywhere on a Raspberry Pi 4


SSH to your rpi adn run
```
sudo apt-get update && sudo apt-get upgrade
```
Donwload the get docker scripts and run it:
```
curl -fsSL https://get.docker.com -o get-docker.sh
chmod +x get-docker.sh
./get-docker.sh
```
Add the local user to the docker group ( presumption that ubuntu is the name of the user logged in )
```
sudo usermod -aG docker ubuntu
```
Confirm docker is running
````
docker version
docker info
docker run hello-world
```

Next head off to:  https://github.com/jasonumiker/ecsanywhere-dind and follow the instructions to setup the ECS Cluster, create a task definition, and register the external instance.


You may run into this issue if the ECS service fails to connect.  I'm not sure why this happened to me, it's possible it's due to a faulty sd-card and I will try again once I upgrade to M2 drive.
```
https://stackoverflow.com/questions/30984569/error-error-creating-aufs-mount-to-when-building-dockerfile
```

Add in /etc/docker/daemon.json

```
{
    "storage-driver": "vfs"
}
```
the restart docker:
```
sudo service docker restart
```

Lastly the Raspberry Pi on the raspberry pi cgroup is disabled by default to enable this:

edit:
```
/boot/firmware/cmdline.txt
```
and add:
```
cgroup_enable=memory
```
To the list.  This will laso require a restart.
