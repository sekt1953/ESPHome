# Fil- og Mappestruktur

## Mapper og Filer navne

```txt
homeassistant "set som CONFIG i Studio Code Server"
  ├─ esphome
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
  │    │    │    ├─ webserver.yaml
  │    │    │    ├─ wifi.yaml
  │    ├─ peripherals
  │    │    ├─ spi
  │    │    ├─ i2c
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

## Mappe Beskrivelse

### esphome/archive_distribute

## Filer

### esphome/boards/esp32/wroom_32d_hw_394.yaml

```txt
################################################################################
# ESP32D-Wroom-HW-394 used USB-C
################################################################################
# Anvendelse:
#   Tilføj følgende kode til pakkeafsnittet i enhedsfilen
# ------------------------------------------------------------------------------
# packages:
#   board: !include 
#     file: boards/esp32/wroom_32d_hw_394.yaml
#     vars:
#       i2c_scan: true
#       uart_baud_rate: 115200
#       framework_version: recommended 
# ------------------------------------------------------------------------------
# Note about Framework: https://esphome.io/components/esp32/#framework   
# framework_version:
#  - dev         : Use the latest commit, note this may break at any time
#  - latest      : Use the latest release, even if it hasn’t been recommended yet.
#  - recommended : Use the recommended framework version.  
# ------------------------------------------------------------------------------
# Krav:
#   Kræver følgende erstatning i enhedsfilen:
#     - device_internal_name
#     - device_sampling_time
#
################################################################################
# PinOut; 
#   https://www.espboards.dev/img/S36MIHQRiM-900.png
#
# Safe Pins to Use:
#  -GPIO13	avoid using as GPIO if JTAG is needed.
#   GPIO14	produce spurious signals at boot.
#   GPIO16	
#   GPIO17	
#  -GPIO18  SPI/CLK
#  -GPIO19  SPI/MISO
#  -GPIO21  I2C/SDA
#  -GPIO22  I2C/SCL
#  -GPIO23  SPI/MOSI
#  -GPIO25
#  -GPIO26
#  -GPIO27
#  -GPIO32
#  -GPIO33
#   GPIO34	only suitable for analog/digital input.
#   GPIO35	only suitable for input.
#
################################################################################
# Author: Svenn-Erik K. Thomsen
# Web: https://www.youtube.com/@sekt1953
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# Notes:
#     - ikke Tested OK
################################################################################
# Board Specific Configurations
# ------------------------------------------------------------------------------
# This specification file is specific to the ESP32 S3 N16R8 development board!
# Do not try to use another!
# ------------------------------------------------------------------------------
esp32:
  board: esp32dev
  framework:
    type: arduino
    version: ${framework_version}

# Status led
status_led:
  id: ${device_internal_name}_status_led
  pin:
    number: GPIO2
    inverted: false

# I2C settings
i2c:
  id: ${device_internal_name}_i2c_bus0
  sda: GPIO21
  scl: GPIO22
  scan: ${i2c_scan}

# UART
uart:
  id: ${device_internal_name}_uart_bus0
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: ${uart_baud_rate}
  parity: NONE
  stop_bits: 1

# SPI
spi:
  id: ${device_internal_name}_spi_bus0
  clk_pin: GPIO18
  mosi_pin: GPIO23
  miso_pin: GPIO19
  interface: hardware

# Sensors
sensor:
  # Internal Temperature Sensor
  - platform: internal_temperature
    id: ${device_internal_name}_internal_temperature
    name: "Internal Temperature"
    update_interval: ${device_sampling_time}
    disabled_by_default: true
```

### esphome/common/core/settings.yaml

```txt
################################################################################
# Common Core Settings
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   settings: !include 
#     file: common/core/settings.yaml
#     vars:
#        time_region: Europe/Copenhagen
# -----------------------------------------------------------------------------      
# Notes:
#     - Uses the following substitutions from the project file:
#       - device_friendly_name   
#       - device_internal_name
#       - device_sampling_time
#       - time_region
#     - Uses the following from the secrets file:
#       - api_encryption_key
#       - web_server_password
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

