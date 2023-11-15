# quarkus-podman-in-podman-db-devserv-fail-example

This is just an fugly issue project added as an companion to reporting a Quarkus issue.

It shows that podmaninpodman fails with devservices enabled 
i.e Quarkus has some issues with podman. 

Steps to reproduce (used podman 4.6.2 as that was the latest availabe on Ubuntu)


1. Verify your can run this project with podman locally first,  ie no docker installed and should handle a mvn clean test 

    https://quarkus.io/guides/podman


2. Build the companion podman:stable images, with added jdk and maven for ease.

    podman build -f DOCKERFILE.podman.jdk -t podman-jdk-node:v4.7.2 .

3. now time for podman in podman, mount this project repo. 

    podman run --rm -it --net=host --user root -v $(pwd):/home/podman/app localhost/podman-jdk-node:v4.7.2 /bin/bash

From now on everything is in the container

4. make target folder readable for all 

5. change user to podman (su podman)


5. mvn verify (fails)

6. setup podman so that socket exists, export DOCKER_HOST etc https://quarkus.io/guides/podman

7. mvn verify (fails), because something stills seems to be hardcoded to var/run/docker.sock so add a softlinlk

8. ln -s <PODMAN SOCKET FROM INFO ABOVE> var/run/docker.sock

8. mvn verify (starts up, but FAILS due to unknow issues in quarkus devservice?)


Note: doing the same producer but doing a podman run mypostgresimage and giving url to that will work (skipping devservices)

Note: using docker for the same usecase (dind rootless) will work fine.


