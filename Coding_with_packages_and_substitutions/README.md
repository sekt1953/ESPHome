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

Let's start with OMJK's "external control cabinet", here I use an OLIMEX ESP32-POE with ESP32-D0WD V3 and 4 MB Flash, and I use the I2C interface for mcp23017 to connect push buttons and GPIO to control NeoPixel Light SK6812.
The "external control cabinet" is powered via PoE, since I want to use PoE, I will therefore not use WiFi; but Ethernet (LAN).

#### The MCU Board



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
