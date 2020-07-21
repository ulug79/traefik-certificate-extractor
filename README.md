# Traefik Certificate Extractor

Forked from [KurzonDax/traefik-certificate-extractor](https://github.com/KurzonDax/traefik-certificate-extractor)

Tool to extract Let's Encrypt certificates from Traefik's ACME storage file. Can automatically restart containers using the docker API.

Note: This version differs from original in that:
* Changed requirements.txt to use watchdog instead of watchdog3 which doesn't appear to exist any more
* Support for ACME v1 has been removed.  You must be using the acme_v2 url in your resolver(s)
* Added support for multiple resolvers
* An initial dump of the certs will be performed before starting to watch the acme.json file for changes
* _**Not thoroughly tested**_. Specifically, I don't know if SANs work since I don't use them.

## Installation
```shell
git clone https://github.com/snowmb/traefik-certificate-extractor
cd traefik-certificate-extractor
```

## Usage
```shell
usage: extractor.py [-h] [-c CERTIFICATE] [-d DIRECTORY] [-f] [-r] [--dry-run]
                    [--include [INCLUDE [INCLUDE ...]] | --exclude
                    [EXCLUDE [EXCLUDE ...]]]

Extract traefik letsencrypt certificates.

optional arguments:
  -h, --help            show this help message and exit
  -c CERTIFICATE, --certificate CERTIFICATE
                        file that contains the traefik certificates (default
                        acme.json)
  -d DIRECTORY, --directory DIRECTORY
                        output folder
  -f, --flat            outputs all certificates into one folder
  -r, --restart_container
                        uses the docker API to restart containers that are
                        labeled accordingly
  --dry-run             Don't write files and do not start docker containers.
  --include [INCLUDE [INCLUDE ...]]
  --exclude [EXCLUDE [EXCLUDE ...]]
```
Default file is `./data/acme.json`. The output directories are `./certs` and `./certs_flat`.

## Docker

There is no image on hub.docker.com for this fork. Feel free to build your own by cloning this repository and using the included Dockerfile.  It ought to work. Maybe.

## Output
```
certs/
    example.com/
        cert.pem
        chain.pem
        fullchain.pem
        privkey.pem
    sub.example.nl/
        cert.pem
        chain.pem
        fullchain.pem
        privkey.pem
certs_flat/
    example.com.crt
    example.com.key
    example.com.chain.pem
    sub.example.nl.crt
    sub.example.nl.key
    sub.example.nl.chain.pem
```
