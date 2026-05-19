# ESPHome_Directory_Structure

## My default layout for ESPHome in Home Assistant

* CONFIG
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