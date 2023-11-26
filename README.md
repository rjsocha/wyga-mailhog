# Mailhog Image

This Mailhog image is built for both Intel and M1 architectures. 

The upstream image on Docker Hub is outdated and lacks support for Mac's M1.

## Usage


```
---
version: "3"
name: "mailhog"

x-logging:
  &default-logging
  options:
    max-file: "10"
    max-size: "25M"
    compress: "true"

services:
  mailhog:
    image: wyga/mailhog:latest
    restart: unless-stopped
    logging: *default-logging
    labels:
      - wyga.expose=mailhog
    networks:
      gateway:
        priority: 100
      default:
        priority: 500

networks:
  gateway:
    name: wyga-site
    external: true
```
