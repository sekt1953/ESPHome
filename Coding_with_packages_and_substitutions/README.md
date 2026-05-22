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
  * [See this Video, from time: 1:57 to time: 16:19](https://youtu.be/52_ZJmTz3bs?t=116)

#### Boards Folder

* Video Clip:
  * [See this Video, from time: 16:19 to time: 33:02](https://youtu.be/52_ZJmTz3bs?t=979)

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
# Requires the following variables in the device file:
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

#### Common Core Settings file

* Video Clip:
  * [See this Video, from time: 40:44 to time: 1:15.21](https://youtu.be/52_ZJmTz3bs?t=2444)

```yaml
################################################################################
# Common Settings
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   settings: !include common/core/settings.yaml
# ------------------------------------------------------------------------------
# Requires the following variables in the device file:
#   - device_internal_name
#   - device_friendly_name
#   - device_sampling_time
################################################################################
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.2.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
################################################################################
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
################################################################################
# ESPHome Configuration
################################################################################
esphome:
  name: ${device_internal_name}
  friendly_name: ${device_friendly_name}

################################################################################
# Enable logging
################################################################################
logger:
  id: ${device_internal_name}_logger

################################################################################
# Enable Home Assistant API
################################################################################
api:
  id: ${device_internal_name}_ha_api
  reboot_timeout: 0s

################################################################################
# OTA Settings
################################################################################
ota:
  - platform: esphome

################################################################################
# Safe Mode Settings
################################################################################
safe_mode:
  boot_is_good_after: 1min
  disabled: false
  reboot_timeout: 10min
  num_attempts: 5

################################################################################
# Network Settings
################################################################################
# Ensure mDNS is enabled
mdns:
  disabled: false

# Disable IPV6
network:
    enable_ipv6: false

################################################################################
# Binary Sensors
################################################################################
binary_sensor:
  # ESP Status Sensor
  - platform: status
    id: ${device_internal_name}_status
    name: "Status"
    icon: mdi:network-pos
    disabled_by_default: true

################################################################################
# Text Sensors
################################################################################
text_sensor:
  # ESP32 Version Sensor
  - platform: version
    id: ${device_internal_name}_version
    name: "ESPHome Version"
    hide_timestamp: true
    disabled_by_default: true

  - platform: uptime
    name: "Uptime"

################################################################################
# Button
################################################################################
button: # Allows to remotely force into safe mode
  - platform: safe_mode
    id: ${device_internal_name}_safe_mode
    name: Enter Safe Mode
    disabled_by_default: true

################################################################################
# Switch
################################################################################
switch:
  # Soft Restart the device
  - platform: restart
    id: ${device_internal_name}_device_restart
    name: Restart
    disabled_by_default: true

  # Restart the device in Safe Mode
  - platform: safe_mode
    id: ${device_internal_name}_device_safe_mode
    name: Use Safe Mode
    disabled_by_default: true
```

#### Common Network WiFi

* Video Clip:
  * [See this Video, from time: 1:15:21 to time: 1:39:43](https://youtu.be/52_ZJmTz3bs?t=4521)

```yaml
################################################################################
# WiFi Common Component for ESP32
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   wifi: !include common/network/wifi.yaml
# ------------------------------------------------------------------------------
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.4.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# Reference: https://www.esphome.io/components/wifi
#            https://www.esphome.io/components/text_sensor/wifi_info
#            https://www.esphome.io/components/sensor/wifi_signal
# ------------------------------------------------------------------------------
# Notes: 
#       - Requires the following substitution in the device file:
#           - device_ip_address
#           - device_internal_name
#           - device_wifi_name
#           - device_sampling_time
#       - Requires the folowing secrets:
#           - wifi_ssid
#           - subnet_address
#           - gateway_address
#           - subnet_address
#           - dns_address (Optional)
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
################################################################################
# Globals
################################################################################
globals: ##to set default reboot behavior
  - id: ${device_internal_name}_wifi_connection
    type: bool
    restore_value: no
    initial_value: "false"

  - id: ${device_internal_name}_wifi_radio_status
    type: bool
    restore_value: no
    initial_value: "false"

################################################################################
# WiFi Settings
################################################################################
# Please note that all wifi settings have moved to wifi_fixedip.yaml or wifi_dynamicip.yaml

################################################################################
# Captive Portal
################################################################################
captive_portal:

################################################################################
# Sensors
################################################################################
sensor:
  - platform: wifi_signal
    id: ${device_internal_name}_wifi_signal_db
    name: WiFi Signal dBm
    icon: mdi:wifi
    update_interval: ${device_sampling_time}
    entity_category: diagnostic
    disabled_by_default: true
    web_server:
      sorting_weight: 3
      sorting_group_id: wifi_group

  #See https://esphome.io/components/sensor/wifi_signal.html for details
  - platform: copy
    source_id: ${device_internal_name}_wifi_signal_db
    id: ${device_internal_name}_wifi_signal_percentage
    name: WiFi Signal Percentage
    icon: mdi:wifi
    filters:
      - lambda: return min(max(2 * (x + 100.0), 0.0), 100.0);
    unit_of_measurement: "%"
    entity_category: diagnostic
    disabled_by_default: true
    web_server:
      sorting_weight: 4
      sorting_group_id: wifi_group

################################################################################
# Text Sensors
################################################################################
text_sensor:
  # WiFi internal sensors
  - platform: wifi_info
    ip_address:
      id: ${device_internal_name}_wifi_ip_address
      name: IP Address
      icon: mdi:wifi
      web_server:
        sorting_weight: 1
        sorting_group_id: wifi_group
    ssid:
      id: ${device_internal_name}_connected_ssid
      name: Connected SSID
      icon: mdi:wifi
      web_server:
        sorting_weight: 2
        sorting_group_id: wifi_group
    mac_address:
      id: ${device_internal_name}_wifi_mac_address
      name: Mac Wifi Address
      icon: mdi:wifi
      disabled_by_default: true
      web_server:
        sorting_weight: 51
        sorting_group_id: wifi_group
    dns_address:
      id: ${device_internal_name}_wifi_dns_address
      name: DNS Address
      icon: mdi:network
      disabled_by_default: true
      web_server:
        sorting_weight: 52
        sorting_group_id: wifi_group

  # WiFi Strength human readable
  # I had to remove the dynamic icon, I am still investigating but at first glance the set_icon() as been depercated and not replaced in 2026.4
  # https://developers.esphome.io/blog/2026/03/12/icon-and-device-class-getter-migration/
  - platform: template
    id: ${device_internal_name}_wifi_strength
    name: Wifi Signal Strength
    icon: mdi:wifi
    lambda: |-
      if (id(${device_internal_name}_wifi_signal_percentage).state >= 85) {
        return std::string("Excellent");
      } else if (id(${device_internal_name}_wifi_signal_percentage).state > 65) {
        return std::string("Good");
      } else if (id(${device_internal_name}_wifi_signal_percentage).state > 30) {
        return std::string("Fair");
      } else if (id(${device_internal_name}_wifi_signal_percentage).state > 10) {
        return std::string("Weak");
      } else {
        return std::string("None");
      }
    entity_category: diagnostic
    update_interval: ${device_sampling_time}
    web_server:
      sorting_weight: 5
      sorting_group_id: wifi_group
      
################################################################################
# Switches
################################################################################
switch: 
  # WiFi Radio Control 
  - platform: template
    id: ${device_internal_name}_wifi_radio_switch
    name: WiFi Radio Switch
    icon: mdi:wifi
    lambda: return ${device_internal_name}_wifi_radio_status;
    turn_on_action:
      - lambda: wifi::global_wifi_component->enable();
      - lambda: id(${device_internal_name}_wifi_radio_status) = true;
      - delay: 2s
    turn_off_action:
      - lambda: wifi::global_wifi_component->disable();
      - lambda: id(${device_internal_name}_wifi_radio_status) = false;
      - delay: 2s
    entity_category: config
    disabled_by_default: true
    web_server:
      sorting_weight: 53
      sorting_group_id: wifi_group

################################################################################
# Interval
################################################################################
interval:
  - interval: 10s
    then:
      - if:
          condition:
            wifi.connected:
          then:
            - lambda: |-
                id(${device_internal_name}_wifi_connection) = true;
          else:
            - lambda: |-
                id(${device_internal_name}_wifi_connection) = false;
      - if:
          condition: wifi.enabled
          then:
            - lambda: |-
                id(${device_internal_name}_wifi_radio_status) = true;
          else:
            - lambda: |-
                id(${device_internal_name}_wifi_radio_status) = false;
```

#### Common Network Webserver

* Video Clip:
  * [See this Video, from time: 1:39:40 to time: 1:39:43](https://youtu.be/52_ZJmTz3bs?t=5980)

```yaml
################################################################################
# Web Server Common Component for ESP32
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   webserver: !include common/network/webserver.yaml
# ------------------------------------------------------------------------------
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.1.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# Reference: 
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
################################################################################
web_server:
  port: 80
  version: 3
  local: true
  include_internal: true
  auth:
    username: !secret web_server_user
    password: !secret web_server_password
  sorting_groups:
    - id: wifi_group
      name: Wi-Fi Diagnostic
      sorting_weight: 90
    - id: ble_group
      name: BLE Diagnostic
      sorting_weight: 91
    - id: device_group
      name: Hardware Information
      sorting_weight: 92
```

#### Tempalte File

* Video Clip:
  * [See this Video, from time: 33:10 to time: 40:44](https://youtu.be/52_ZJmTz3bs?t=1982)

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
