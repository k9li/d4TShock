# dockerTShock
Dockerfile for TShock
## Quick start guide

First you need a linux machine with [Docker][Docker] installed. Everything from here on out assumes the docker service is running _(you may need to start the service after install)_.

### Create directory to save your world to

Next create a directory for your world file, configuration, and logs

```bash
mkdir -p data/Worlds
```

### Creating a fresh world

For the first run you will need to generate a new world with a size where: _1=Small, 2=Medium, 3=Large_

```bash
sudo docker run -it -p 7777:7777 --rm -v data:/data ky9i/t_shock -world /data/Worlds/<world_name_here>.wld -autocreate <world_size_number_here>
```

**Note:** If you close the the terminal, the server will stop running.  You will need to restart with a preexisting world. It may
be worth while to close after creation anyway to update the initial `config.json` settings.

To create a world with a few more initial options, you can do so in an interactive mode.

```bash
sudo docker run -it -p 7777:7777 --rm -v data:/data ky9i/t_shock
```

### To start with a preexisting world

```bash
sudo docker run -d --rm -p 7777:7777 -v data:/data --name="TShock" -e WORLD_FILENAME=<.wld world_filename_here> ky9i/t_shock
```

**Note:** This command is designed to run in the background, and it is safe to close the terminal window.

Any `config.json` in the directory will automatically be loaded.  The `<world_file_name>.wld` should be the name of your wld file in your data/Worlds directory.

## Updating your container

Updating is easy!

1. Grab the latest terraria container

    ```bash
    docker pull ky9i/t_shock
    ```

2. First we need to find our running container to stop, so we can later restart with the latest

    ```bash
    docker container ls | grep ky9i/t_shock
    ```

    The first few numbers and letters, on a line, are the container hash.  Remember the first 3 or so letters or numbers

    Example:

    ```bash
    1234567890ab        ky9i/t_shock:latest   "/bin/sh bootstrap.s…"   3 minutes ago       Up 3 minutes        0.0.0.0:7777->7777/tcp, 7878/tcp   reverent_solomon
    ```

    `f25` would be the first few letters/numbers of the container hash

    **NOTE:** If you see multiple lines, find the one that still has an `up` status.

3. Stop and remove the container

    ```bash
    docker container rm -f xxx # xxx is the letters/numbers from the last step
    ```

4. Start your container again with your world _(see the [Quick start](#Quick-start-guide))_
