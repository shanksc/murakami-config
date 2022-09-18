# Lanner Setup

## OS installation

Precondition: Lanner is turned off and connected via the management port to a machine with telnet/PuTTY installed

1. Insert Ubuntu 20.04 installation media into USB drive
2. Open telnet connection using the correct serial COM port and 115200 baud
3. Power on Lanner
4. When greeted with the grub bootloader, press `e` to edit the "Install" options
5. Change the line that begins with `linux` to add the following two flags before the triple hyphen `---`:
    * `console=tty0`
    * `console=ttyS0,115200n8`
6. Press `ctrl+X` to boot into the Ubuntu installer
7. Install the minimified version of the OS -- this will save space
8. Use username `TODO` and password `TODO`

## Docker installation

Precondition: Ubuntu 20.04 is set up and a network cable is plugged into the device

1. Power on and log into the Lanner
2. Install `curl` from apt using `sudo apt update && sudo apt install curl`
3. Use the convenience script to install Docker
```
$ curl -fsSL https://get.docker.com | sudo sh -
```

## Murakami setup

Precondition: Ubuntu 20.04 is set up, network is attached, Docker is installed

1. Pull down the murakami.toml from github.com/Better-Broadband/murakami-config.git. Name it `murakami.toml`.
2. Get the service account keyfile from GCP for the `monitors` service account. Name it `service-account-keyfile.json`.
3. Put those two files in any arbitrary directory, we'll call it `/config`
4. Start the docker container using the following config:
```
sudo docker run -d --network host --volume /config:/murakami/configs/ measurementlab/murakami:latest -c /murakami/configs/murakami.toml
```
5. Check the docker logs to make sure authentication of the first test is successful
```
$ sudo docker ps  # to get the id of the container
$ sudo docker logs -f $id  # using the id from above
# ctrl+c to exit once you've observed the upload
```

Note: files can be transferred using `scp` on Linux or `pscp` on Windows with a syntax like:

```
$ scp path/to/file username@$LANNER_IP:/remote/path/to/destination  # identical on Windows, just with pscp instead of scp
```

This may prompt you to accept a fingerprint if it's the first time you're connecting to the Lanner this way.

## Shutdown

Precondition: Whole system set up, running, and upload observed

1. Disconnect all terminal sessions
2. Press the small power button on the back of the Lanner once to trigger a graceful shutdown
3. Unplug from your workbench and prepare for shipment to customer