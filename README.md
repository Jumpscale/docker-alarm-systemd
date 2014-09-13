# Systemd inside ArchLinux

A base Dockerfile with which it is easy to build more specialized systemd-enabled containers. 

Prebuild images are available at docker-hub

## Example

Lets build an image for running a nginx webserver: 

Save the following lines in a file called `Dockerfile` and run `docker build -t nginx-test .`.

````
FROM macedigital/docker-arch-systemd
RUN pacman -S nginx --noconfirm; systemctl enable nginx.service
EXPOSE 80
CMD ["/usr/sbin/init"]
````

After its done, you can run the following command: 

`docker run --privileged=true -p 127.0.0.1:8887:80 -v /sys/fs/cgroup:/sys/fs/cgroup:ro nginx-test`

Voila, go to localhost:8888 on your host machine and rejoice.

## Notes

* You cannot run any such build containers if your host OS does not support systemd (sorry, Ubuntu). 

* At the moment of writing, all containers must be run in privileged mode. Otherwise you will encounter this message `Failed to mount tmpfs at /run: Operation not permitted` on start.

* Its necessary to always mount the cgroups filesystem into the container using the `-v /sys/fs/cgroup:/sys/fs/cgroup:ro` parameter.

## Issues

The base Dockerfile should work quite well for any more special purpose build. Nonetheless, feel free to use the issue tracker if you feel something is not right. Also, pull requests are welcomed.

