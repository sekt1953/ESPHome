# ESPHome_Directory_Structure

## Sources

* [ESPHome Coding practices, tips and tricks, Tutorial 1, Foundation Part 2, Folder](https://www.youtube.com/watch?v=G9WRg6jk7xk&t=899s "Home Automator")

## My default layout for ESPHome in Home Assistant

* homeassistant   "set som **CONFIG** i Studio Code Server"
  * esphome
    * boards
      * esp32
      * esp8266
      * rp2040
    * common
      * core
      * network
    * includes
    * peripherals
      * spi
      * i2c
    * sensors
      * analogue
      * binary
      * bluetooth
      * core
      * i2c
      * one_wire
      * spi
      * uart
    * templates
    * secrets.yaml
    * .gitignore

## Some standard Files

### .gitignore

```code
# Gitignore settings for ESPHome
# You can modify this file to suit your needs.
/.esphome/
/secrets.yaml
```

### secrets.yaml helper file

```yaml
<<: !include ../../secrets.yaml
```

### /homeassistant/esphome/secrets.yaml

```yaml
################################################################################
# Secrets sample file
################################################################################
# Tutorial:     https://youtu.be/D9veJLKqnpg
# -----------------------------------------------------------------------------      
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Use at your own risks!
################################################################################

#Api
api_encryption_key: "key"

#OTA
ota_password: "key"

# Your Wi-Fi SSID and password
wifi_ssid: "IOT-SSID"
wifi_password: "password"
ap_password: "password"

# Network settings
gateway_address: 192.168.1.1
subnet_address: 255.255.255.0
dns_address: 192.168.1.1
```