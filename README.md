# ESPHome

## Kodning med ESPHome for modelbane folk og andre

### Hvorfor dette

* Jeg er igang med at bygge elektronik, til OMJK modelbane sikkerhedssystem, som jeg vil bygge oven på Home Assistant & ESPHome
* Indholdet her er til støtte for mine videoer, om at kode med ESPHome,  som var det Lego klodser vi arbejde med.
* Jeg håber med YouTube videoerne og denne dokumentation, at kan få flere indvolveret i udviklings processen og velige holdelse af koden

### Antagelse

* Vi har en frisk installation af Home Assistant med følgende APPS installeret
  * MariaDB
  * ESPHome Device Builder
  * Studio Code Server
  * File Editor
  * Nice to Have
    * HACS
      * [How to install HACS in Home Assistant (2025)](https://youtu.be/TL2eqsjhmTE)
    * Advanced SSH & Web Terminal
      * [Advanced SSH and Web Terminal Installation and Configuration](https://youtu.be/fw69Tf9F5DU)
* Vi har PC med en Chrome Browser
* PCsn og Home Assistant er forbundet til samme netwærk, og PCen har adgang til internettet.
* At vi mindst 1 stk ESP32 dev board, med USB kabel for programering

## Kilder

* [ESPHome Docs](https://esphome.io/components/)
  * [YAML Configuration in ESPHome](https://esphome.io/guides/yaml/)
  * [Packages](https://esphome.io/components/packages/)
  * [Substitutions](https://esphome.io/components/substitutions/)
  * [Security Best Practices](https://esphome.io/guides/security_best_practices/)
* Pascal Parent YouTube & GitHub link:
  * [homeautomatorza/esphome](https://github.com/homeautomatorza/esphome)
  * [homeautomatorza/ESPHome-Modules](https://github.com/homeautomatorza/ESPHome-Modules)
  * [Let's rework our Room Sensor Package Code!](https://youtu.be/52_ZJmTz3bs)

## Indhold

1. [Mappestruktur](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Fil_Mappestruktur.md "Klik Her")
    * [Mapper og Filer](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Fil_Mappestruktur.md#mapper-og-filer)
    * [Kort Mappe Beskrivelse](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Fil_Mappestruktur.md#kort-mappe-beskrivelse)
    * [Særlige filer - secrets.yaml & .gitignore](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Fil_Mappestruktur.md#særlige-filer)
2. [Basic Filer](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/PackegeFiles.md)
    * [boards](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/PackegeFiles.md#boards "Klik Her")
    * [common/core](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/PackegeFiles.md#common-core "Klik Her")
    * [common/networks](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/PackegeFiles.md#common-network)
3. [peripherals](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Packet_Peripherals.md)
    * [display](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Packet_Peripherals.md)
    * [motor_controller](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Packet_Peripherals.md)
    * [light_controller](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Packet_Peripherals.md)
4. [sensors](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Package_Sensors.md)
    * [distance](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Package_Sensors.md)
    * [environment](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Package_Sensors.md#environment)
    * [presence](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Package_Sensors.md#presence)
5. [ESPHome files](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/ESPHome_files.md)
    * [esphome_study_wifi_dynamic_ip.yaml](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/ESPHome_files.md#esphome_study_wifi_dynamic_ipyaml)
    * [esphome_study_wifi_static_ip.yaml](Kodning_med_ESPHome_for_modelbane_folk_og_andre/ESPHome_files.md#esphome_study_wifi_static_ipyaml)

## [Coding with packages and substitutions inspired by Pascal Parent videos](./Coding_with_packages_and_substitutions/README.md)

* Place where i will place my new ESPHome project, which is heavily inspired by the [Home Automator](https://www.youtube.com/@homeautomatorza/featured) video series ["Coding Practices, Tips and Tricks"](https://www.youtube.com/playlist?list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I)
* Before you start using this guide, watch at least these videos :
  * [home-automator - ESPHome Coding Practices Tips and Tricks](https://www.youtube.com/playlist?list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I)
* and articles:
  * [olegtarasov - packages and substitutions tutorial](https://olegtarasov.me/esphome-packages-substitutions-tutorial/)
  * [ESPHome.io - Packages](https://esphome.io/components/packages/)

