# How to use the Zimbra Docker

This is the recommended way to run Zimbra Docker.

## Create the project folder structure

```bash
mkdir -p myproject/configs
cd myproject
```

## Create the `compose.yml` file

```bash
cat <<EOT > compose.yml
services:
  mail:
    # Build
    build: build
    image: yeak/zimbra-8815

    # Run
    hostname: mail.zimbra.lab
    volumes:
      - data:/data
      - ./configs:/configs
    environment:
      DEFAULT_PASSWORD: Zimbra2024
      DEFAULT_TIMEZONE: Asia/Kuala_Lumpur
      DEFAULT_ADMIN: sysadmin
      MAX_MEMORY_GB: 8
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "25:25"
      - "465:465"
      - "587:587"
      - "110:110"
      - "143:143"
      - "993:993"
      - "995:995"
      - "7071:7071"
      - "9071:9071"
#    networks:
#      - zimbranet

#networks:
#  zimbranet:
#    name: zimbranet
#    external: true

volumes:
  data:
EOT
```

- Change `hostname` value and register it in your DNS.
- Edit `DEFAULT_ADMIN`, `DEFAULT_PASSWORD`, `DEFAULT_TIMEZONE`
- Set `MAX_MEMORY_GB` value you want Zimbra to consume from your host machine.

**Special container networks**

If you plan to run Zimbra container in dedicated network and routable, you can check out `zimbranet.sh` file. Then enable the networks section with `external: true`.

## Start the container

```bash
docker compose up -d
```

The first time you start the container, it will automatically configure itself using the information you set in the environment variables.

To watch the output of the setup process, type `docker logs -f myproject-mail-1`. Type Ctrl-C to stop watching.

## Stop the container

```bash
docker compose down
```

This will shutdown the container. Your data is safe in the docker volume.

The next time you start up the container, your data will be attached to the newly created container.

## Data backup

The data is saved in the docker volume. See `docker volume ls`

By default the `/var/lib/docker` folder is your data. You should backup it.

## Update the container

Once a while Zimbra will release new updates. We will build again the new image and push to the docker hub. You just need to `pull` the new image, `down` and `up` the container.

```bash
docker compose pull
docker compose down
docker compose up -d
```

## Reset to default

To clean up and restart again all above,

```bash
docker compose down
docker volume ls
docker volume rm myproject_data
docker compose up -d
```

NOTE: Remove volume will destroy all your data.

## How to run in Production?

Zimbra 8.8.15 is meant to run as VM. We make it into container for quick deployment. This container is still expected to run as VM. You should use dedicated VM for this purpose.

Here is a good use case when run as container:

1. Devel: `mkdir devel` and create `compose.yml`. This can be used for testing and further development. Test your changes before try it on production.

2. Staging: `mkdir staging` and create `compose.yml`. Apply what you did in the testing environment here. A staging environment could have other systems relying on your Zimbra. This is a good place to verify everything still work as expected.

3. Production: `mkdir production` and create `compose.yml`. The actual production environment.

You can use 3 VM for each of the environment above. Or you can squeeze all into one VM.

## More helps

1. How to access the container shell prompt?

   `docker exec -it myproject-mail-1 /bin/bash`

2. Is FOSS end of life?

   Yes, this 8.8.15 is end of life on 31 Dec 2023. https://www.zimbra.com/product/product-lifecycle/

3. How can I contribute?

   The source is published at https://github.com/Mailhappen/zimbra-8815
