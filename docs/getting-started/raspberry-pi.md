# Running PhotoPrism on a Raspberry Pi

Download our [docker-compose.yml](https://dl.photoprism.org/docker/arm64/docker-compose.yml) file
(right click and *Save Link As...* or use `wget`) to a folder of your choice, 
change the [configuration](config-options.md) as needed, and run `docker-compose up` to start PhotoPrism:

```
wget https://dl.photoprism.org/docker/arm64/docker-compose.yml
sudo docker-compose up -d
```

!!! attention "Change Password"
    Please change `PHOTOPRISM_ADMIN_PASSWORD` so that PhotoPrism starts with a secure **initial password**.
    Never use `photoprism`, or other easy-to-guess passwords, on a public server.
    A minimum length of 4 characters is required.

See [Setup Using Docker Compose](docker-compose.md) and [Config Options](config-options.md) for details.

All commands may have to be prefixed with `sudo` when not running as root.
Note that this will change the home directory `~` to `/root` in your configuration.

Our repository on Docker Hub: [`photoprism/photoprism-arm64`](https://hub.docker.com/r/photoprism/photoprism-arm64)

### System Requirements ###

You need to [boot](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)
your Raspberry Pi 3 / 4 with the parameter `arm_64bit=1` in `config.txt` in order to use this image.
Alternatively, you may run the image on [UbuntuDockerPi](https://github.com/guysoft/UbuntuDockerPi).
It's a 64bit Ubuntu Server with Docker pre-installed.

Indexing large photo and video collections significantly benefits from fast, local SSD storage 
and enough memory for caching.

To avoid permission issues, docker-compose.yml should include the following security options:

```yaml
  photoprism:
    security_opt:
      - seccomp:unconfined
      - apparmor:unconfined
```

Big thank you to [Guy Sheffer](https://github.com/guysoft) for
[building](https://github.com/photoprism/photoprism/issues/109) this!

!!! tip "Reducing Server Load"
    If you're running out of memory - or other system resources - while indexing, please limit the
    [number of workers](https://docs.photoprism.org/getting-started/config-options/) by setting
    `PHOTOPRISM_WORKERS` to a value less than the number of logical CPU cores in `docker-compose.yml`.
    Also make sure your server has [swap](https://opensource.com/article/18/9/swap-space-linux-systems)
    configured so that indexing doesn't cause restarts when there are memory usage spikes.
    As a measure of last resort, you may additionally disable image classification using TensorFlow.