# ----------------------------------------------------------------------------- 
# Board Configuration
esphome:
  name: ${device_internal_name}
  friendly_name: ${device_friendly_name}

# ----------------------------------------------------------------------------- 
# Enable logging
logger:

# ----------------------------------------------------------------------------- 
# Enable time syncronisation from Home Assistant
time:
  - platform: homeassistant
    id: ${device_internal_name}_internal_time
    timezone: ${time_region}

# ----------------------------------------------------------------------------- 
# Enable Home Assistant API
api:
  id: ${device_internal_name}_ha_api
  encryption:
    key: !secret api_encryption_key
  reboot_timeout: 0s

# ----------------------------------------------------------------------------- 
# OTA Settings
ota:
  platform: esphome
  password: !secret web_server_password

# ----------------------------------------------------------------------------- 
# Safe Mode
safe_mode:
  disabled: false
  reboot_timeout: 10min
  num_attempts: 5

# -----------------------------------------------------------------------------
# Network Settings
mdns:
  disabled: false # Ensure mDNS is enabled

# Disable IPV6
network:
    enable_ipv6: false

# ----------------------------------------------------------------------------- 
# Binary Sensors
binary_sensor:
  # ESP Status Sensor
  - platform: status
    id: ${device_internal_name}_status
    name: Status
    icon: mdi:network-pos
    disabled_by_default: true

# ----------------------------------------------------------------------------- 
# Sensors
sensor:
  # Human Readable Uptime Conversion Sensor
  - platform: uptime
    id: ${device_internal_name}_uptime_sensor
    name: Uptime Sensor
    update_interval: ${device_sampling_time}

# ----------------------------------------------------------------------------- 
# Text Sensors
text_sensor:
  # ESP32 Version Sensor
  - platform: version
    id: ${device_internal_name}_version
    name: ESPHome Version
    hide_timestamp: true
    disabled_by_default: true

# ----------------------------------------------------------------------------- 
# Switch
switch:
  # Soft Restart the device
  - platform: restart
    id: ${device_internal_name}_device_restart
    name: Restart

  # Restart the device in Safe Mode
  - platform: safe_mode
    id: ${device_internal_name}_device_safe_mode
    name: Use Safe Mode
    disabled_by_default: true
```

## esphome/common/network/static_ip.yaml

```txt
################################################################################
# Stacic IP Package for WiFi & Ethernet Include file
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# Include for WiFi:
# wifi:
#   <<: !include common/network/static_ip.yaml
# 
# Include for Ethernet:
# ethernet:
#   <<: !include common/network/static_ip.yaml
# ------------------------------------------------------------------------------
# Requirement: 
#   - Requires the following substitution in the device file:
#       - device_static_ip:
#
#   - Requires the following from the secrets file:
#       - gateway_address
#       - subnet_address
#
################################################################################
# Author: Svenn-Erik K. Thomsen
# YouTube: https://www.youtube.com/@sekt1953
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
################################################################################
# Notes:
#     - ikke Tested OK
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
################################################################################
static_ip:
  static_ip: ${device_static_ip}
  gateway: !secret gateway_address
  subnet: !secret subnet_address
```

### esphome/common/network/webserver.yaml

```txt
################################################################################
# Web Server Package for ESP32
################################################################################
# Reference(s): https://esphome.io/components/web_server.html
# ------------------------------------------------------------------------------  
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   webserver: !include common/network/webserver.yaml
# ------------------------------------------------------------------------------
# Requirement:
#     - Requires either WiFi or Ethernet to be enabled on the board
#     - Uses the following from the secrets file:
#       - web_server_user
#       - web_server_password
#
# Warning:
#     - On low SRAM or PSRAM boards, avoid using the Web Server with Bluetooth,
#       or Voice Assistant, it may cause instability or reboot loops, 
#       rather switch the radio off.
# ------------------------------------------------------------------------------     
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

web_server:
  port: 80
  version: 3
  include_internal: true
  auth:
    username: !secret web_server_user
    password: !secret web_server_password
  local: true
