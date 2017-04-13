<properties
   pageTitle="Lubamine või keelamine Azure'i VM jälgimine"
   description="Kirjeldab, kuidas lubada või keelata Azure'i VM jälgimine"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>Lubada või keelata Azure'i VM jälgimine

Selles jaotises kirjeldatakse, kuidas lubada või keelata, jälgige töötavate Azure'i virtuaalmasinates. Jälgimine on vaikimisi lubatud Azure virtuaalse arvutites kui juurutada [Azure portaali](https://portal.azure.com) ja jälgimisega seotud diagrammid on esitatud vaikimisi 1-minutilise punkt. Saate lubada või keelata jälgimise kaudu portaali või Azure käsurea liides Mac, Linux ja Windows (Azure'i CLI). 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Lubada / keelata jälgimise Azure portaali kaudu
 
Saate lubada Azure VM, mis pakub andmete kohta oma eksemplari 1-minutilise perioodide jälgimine. (salvestusruumi muudatused rakenduvad). Diagnostika üksikasjalikud andmed on seejärel VM portaali graafikud või API kaudu saadaval. Azure'i portaalis jälgimise vaikimisi, kuid saate selle välja lülitada, nagu allpool kirjeldatud. Saate jälgida VM töötab või on peatatud olekus ajal lubada.

- Avage Azure portaali aadressil ** [https://portal.azure.com](https://portal.azure.com)**

- Klõpsake vasakpoolsel navigeerimisalal virtuaalmasinates.

- Valige loendis virtuaalmasinates, töötab või on peatatud eksemplari. Avatakse virtuaalse masina blad.

- Klõpsake nuppu "Kõik sätted".

- Klõpsake nuppu "Diagnostika".

- Olekuks sees või väljas. Võite valida ka rakenduses selle tera järelevalve üksikasjad, mida soovite lubada arvuti virtuaalne tase.

[Azure.Note] Üleminek diagnostika on vaikimisi, kui loote uue virtuaalse masina

![Lubada / keelata jälgimise Azure portaali kaudu.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Lubada / keelata Azure CLI jälgimine
 
Lubamiseks on Azure VM jälgimine.

- Looge fail nimega näiteks PrivateConfig.json sisu on järgmine.
        {"storageAccountName": "salvestusruum konto andmete", "storageAccountKey": "konto võti"}
- Käivitage järgmine käsk Azure'i CLI.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Saate muuta versioonilt 2.0 uuemale versioonile, kui need on saadaval. 

Täpsemat teavet konfigureerimise mõõdikute ja näidised, külastage dokumendi - **[Abil Linux diagnostika laiend kuvari Linux VM jõudlus ja Diagnostikaandmete](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

