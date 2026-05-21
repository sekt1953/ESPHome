# ESPHome, Coding with packages and substitutions

## Intro

* Place where i will place my new ESPHome project, which is heavily inspired by the [Home Automator](https://www.youtube.com/@homeautomatorza/featured) video series ["Coding Practices, Tips and Tricks"](https://www.youtube.com/playlist?list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I)
* Before you start using this guide, watch at least these videos :
  * [home-automator - ESPHome Coding Practices Tips and Tricks](https://www.youtube.com/playlist?list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I)
* and articles:
  * [olegtarasov - packages and substitutions tutorial](https://olegtarasov.me/esphome-packages-substitutions-tutorial/)
  * [ESPHome.io - Packages](https://esphome.io/components/packages/)

### Sources

* [ESPHome Dokumentation](./Sources/ESPHome_Dokumentation.md)
* [ESP32 Development Boards](./Sources/ESP32_Development_Boards.md)
* [YouTube Videos to learn from](./Sources/YouTube_Videos.md)
  * [Home Automator](./Sources/YouTube_Videos.md#home-automator)
    * [Good Folder structure, 14:59](https://www.youtube.com/watch?v=G9WRg6jk7xk&t=899s "Home Automator")
    * [Mudolar code with Package & Include: 1.57 to time: 16.19](https://youtu.be/52_ZJmTz3bs?t=116)
    * []()
    * []()
    * []()
* [GitHub Artikler and more](./Sources/GitHub_Artikler.md)

## Folder & Packages files Overview

* Video clip to see
  * [ESPHome Coding practices, tips and tricks, Tutorial 1, Foundation Part 2, 14:59 - Folders](https://www.youtube.com/watch?v=G9WRg6jk7xk&t=899s "Home Automator")

### Folder

```txt
homeassistant "set som CONFIG i Studio Code Server"
  ├─ esphome
  │    ├─ boards
  │    │    ├─ esp32
  │    │    │    ├─ c3_mini_espressif.yaml
  │    │    │    ├─ poe_flash4_olimex.yaml
  │    │    │    ├─ poe_iso_flash16_olimex.yaml
  │    │    ├─ esp8266
  │    │    ├─ rp2040
  │    ├─ common
  │    │    ├─ core
  │    │    │    ├─ settings.yaml
  │    │    ├─ network
  │    │    │    ├─ lan.yaml
  │    │    │    ├─ webserver.yaml
  │    │    │    ├─ wifi.yaml
  │    │    │    ├─ bluetooth_off.yaml    
  │    ├─ includes
  │    ├─ peripherals
  │    │    ├─ spi
  │    │    ├─ i2c
  │    ├─ sensors
  │    │    ├─ analogue
  │    │    ├─ binary
  │    │    ├─ bluetooth
  │    │    ├─ core
  │    │    ├─ i2c
  │    │    ├─ one_wire
  │    │    ├─ spi
  │    │    ├─ uart
  │    ├─ templates
  │    ├─ secrets.yaml
  │    ├─ .gitignore
  │    │
  │    ├─ esphome_device_tutorial.yaml
```

### Packages files

* Let's start with OMJK's "external control cabinet", here I use an OLIMEX ESP32-POE with ESP32-D0WD V3 and 4 MB Flash, and I use the I2C interface for mcp23017 to connect push buttons and GPIO to control NeoPixel Light SK6812.
* The "external control cabinet" is powered via PoE, since I want to use PoE, I will therefore not use WiFi; but Ethernet (LAN).
* Video Clip:
  * [See this Video, from time: 1.57 to time: 16.19](https://youtu.be/52_ZJmTz3bs?t=116)

#### Boards Folder

* Video Clip:
  * [See this Video, from time: 16.19 to time: 33.02](https://youtu.be/52_ZJmTz3bs?t=979)

```yaml
################################################################################
# Olimex ESPHome PoE Ethernet
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   board: !include boards/esp32/poe_ethernet_olimex.yaml 
# ------------------------------------------------------------------------------
# substitutions used:
#   device_internal_name
#   device_sampling_time
# ------------------------------------------------------------------------------
# Author: Svenn-Erik K. Thomsen
# Email: sekt1953@gmail.com
# Web: https://www.youtube.com/@sekt1953
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# Notes: 
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
# Use at your own risks!
################################################################################
esp32:
  board: esp32-poe
  framework:
    type: arduino
    version: latest

# I2C settings
i2c:
  id: ${device_internal_name}_i2c_bus0
  sda: GPIO21
  scl: GPIO22
  scan: true

# Sensors
sensor:
  # Internal Temperature Sensor
  - platform: internal_temperature
    id: ${device_internal_name}_internal_temperature
    name: "Internal Temperature"
    update_interval: ${device_sampling_time}
    disabled_by_default: true
```

#### Common Folder

* Video Clip:
  * [See this Video, from time: 40.44 to time: 40.44](https://youtu.be/52_ZJmTz3bs?t=2444)


#### Tempalte File

* Video Clip:
  * [See this Video, from time: 33.10 to time: 40.44](https://youtu.be/52_ZJmTz3bs?t=1982)

```yaml
################################################################################
# Udvendig Betjenings Pult
################################################################################
# The components used are:
#     - Generic 
#         - Enabled hardware and services:
#             - Ethernet
#             - I2C 
#                 - SDA: GPIO3
#                 - SDA: GPIO4
#                 - ID: bus_a
#                 - Frequency: 400kHz
#             - NeoPixelBus
#                 - Pin: GPIO2
# ------------------------------------------------------------------------------
# Author: Svenn-Erik K. Thomsen
# Email: sekt1953@gmail.com
# Web: https://www.youtube.com/@sekt1953
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# Notes: 
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
# Use at your own risks!
################################################################################

################################################################################
# Substitution Variables
################################################################################
substitutions:
  device_internal_name: esphome_poe_ethernet_olimex 
  device_wifi_name: esphome-_poe-ethernet-olimex
  device_friendly_name: ESPHome ESP32-Poe Ethernet Olimex
  device_ip_address: 192.168.0.102
  device_sampling_time: 30s
  region: Europe/Copenhagen # tz time zones

################################################################################
# Packages
################################################################################
packages:
  # ----------------------------------------------------------------------------
  # Core packages
  # ----------------------------------------------------------------------------
  board: !include boards/esp32/poe_ethernet_olimex.yaml
  settings: !include common/core/settings.yaml
  time: !include common/time/home_assistant.yaml

  # Network
  Ethernet: !include common/network/ethernet.yaml
  webserver: !include common/network/webserver.yaml

################################################################################
# Board Configuration
################################################################################
esphome:
  comment: Hjulby Udvendig Betjenings Skab


```

## OLD




* [Common Core File](./My_ESPHome_Setup/Demofiles.md#common-core-file)
  * [settings.yaml](./My_ESPHome_Setup/Demofiles.md#homeassistantesphomecommoncoresettingsyaml)
* [Common Network File](./My_ESPHome_Setup/Demofiles.md#common-network-file)
  * [webserver.yaml](./My_ESPHome_Setup/Demofiles.md#homeassistantesphomecommonnetworkwebserveryaml)
  * [wifi.yaml](./My_ESPHome_Setup/Demofiles.md#homeassistantesphomecommonnetworkwifiyaml)
  * [bluetooth_off.yaml](./My_ESPHome_Setup/Demofiles.md#homeassistantesphomecommonnetworkbluetooth_offyaml)
* [The Board File](./My_ESPHome_Setup/Demofiles.md#the-cpu-board-file)
  * [c3_mini_espressif.yaml](./My_ESPHome_Setup/Demofiles.md#homeassistantesphomeboardsesp32c3_mini_espressifyaml)
* [The Device File](./My_ESPHome_Setup/Demofiles.md#the-device-file)
  * [esphome_device_tutorial.yaml](./My_ESPHome_Setup/Demofiles.md#homeassistantesphomeesphome_device_tutorialyaml)
