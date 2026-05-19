# Demofiles

## Common Core File

### /homeassistant/esphome/common/core/settings.yaml

```yaml
################################################################################
# 
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   board: !include common/core/settings.yaml
# -----------------------------------------------------------------------------      
# Notes:
#     - Uses the following substitutions from the project file:
#       - device_friendly_name   
#       - device_internal_name
#       - device_sampling_time
#       - region
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
# Change: 20260519 23:03, Svenn-Erik K. Thomsen
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
    timezone: ${region}

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

## Common Network File

### /homeassistant/esphome/common/network/webserver.yaml

```yaml
################################################################################
# Web Server Common Component for ESP32
################################################################################
# Tutorial:     https://youtu.be/
# Reference(s): https://esphome.io/components/web_server.html
# ------------------------------------------------------------------------------  
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   board: !include common/network/webserver.yaml
# ------------------------------------------------------------------------------
# Notes:
#     - Requires either WiFi or Ethernet to be enabled on the board
#     - Uses the following from the secrets file:
#       - web_server_user
#       - web_server_password
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
# Change: 20260519 23:02, Svenn-Erik K. Thomsen
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

### /homeassistant/esphome/common/network/wifi.yaml

```yaml
################################################################################
# WiFi Common Component for ESP32
################################################################################
# Tutorial:     https://youtu.be/
# Reference(s): https://docs.espressif.com/projects/arduino-esp32/en/latest/boards/ESP32-C3-DevKitM-1.html
# ------------------------------------------------------------------------------  
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   board: !include common/network/wifi.yaml
# ------------------------------------------------------------------------------
# Notes:
#     - Uses the following substitutions from the project file:
#       - device_internal_name
#       - device_sampling_time
#       - device_ip_address
#     - Uses the following from the secrets file:
#       - wifi_ssid
#       - wifi_password
#       - gateway_address
#       - subnet_address
#       - dns_address (optional)
#       - ap_password
# ------------------------------------------------------------------------------     
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# Change: 20260519 23:01, Svenn-Erik K. Thomsen
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Use at your own risks!
################################################################################

# ------------------------------------------------------------------------------
# WiFi Settings
wifi:
  networks:
    - ssid: !secret wifi_ssid
      password: !secret wifi_password
  manual_ip:
    static_ip: ${device_ip_address}
    gateway: !secret gateway_address
    subnet: !secret subnet_address
  domain: .local # Network domain name.
  fast_connect: true # Reconnect with the last access point without searching.
  ap: # Enable fallback hotspot (captive portal) in case wifi connection fails.
    ssid: ${device_wifi_name}
    password: !secret ap_password

captive_portal:

# ------------------------------------------------------------------------------
# Sensors
sensor:
  - platform: wifi_signal
    id: ${device_internal_name}_wifi_signal_db
    name: WiFi Signal dB
    icon: mdi:wifi
    update_interval: ${device_sampling_time}
    entity_category: diagnostic
    disabled_by_default: true

# ------------------------------------------------------------------------------
# Text Sensors
text_sensor:
  # WiFi internal sensors
  - platform: wifi_info
    ip_address:
      id: ${device_internal_name}_ip_address
      name: IP Address
      icon: mdi:ip-network
    ssid:
      id: ${device_internal_name}_connected_ssid
      name: Connected SSID
      icon: mdi:access-point
    mac_address:
      id: ${device_internal_name}_mac_address
      name: Mac Wifi Address
      icon: mdi:network-outline
      disabled_by_default: true
    dns_address:
      id: ${device_internal_name}_dns_address
      name: DNS Address
      icon: mdi:dns
      disabled_by_default: true
```

### /homeassistant/esphome/common/network/bluetooth_off.yaml

```yaml
################################################################################
# Bluetooth Common Component for ESP32
################################################################################
# Reference(s):
#         - https://esphome.io/components/esp32_ble.html
#         - https://esphome.io/components/esp32_ble_tracker.html 
#         - https://esphome.io/components/bluetooth_proxy.html
# ------------------------------------------------------------------------------  
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   board: !include common/network/bluetooth_off.yaml
# ------------------------------------------------------------------------------
# Notes:
#     - Enables the Bluetooth Radio, Low Energy Tracker Hub and the Bluetooth
#       Proxy.
#     - Not compatible with the ESP8266 or the Raspberry Pi Pico or any boards 
#       without Bluetooth.
#     - If you want to reduce power consumption you can disable the Bluetooth
#       radio by using the bluetooth_off.yaml in this directory
#     - On low SRAM or PSRAM boards, avoid using bluetooth with the Web Server,
#       or Voice Assistant, it may cause instability or reboot loops, 
#       rather switch the radio off.
# ------------------------------------------------------------------------------     
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# Change: 20260519 23:00, Svenn-Erik K. Thomsen
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Use at your own risks!
################################################################################
esp32_ble:
  id: ${device_internal_name}_ble
  enable_on_boot: false
```

## The CPU Board File