```

### esphome/common/network/wifi.yaml.yaml

```txt
################################################################################
# WiFi Package for ESP32
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   wifi: !include common/network/wifi_dynamicip.yaml
# ------------------------------------------------------------------------------
# Requirement: 
#   - Requires the following substitution in the device file:
#       - device_wifi_name:
#
#   - Requires the following from the secrets file:
#       - wifi_ssid:
#       - wifi_password;
#       - ap_password:
#
################################################################################
# Author: Svenn-Erik K. Thomsen
# YouTube: https://www.youtube.com/@sekt1953
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
################################################################################
# Notes:
#     - ikke Tested OK
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
################################################################################
wifi:
  networks:
    - ssid: !secret wifi_ssid
      password: !secret wifi_password
  fast_connect: true
  domain: .local
  

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: ${device_wifi_name}
    password: !secret ap_password

captive_portal:
```

### esphome/peripherals/spi

```txt

```

### esphome/sensors/i2c/bh1750.yaml

```txt
################################################################################
# BH1750 Illuminance Sensor Package
################################################################################
# Schematic – ESP32 with BH1750
#   - https://i0.wp.com/randomnerdtutorials.com/wp-content/uploads/2022/01/ESP32-BH1750-Wiring-Diagram.png?w=672&quality=100&strip=all&ssl=1
#
################################################################################
# Usage:
#   Add the following code to package section in the device file changing the 
#   I2C Address to the appropriate one.
# -----------------------------------------------------------------------------
# packages:
#  bh1750: !include 
#    file: sensors/i2c/bh1750.yaml
#    vars:
#      i2c_address: 0x23
# ------------------------------------------------------------------------------
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# Reference: https://esphome.io/components/sensor/bh1750.html
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
################################################################################
sensor:  
  - platform: bh1750
    name: BH1750 Illuminance
    id: ${device_internal_name}_bh1750_illuminance
    icon: mdi:sun-wireless-outline
    address: ${i2c_address}
    on_value:
      then:
        component.update: ${device_internal_name}_bh1750_illuminance_readable
    device_class: illuminance
    state_class: measurement
    update_interval: ${device_sampling_time}

text_sensor:
  - platform: template
    id: ${device_internal_name}_bh1750_illuminance_readable
    name: BH1750 Illuminance Human Readable
    icon: mdi:sunglasses
    lambda: |-
      std::string brightness = "";
      int lux = id(${device_internal_name}_bh1750_illuminance).state;
      if (lux <= 200) {
        brightness = "Dark";
      }
      else if (lux <= 300) {
        brightness = "Dim";
      }
      else if (lux <= 500) {
        brightness = "Ideal";
      }
      else if (lux <= 750) {
        brightness = "Bright";
      }
      else if (lux <= 1000) {
        brightness = "Very Bright";
      }
      else if (lux >= 1000) {
        brightness = "Too Bright";
      }
      else {
        brightness = "Unknown";
      }
      return brightness;
    update_interval: never
```

### esphome/sensors/environment


```txt

```

### Shell filer

### esphome/esphome-room-sensors_wifi_static_ip.yaml

```txt
################################################################################
# Substitution Variables
################################################################################
substitutions:
  device_internal_name: esphome_study_sensors
  device_friendly_name: ESPHome Study Sensors
  device_wifi_name: esphome-study-sensors
  device_sampling_time: 30s
  device_static_ip: 192.168.0.227

################################################################################
# Common Packages
################################################################################
packages:
  # Include ESPHome Board Configuration 
  # version: 2.0.9
  board: !include 
    file: boards/esp32/wroom_32d_hw_394.yaml
    vars:
      i2c_scan: true
      uart_baud_rate: 115200
      framework_version: recommended 

  # Include Common Core ESPHome definitions
  settings: !include 
    file: common/core/settings.yaml
    vars:
      time_region: Europe/Copenhagen

  # Include Common Network Definition
  wifi: !include common/network/wifi.yaml
  
  webserver: !include common/network/webserver.yaml

  ##############################################################################
  # Sensors
  ##############################################################################

  bh1750: !include 
    file: sensors/i2c/bh1750.yaml
    vars:
      i2c_address: 0x23

################################################################################
# Include files: static_ip
################################################################################
wifi:
  <<: !include common/network/static_ip.yaml

```
