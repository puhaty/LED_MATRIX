# PROJ-Led-Board-with-REST-API
Sterownik tablicy LED z interfejsem częściowo równoległym (docelowo) z wykorzystaniem interfejsu REST API.

Projekt powstał w ramach przedmiotu *Projekt Grupowy 1/2*.

## Spis treści

- [PROJ-Led-Board-with-REST-API](#proj-led-board-with-rest-api)
  - [Spis treści](#spis-treści)
  - [Dokumentacja](#dokumentacja)
  - [Budowanie projektu](#budowanie-projektu)
    - [Instalacja bibliotek](#instalacja-bibliotek)
    - [Konfiguracja](#konfiguracja)
    - [Budowanie](#budowanie)


## Dokumentacja
- [Getting started with pico](https://datasheets.raspberrypi.com/pico/-getting-started-with-pico.pdf)
- [Connection to the internet with Pico W](https://datasheets.raspberrypi.com/picow/connecting-to-the-internet-with-pico-w.pdf)
- [Raspberry Pi Pico C/C++ SDK](https://datasheets.raspberrypi.com/pico/raspberry-pi-pico-c-sdk.pdf)

## REST API

Zdefiniowano trzy styki REST:
- `/api/text`, przyjmujący trzy parametry query
  - `t` - tekst do wyświetlenia, tylko `A-Z` `0-9` i znak `_` jako spacja, maksymalnie `254` znaków
  - `speed` -  opcjonalny, szybkość przewijania, zakres `0-255`, najlepsze efekty dla zakresu `245-254`
  - `color` - opcjonalny, liczba z zakresu `0-255` zawierająca kolory zakodowane w sposób `RRRGGGBB` - 8 bitów
- `/api/spectrum`, wyświetlający animację prezentującą spektrum kolorów
  - `speed` - prędkość animacji
- `/api/next` - zwraca dane o żądaniu w kolejce które będzie obsługiwane jako następne, postać `json`

## Budowanie projektu

Testowano na **Kali Linux**, powinno działać na każdej dystrybucji opartej na **Debianie**.

### Przygotowanie repozytorium
```
mkdir pico
cd pico
git clone https://github.com/raspberrypi/pico-sdk.git --branch master
cd pico-sdk
git submodule update --init
cd ..
git clone https://github.com/raspberrypi/pico-examples.git --branch master
git clone https://github.com/MrMroz/PROJ-Led-Board-with-REST-API.git
cd PROJ-Led-Board-with-REST-API
git submodule update --init
```

### Instalacja bibliotek
```
sudo apt update
sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential libstdc++-arm-none-eabi-newlib libusb-1.0-0-dev pkg-config gperf
```

### Konfiguracja
```
mkdir build
cd build
export PICO_SDK_PATH=../../pico-sdk
```

### Budowanie
```
cd build
cmake -DPICO_BOARD=pico_w -DWIFI_SSID="nazwa" -DWIFI_PASSWORD="haslo" ..
make -j
```
*Jeżeli nie jest wymagana łączność WIFI, to flagi -DWIFI są zbędne.*


## Program demonstracyjny

Program demonstracyjny naprzemiennie wyświetla spektrum kolorów oraz napis prezentujący wszystkie obsługiwane znaki (*A-Z 0-9*).
