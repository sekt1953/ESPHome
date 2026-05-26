# Sensor Package

## Environment

### homeassistant/esphome/sensors/environment/bh1750.yaml

```yaml
################################################################################
# BH1750 Illuminance Sensor
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

### homeassistant/esphome/sensors/environment/ds18b20.yaml

```yaml
################################################################################
# DS18B20 Temperature Sensor
################################################################################
# Usage:
#   Add the following code to package section in the device file changing the 
#   one wire GPIO to the appropriate one.
# -----------------------------------------------------------------------------
# packages:
#  ds18b20: !include 
#    file: sensors/digital/other/ds18b20.yaml
#    vars:
#      data_pin: GPIO16
#      sensor_address: 0x7114d0d446197628
# ------------------------------------------------------------------------------
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.1.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# Reference: https://esphome.io/components/sensor/dallas_temp.html 
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
################################################################################
one_wire:
  - platform: gpio
    pin: ${data_pin}
    id: ${device_internal_name}_1_wire

sensor:
  - platform: dallas_temp
    name: DS18B20 Temperature
    id: ${device_internal_name}_ds18b20_temperature
    one_wire_id: ${device_internal_name}_1_wire
    address: ${sensor_address}
    resolution: 10
    icon: mdi:thermometer
    device_class: temperature
    state_class: measurement
    update_interval: ${device_sampling_time}

  - platform: template
    id: ${device_internal_name}_ds18b20_temperature_f
    name: DS18B20 Temperature (F)
    icon: mdi:temperature-fahrenheit
    unit_of_measurement: °F
    lambda: return id(${device_internal_name}_ds18b20_temperature).state * 9/5+32;
    device_class: temperature
    state_class: measurement
    update_interval: ${device_sampling_time}
    disabled_by_default: true
```

## Presence

### homeassistant/esphome/sensors/presence/hlk-ld2410c_minimal.yaml

```yaml
################################################################################
# HLK-LD2410C mmWave Presence Detection Sensor
################################################################################
# Source: 
#   https://naylampmechatronics.com/img/cms/001080/HLK-LD2410C_datasheet.pdf
# -----------------------------------------------------------------------------
# Usage:
#   Add the following code to package section in the device file changing the 
#   I2C Address to the appropriate one.
# -----------------------------------------------------------------------------
# packages:
#  ld2410c: !include 
#    file: sensors/presence/hlk-ld2410c_minimal.yaml
#    vars:
#      presence_pin: GPIO27
#      uart_port: ${device_internal_name}_uart_bus1
# ------------------------------------------------------------------------------
# Author: Pascal Parent
# Company: Home Automator (ZA)
# Web: https://www.youtube.com/@homeautomatorza
# Version: 1.2.0
# Licence: CCO 1.0 https://creativecommons.org/publicdomain/zero/1.0/
# Reference: https://esphome.io/components/sensor/ld2410.html
# ------------------------------------------------------------------------------
# WARNING:
# This code carries a "It works on my setup" disclaimer!
# Meaning that it works on my setup but it may not work on yours.
################################################################################
ld2410:
  uart_id: ${uart_port}
  id: ${device_internal_name}_ld2410_mmwave

binary_sensor:
  - platform: ld2410
    has_target:
      id: ${device_internal_name}_ld2410_presence
      name: LD2410 Presence
      disabled_by_default: true
    has_moving_target:
      id: ${device_internal_name}_has_moving_target
      name: LD2410 Moving Target
      disabled_by_default: true
    has_still_target:
      id: ${device_internal_name}_has_still_target
      name: LD2410 Still Target
      disabled_by_default: true
    ld2410_id: ${device_internal_name}_ld2410_mmwave

  - platform: gpio
    id: ${device_internal_name}_ld2410_gpio_presence
    name: LD2410 GPIO Presence
    icon: mdi:motion-sensor
    pin: ${presence_pin}
    device_class: presence

sensor:
  - platform: ld2410
    moving_distance:
      id: ${device_internal_name}_moving_distance
      name : LD2410 Moving Distance
      disabled_by_default: true
    still_distance:
      id: ${device_internal_name}_still_distance
      name: LD2410 Still Distance
      disabled_by_default: true
    moving_energy:
      id: ${device_internal_name}_move_energy
      name: LD2410 Move Energy
      disabled_by_default: true
    still_energy:
      id: ${device_internal_name}_still_energy
      name: LD2410 Still Energy
      disabled_by_default: true
    detection_distance:
      id: ${device_internal_name}_detection_distance
      name: LD2410 Detection Distance
      disabled_by_default: true
    ld2410_id: ${device_internal_name}_ld2410_mmwave

switch:
  - platform: ld2410
    bluetooth:
      id: ${device_internal_name}_bluetooth
      name: LD2410 Bluetooth
      disabled_by_default: true
    ld2410_id: ${device_internal_name}_ld2410_mmwave

button:
  - platform: ld2410
    factory_reset:
      id: ${device_internal_name}_factory_reset
      name: LD2410 Factory Reset
    restart:
      id: ${device_internal_name}_restart
      name: LD2410 Restart
      entity_category: config
    ld2410_id: ${device_internal_name}_ld2410_mmwave

text_sensor:
  - platform: ld2410
    version:
      id: ${device_internal_name}_firmware_version
      name: LD2410 Firmware Version
      disabled_by_default: true
    ld2410_id: ${device_internal_name}_ld2410_mmwave

select:
  - platform: ld2410
    distance_resolution:
      id: ${device_internal_name}_distance_resolution
      name: LD2410 Distance Resolution
      disabled_by_default: true
    ld2410_id: ${device_internal_name}_ld2410_mmwave
```



```yaml

```
