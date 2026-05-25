# Mappestruktur

## Mapper og Filer

```txt
homeassistant "set som CONFIG i Studio Code Server"
  ├─ esphome
  │    ├─ archive_distribute
  │    ├─ archive_distribute
  │    ├─ boards
  │    │    ├─ esp32
  │    │    │    ├─ wroom_32d_hw_394.yaml
  │    │    ├─ esp8266
  │    │    ├─ rp2040
  │    ├─ common
  │    │    ├─ core
  │    │    │    ├─ settings.yaml
  │    │    ├─ network
  │    │    │    ├─ lan.yaml
  │    │    │    ├─ static_ip.yaml
  │    │    │    ├─ webserver.yaml
  │    │    │    ├─ wifi.yaml
  │    ├─ peripherals
  │    │    ├─ display
  │    │    ├─ motor_controller
  │    │    │    ├─ sporskifte.yaml
  │    │    ├─ light_controller
  │    │    │    ├─ neopixel.yaml
  │    ├─ sensors
  │    │    ├─ presence_detection
  │    │    │    ├─ ld2410c.yaml
  │    │    │    ├─ hc_sr501.yaml
  │    │    ├─ distance
  │    │    ├─ environment
  │    │    │    ├─ hlk_bh1750.yaml 
  │    │    │    ├─ bme280.yaml 
  │    │    │    ├─ bme680.yaml 
  │    │    │    ├─ dth22.yaml
  │    ├─ templates
  │    ├─ secrets.yaml
  │    ├─ .gitignore
  │    │
  │    ├─ esphome_study_wifi_static_ip.yaml
```

## Mappe Beskrivelse

### esphome/archive_distribute

## Særlige filer

### esphome/secrets.yaml

* Note! : password og SSID er kun for at vise hvordan
  * jeg bruger https://generate.plus/en/base64 32Byte for encryption_key
  * jeg bruger https://generate.plus/en/passwords for Password

```txt
################################################################################
# Secrets sample file
################################################################################
# Encryption Key Generator: base64 string 
#     32Byte Base64 String https://generate.plus/en/base64
# Password Generator:
#     https://generate.plus/en/passwords
################################################################################
# Author: Svenn-Erik K. Thomsen
# Web: https://www.youtube.com/@sekt1953
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Use at your own risks!
################################################################################

# Api
api_encryption_key: "KLJhuLK31lsvG243t/uK8TYjneWwxnxJMcAUY2YCkLw="

# OTA
ota_password: "N5fkTsaKqc1CS2fjO2K4A9"

# Your Wi-Fi SSID and password
wifi_ssid: "My_SSID"
wifi_password: "CiafqFGnckn1qJ5JB2FEi7"
ap_password: "levInJZ3WjhrMt15CZudS0"

# Network settings
gateway_address: 192.168.8.1
subnet_address: 255.255.255.0
dns_address: 1.1.1.1,8.8.8.8

# Webserver
web_server_user: "admin"
web_server_password: "8AArjvC5VYz8"
```
