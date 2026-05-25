# ESPHome files

## esphome_study_wifi_dynamic_ip.yaml

```yaml
################################################################################
# Substitution Variables
################################################################################
substitutions:
  device_internal_name: esphome_study_wifi_dynamic_ip
  device_friendly_name: ESPHome Study WiFi Dynamic IP
  device_wifi_name: esphome-study-wifi-dynamic-ip
  device_sampling_time: 30s

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
    file: sensors/environment/bh1750.yaml
    vars:
      i2c_address: 0x23

################################################################################
# Include files: static_ip
################################################################################
```

## esphome_study_wifi_static_ip.yaml

```txt
################################################################################
# Substitution Variables
################################################################################
substitutions:
  device_internal_name: esphome_study_wifi_static_ip
  device_friendly_name: ESPHome Study WiFi Static IP
  device_wifi_name: esphome-study-wifi-static-ip
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
    file: sensors/environment/bh1750.yaml
    vars:
      i2c_address: 0x23

################################################################################
# Include files: static_ip
################################################################################
wifi:
  <<: !include common/network/static_ip.yaml
```