* Video
  * [02:23 - Board Definition Module - Our First Device](https://www.youtube.com/watch?v=eyEGK1eKyDM&list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I&index=2&t=846s "Home Automator")

### /homeassistant/esphome/boards/esp32/c3_mini_espressif.yaml

```yaml
################################################################################
# Espressif ESP32-C3-MINI-1
################################################################################
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   board: !include boards/esp32/c3_mini_espressif.yaml
# ------------------------------------------------------------------------------
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# Change: 20260519 20:46, Svenn-Erik K. Thomsen
# ------------------------------------------------------------------------------
# Notes:
#     - 
# ------------------------------------------------------------------------------
# Referance: 
#   - https://docs.espressif.com/projects/arduino-esp32/en/latest/boards/ESP32-C3-DevKitM-1.html
#   - https://www.espboards.dev/esp32/esp32-c3-devkitm-1/
# Spec: 
#     - ESP32-C3FN4 RISCV Single Core 
#     - 2.4 GHz Wi­Fi (802.11 b/g/n) and Bluetooth 5 module
#     - 4 MB flash
#     - 1 × I2C
#     - 1 × I2S
#     - 3 x SPI
#     - 2 x UART
#     - 2 × 12-bit SAR ADC
#     - 1 x RGB LED
#     - 1 x Internal Temperature Sensor
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
# Use at your own risks!
################################################################################
esp32:
  board: esp32-c3-devkitm-1
  framework:
    type: arduino
    version: latest

# Internal LEDs
light:
  - platform: esp32_rmt_led_strip
    id: ${device_internal_name}_onboard_rgb_led
    name: Onboard RGB LED
    icon: mdi:led-outline
    rgb_order: GRB
    pin: GPIO8
    num_leds: 1
    rmt_channel: 0
    chipset: ws2812
    entity_category: config
    disabled_by_default: true

# I2C settings
i2c:
  id: ${device_internal_name}_i2c
  sda: GPIO3
  scl: GPIO2
  scan: true

# UART
uart:
  id: ${device_internal_name}_uart_bus0
  tx_pin: GPIO21
  rx_pin: GPIO20
  baud_rate: 256000
  parity: NONE
  stop_bits: 1

# SPI
spi:
  id: ${device_internal_name}_spi_bus0
  clk_pin: GPIO0
  mosi_pin: GPIO1
  miso_pin: GPIO10
  interface: hardware

# Sensors
sensor:
  # Internal Temperature Sensor
  - platform: internal_temperature
    id: ${device_internal_name}_internal_temperature
    name: "Internal Temperature"
    update_interval: ${device_sampling_time}
    disabled_by_default: true

  - platform: template
    id: ${device_internal_name}_internal_temperature_f
    name: Internal Temperature Temperature (F)
    icon: mdi:temperature-fahrenheit
    unit_of_measurement: °F
    lambda: return id(${device_internal_name}_internal_temperature).state * 9/5+32;
    device_class: temperature
    update_interval: ${device_sampling_time}
    disabled_by_default: true
```

## The Device File

* Video
  * [07:43 - The Device File - Our First Device](https://www.youtube.com/watch?v=eyEGK1eKyDM&list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I&index=2&t=463s "Home Automator")
* Standard for device file
  * File Description
  * Substitutions
  * Common Packages
  * ESPHome Configurations
  * Custom Deviced Configuration
    * Sensors

### /homeassistant/esphome/esphome_device_tutorial.yaml

```yaml
################################################################################
# ESPHome ESP32 C3 Device Tutorial
################################################################################
# Tutorial:  [Our First Device](https://www.youtube.com/watch?v=eyEGK1eKyDM&list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I&index=2 "Home Automator")
# ------------------------------------------------------------------------------
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.0.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# ------------------------------------------------------------------------------
# Change: 20260519 21:13, Svenn-Erik K. Thomsen
# ------------------------------------------------------------------------------
# Notes:
#   - Because the webserver is active, the Bluetooth Radio is off
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Use at your own risks!
################################################################################

# ------------------------------------------------------------------------------
# Substitution Variables
# ------------------------------------------------------------------------------
substitutions:
  device_internal_name: esphome_c3_device_tutorial
  device_wifi_name: esphome-c3-device-tutorial
  device_friendly_name: ESPHome C3 Device Tutorial
  device_ip_address: 192.168.0.218
  device_sampling_time: 30s

# ------------------------------------------------------------------------------
# Common Packages
# ------------------------------------------------------------------------------
packages:
  # ----------------------------------------------------------------------------
  # Board
  board: !include boards/esp32/c3_mini_espressif.yaml
  settings: !include common/core/settings.yaml

  # ----------------------------------------------------------------------------
  # Network
  wifi: !include common/network/wifi.yaml
  webserver: !include common/network/webserver.yaml
  bluetooth: !include common/network/bluetooth_off.yaml

# ------------------------------------------------------------------------------
# ESPHome Configurations
# ------------------------------------------------------------------------------
esphome:
  comment: Using a Espressif ESP32-C3
# ------------------------------------------------------------------------------

# ------------------------------------------------------------------------------
# Custom Deviced Configuration
# ------------------------------------------------------------------------------

# ------------------------------------------------------------------------------
# Sensors
# This is where you add additional configurations to your device that is not
# included in you components.
# ------------------------------------------------------------------------------
  





```

### /homeassistant/esphome/common/core/settings.yaml

```yaml
################################################################################
# Hardware: DTH11/22 Temperature and Humidity Sensor
################################################################################
# Tutorial:     https://youtu.be/D9veJLKqnpg
# -----------------------------------------------------------------------------  
# Usage:
#   Add the following code to package section in the device file
# ------------------------------------------------------------------------------
# packages:
#   board: !include common/core/settings.yaml
# -----------------------------------------------------------------------------      
# Notes:
#     - Uses the following substitutions from the project file:
#       - device_friendly_name   
#       - device_internal_name
#       - device_sampling_time
#       - region
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
    timezone: ${region}

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

##


```yaml

```
