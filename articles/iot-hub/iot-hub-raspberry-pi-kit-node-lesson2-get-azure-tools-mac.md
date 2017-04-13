<properties
 pageTitle="Saada Azure'i tööriistad (macOS 10.10) | Microsoft Azure'i"
 description="Installige macOS Python ja Azure käsurea liides (Azure'i CLI)."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="21-get-azure-tools-macos-1010"></a>2.1 saada Azure'i tööriistad (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1, mida te teete

Installimine on Azure käsurea liides (Azure'i CLI). Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2, mida saate

- Kuidas installida Azure CLI.
- Kuidas lisada Azure CLI asjade alagrupis.

## <a name="213-what-you-need"></a>2.1.3 vajalik

- Interneti-ühendus Mac
- Azure'i aktiivne tellimus. Kui teil pole kontot, saate luua ka [tasuta konto](https://azure.microsoft.com/free/) vaid paar minutit.

## <a name="214-install-python"></a>2.1.4 installida Python

Kuigi macOS väljub Python 2,7 välja, on soovitatav Python Homebrew kaudu installida. Vaadake [Installimist Python macOS kohta](http://docs.python-guide.org/en/latest/starting/install/osx/).

Installige Python ja pip, käivitage järgmine käsk:

```bash
brew install python
```

## <a name="215-install-the-azure-cli"></a>2.1.5 installida Azure CLI

Azure'i CLI pakub mitmeplatvormilist käsurea kogemus Azure, mistõttu saate töötada otse käsureal ettevalmistamine ja ressursside haldamine. 

Installige uusim Azure CLI, toimige järgmiselt.

1. Käivitage järgmised käsud Terminal aknas. See võib võtta installida Azure CLI viis minutit.

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Kontrollige installi, käivitage järgmine käsk:

    ```bash
    az iot -h
    ```
  
Peaksite nägema järgmine väljund kui installi õnnestumise.

![AZ asjade Interneti -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="215-summary"></a>2.1.5 Kokkuvõte

Azure'i CLI installitud. Jätkake oma Azure'i asjade jaoturi ja seadme identiteedi abil Azure'i CLI loomiseks järgmise jaotise juurde.

## <a name="next-steps"></a>Järgmised sammud

[2.2 luua oma asjade jaoturi ja registreerida oma Vaarika Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
