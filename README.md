# ESPHome

## Kodning med ESPHome for modelbane folk og andre

### Hvorfor dette

* Jeg er igang med at bygge elektronik, til OMJK modelbane sikkerhedssystem, som jeg vil bygge oven på Home Assistant & ESPHome
* Indholdet her er til støtte for mine videoer, om at kode med ESPHome,  som var det Lego klodser vi arbejde med.
* Jeg håber med YouTube videoerne og denne dokumentation, at kan få flere indvolveret i udviklings processen og velige holdelse af koden

### Antagelse

* Vi har en frisk installation af Home Assistant med følgende APPS installeret
  * ESPHome Device Builder
  * Studio Code Server
  * Advanced SSH & Web Terminal
  * File Editor
* Vi har PC med en Chrome Browser
* PCsn og Home Assistant er forbundet til samme netwærk, og PCen har adgang til internettet.
* At vi mindst 1 stk ESP32 dev board, med USB kabel for programering

## Kilder

* [ESPHome Docs](https://esphome.io/components/)
  * [Packages](https://esphome.io/components/packages/)
  * [Security Best Practices](https://esphome.io/guides/security_best_practices/)
* Pascal Parent
  * [homeautomatorza/esphome](https://github.com/homeautomatorza/esphome)
  * [homeautomatorza/ESPHome-Modules](https://github.com/homeautomatorza/ESPHome-Modules)
  * [Let's rework our Room Sensor Package Code!](https://youtu.be/52_ZJmTz3bs)

## Indhold

1. [Mappestruktur](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/Fil_Mappestruktur.md "Klik Her")
2. [Basic Filer]( "Klik Her")
    1. [boards](./Kodning_med_ESPHome_for_modelbane_folk_og_andre/boards.md "Klik Her")
    2. [common/core]()
    3. [common/networks]()
3. [secrets & .gitignore]()  




## [Coding with packages and substitutions inspired by Pascal Parent videos](./Coding_with_packages_and_substitutions/README.md)

* Place where i will place my new ESPHome project, which is heavily inspired by the [Home Automator](https://www.youtube.com/@homeautomatorza/featured) video series ["Coding Practices, Tips and Tricks"](https://www.youtube.com/playlist?list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I)
* Before you start using this guide, watch at least these videos :
  * [home-automator - ESPHome Coding Practices Tips and Tricks](https://www.youtube.com/playlist?list=PLJ3MNJX_MOUnMWzUNDatN3LWAN8l99v5I)
* and articles:
  * [olegtarasov - packages and substitutions tutorial](https://olegtarasov.me/esphome-packages-substitutions-tutorial/)
  * [ESPHome.io - Packages](https://esphome.io/components/packages/)

