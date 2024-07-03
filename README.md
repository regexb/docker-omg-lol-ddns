# Docker OMG.lol DDNS

This small Alpine Linux based Docker image will allow you to use the [OMG.lol DNS API](https://home.omg.lol/info/dns) as a Dynamic DNS Provider ([DDNS](https://en.wikipedia.org/wiki/Dynamic_DNS)).

This is a multi-arch image and will run on amd64, aarch64, and armhf devices, including the Raspberry Pi.

## Usage

Quick Setup:

```shell
docker run \
  -e API_KEY=xxxxxxx \
  -e ADDRESS=omg.lol.address \
  -e SUBDOMAIN=subdomain \
  regexb/omg-lol-ddns
```

## Parameters

* `--restart=always` - ensure the container restarts automatically after host reboot.
* `-e API_KEY` - Your OMG.lol API token. **Required**
  * `API_KEY_FILE` - Path to load your OMG.lol API token from (e.g. a Docker secret). *If both `API_KEY_FILE` and `API_KEY` are specified, `API_KEY_FILE` takes precedence.*
* `-e ADDRESS` - The OMG.lol address that DDNS updates should be applied to. **Required**
  * `ADDRESS_FILE` - Path to load your OMG.lol DNS Zone from (e.g. a Docker secret). *If both `ZONE_FILE` and `ZONE` are specified, `ZONE_FILE` takes precedence.*
* `-e SUBDOMAIN` - A subdomain of the `ADDRESS` to write DNS changes to. If this is not supplied the root zone will be used.
  * `SUBDOMAIN_FILE` - Path to load your OMG.lol DNS Subdomain from (e.g. a Docker secret). *If both `SUBDOMAIN_FILE` and `SUBDOMAIN` are specified, `SUBDOMAIN_FILE` takes precedence.*

## Optional Parameters

* `-e RRTYPE=A` - Set to `AAAA` to use set IPv6 records instead of IPv4 records. Defaults to `A` for IPv4 records.
* `-e DELETE_ON_STOP` - Set to `true` to have the dns record deleted when the container is stopped. Defaults to `false`.
* `-e INTERFACE=tun0` - Set to `tun0` to have the IP pulled from a network interface named `tun0`. If this is not supplied the public IP will be used instead. Requires `--network host` run argument.
* `-e CUSTOM_LOOKUP_CMD="echo '1.1.1.1'"` - Set to any shell command to run them and have the IP pulled from the standard output. Leave unset to use default IP address detection methods.
* `-e DNS_SERVER=10.0.0.2` - Set to the IP address of the DNS server you would like to use. Defaults to 1.1.1.1 otherwise. 
* `-e CRON="@daily"` - Set your own custom CRON value before the exec portion. Defaults to every 5 minutes - `*/5 * * * *`.

## IPv6

If you're wanting to set IPv6 records set the envrionment variable `RRTYPE=AAAA`. You will also need to run docker with IPv6 support, or run the container with host networking enabled.

## Docker Compose

If you prefer to use [Docker Compose](https://docs.docker.com/compose/):

```yml
version: '1'
services:
  omg-lol-ddns:
    image: regexb/omg-lol-ddns:latest
    restart: always
    environment:
      - API_KEY=xxxxxxx
      - ADDRESS=omg.lol.address
      - SUBDOMAIN=subdomain
```
