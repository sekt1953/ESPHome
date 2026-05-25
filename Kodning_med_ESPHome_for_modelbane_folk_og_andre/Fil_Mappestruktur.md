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

## Kort Mappe Beskrivelse

* Mapper
  * homeassistant/esphome/archive
    * Her gemmer ESPHome slettede esphome filer
  * homeassistant/esphome/archive_distribute/
    * Her placeres esphome filer som er sat i drift
  * homeassistant/esphome/boards
    * Her placeres boards package-filer, i deres respektive MCU mapper.
  * homeassistant/esphome/common/core/
    * Her placeres common/core package-filer.
  * homeassistant/esphome/common/network/
    * Her placeres common/network package-filer.
  * homeassistant/esphome/peripherals/
    * Her placeres, i deres respektive mapper, peripherals package-filer, som:  
      display-, motor_controller-, light_controller-package,  
      alså output-package, som bruges til at styre enheder.
  * homeassistant/esphome/sensors/

  * homeassistant/esphome/

## Særlige filer

* **Advarsel !!!**
  * Der er 2 stk secrets.yaml i en Home Assistant installation
    1. **homeassistant/secrets.yaml** som er for Home Assistant secrets.
    2. **homeassistant/esphome/secrets.yaml** som er for ESPHome secrets.
  * herunder taler vi kun on nr.2 **homeassistant/esphome/secrets.yaml**

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

### esphome/.gitignore

```yaml
# Gitignore settings for ESPHome
# This is an example and may include too much for your use-case.
# You can modify this file to suit your needs.
/.esphome/
/secrets.yaml
```