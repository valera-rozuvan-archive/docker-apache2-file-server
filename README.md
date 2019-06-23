# docker-apache2-file-server

Run an Apache2 static file server via Docker. Key features:

- specify port at runtime
- map random virtual path to host folder

## Build

```
sudo docker build -t file-server .
```

## Run

```
sudo docker run \
  --name file-server-7154 \
  -v /absolute/host/path/to/data/folder:/var/www/html/RANDOM_STRING_FT67VGF667GH \
  -e APACHE_LISTEN_PORT='7154' \
  -p 7154:7154 \
  --expose 7154 \
  --memory="400m" \
  --memory-swap="1g" \
  -d \
  file-server
```

You can now go to http://localhost:7154/RANDOM_STRING_FT67VGF667GH and see your files. If you can open port 7154 on your host system to the world, and your host has an external IP, you will be able to also access files via your IP.

## Check container stats

```
sudo docker ps -q | xargs sudo docker stats --no-stream
```

## Stop container

```
sudo docker stop file-server-7154
sudo docker rm file-server-7154
```

## cgroup config

If you get some warning about `--memory-swap` flag, then you need to update `/etc/default/grub` and set:

```
GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
```

For this to propagate to the kernel, run `update-grub && reboot`.

## Enjoy ;)
