# Build the image

You can skip this if you do not intend to customize the image. You could just tell us what you have in mind and we see if it could be incorporated in the new image.

Image building blueprint is in `build/Dockerfile`.

1. The base OS is RockyLinux 8. This must follow what Zimbra software is released for.
2. Install Zimbra with software-only option. We intend to do Standalone installation, so all software for standalone will be installed.
3. Containers are slim down OS by design. It does not need cron and syslog. But our Zimbra expects them to work. So we add back the basic `sshd`, `rsyslog`, `crond`, `sysstat` only.

The above are the basic items that made up an image (program). When an image is launched, it becomes a container (running process). The first program that runs is called ENTRYPOINT.

Our `entrypoint.sh` script will carry out a series of logic to do these tasks:

1. If the system is not yet configured, run the Zimbra configuration task.
2. If it is configured, then attach the data files to the newly started container. The container could be from the existing image or the new one. This merging process is how we roll out new updates. This is also the point where we need to test new changes in Devel and Staging to ensure new image works with the existing data.

The final part of the Dockerfile describes where is the data VOLUME and ports (EXPOSE) used.

By defining the `/data` as a volume, we tell container do not destroy it when the container itself is being destroyed.

The EXPOSE define all ports we allow the world to access us, or the services we are providing. The less port exposed, the better.

This complete our Zimbra Dockerfile preparation.

To build the image, run `docker build -t myzmdocker ./build`.

# Run the image

To test your image, run this:

    `docker run -it myzmdocker`

This will begin the configuration of Zimbra using the default values. The `entrypoint.sh` is doing all the work here.

If you want to test the image without auto-configuring:

    `docker run -it myzmdocker /bin/bash`

The container will start and give you a shell prompt.

# Stop and remove the container

When the container exit, it will just stop and do nothing.

You can remove the container using `docker rm [container-id]`. The temporary container data will be removed. However because we set aside `/data` as an independent volume, it will be left untouched.

To see the volume, run this:

    `docker volume ls`

# Run the container using compose file

A more organized way is to use `compose.yml`. Refer to [[USAGE]] for more information.

# How to add customization

In the `configs` folder there is `pre-startup.sh` and `post-startup.sh`. The first one is to run before Zimbra is started. The latter one run after Zimbra is started. The logic is inside `start.sh` script.

Our example show how to replace Zimbra logo with our custom own logo.

More example will be available as we progress to improve our Zimbra Docker.

