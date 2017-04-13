<properties 
    pageTitle="Võrgu konfigureerimise faili virtuaalse võrgu konfigureerimine" 
    description="Eksportida ja importida võrgu konfigureerimise faili loomiseks või muutmiseks virtuaalne võrkude Azure'i haldusportaal juhiseid. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Võrgu konfigureerimise faili virtuaalse võrgu konfigureerimine

Azure haldusportaali abil või võrgu konfigureerimise faili abil saate konfigureerida virtuaalse võrgu (VNet).

## <a name="creating-and-modifying-a-network-configuration-file"></a>Loovad ja muudavad võrgu konfigureerimise faili 
Lihtsaim viis võrgu konfigureerimise faili autor on võrgusätted eksportimine olemasoleva virtuaalse võrgu konfiguratsiooni, muutke fail sisaldama sätteid, mida soovite konfigureerida oma virtuaalse võrgustikke.

Võrgu konfigureerimise faili redigeerimiseks saate lihtsalt avage fail, veenduge, et vajalikud muudatused ja salvestage fail. Saate muuta võrgu konfigureerimise faili mis tahes *XML-i* redaktoris. 

Tihedalt järgite [võrgu konfigureerimise faili skeemi sätted](https://msdn.microsoft.com/library/azure/jj157100.aspx)juhised. 

Azure'i leiab alamvõrku, mis on midagi selle nimega **Kasuta**juurutatud. Kui soovitud alamvõrgu on kasutusel, ei saa muuta. Enne muutmine, teisaldamine midagi alamvõrgu erinevate alamvõrku, mis pole muutmist juurutatud.   Vt [VM või muu alamvõrgu rolli eksemplari teisaldada](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Eksportimine ja importimine virtuaalse võrgu sätteid haldusportaali abil  
Saate importida ja eksportida võrgu konfiguratsioonisätted sisalduvate PowerShelli või selle haldusportaali abil oma võrgu konfigureerimise faili. Järgmised juhised aitavad teil haldusportaali abil impordi ja ekspordi. 

### <a name="to-export-your-network-settings"></a>Võrgusätete eksportimine
Kui ekspordite, kirjutatakse kõik sätted virtuaalse võrkude teie tellimus XML-faili. 

1. Logige **haldusportaali**.
2. Haldusportaal **võrkude** lehe allservas nuppu **ekspordi**. 
3. **Ekspordi võrgu konfigureerimise** aknas, veenduge, et olete valinud tellimus, mille soovite eksportida võrgusätted. Klõpsake paremas allnurgas märke. 
4. Kui teilt küsitakse, salvestage fail *NetworkConfig.xml* soovitud kohta.


### <a name="to-import-your-network-settings"></a>Importimiseks võrgusätted

1. Klõpsake **Haldusportaali**allosas vasakul navigeerimispaanil nuppu **Uus**.
2. Klõpsake **võrguteenuste** -> **virtuaalse võrgu** -> **konfiguratsiooni Import**.
3. Lehel **võrgu konfigureerimise faili importimine** sirvides oma võrgu konfigureerimise faili ja klõpsake noolenuppu **Järgmine** .
4. **Võrgu koostamise** lehel kuvatakse ekraanil nähtaval, millised teie võrgu konfigureerimise osad on muudetud või loodud teavet. Kui soovitud muudatused on õige, klõpsake jätkamiseks värskendada või luua virtuaalse võrgu märke. 