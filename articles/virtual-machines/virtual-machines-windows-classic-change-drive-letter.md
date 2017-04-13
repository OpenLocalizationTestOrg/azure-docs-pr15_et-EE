<properties
    pageTitle="D: ketas VM andmed kettale, tehke | Microsoft Azure'i"
    description="Kirjeldab, kuidas muuta tähti Windows VM, nii et saate kasutada andmete ketas D: ketas."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>D: ketas kasutamine andmete draivi Windows VM 

Kui teie rakendus peab juhtida D abil andmete talletamiseks, järgige neid juhiseid muu täht ajutine kettapuhastusriista kasutamiseks. Ärge kasutage ajutist ketas talletada andmeid, mida soovite säilitada.

Kui olete suurust või virtuaalse masina **Peata (Deallocate)** see võib põhjustada paigutuse virtuaalse masina abil uue hypervisor. Planeerimata või kavandatud hooldustööde sündmuse võib põhjustada ka selle paigutuse. Selle stsenaariumi korral saadaval ketas esitähe ümber ajutine ketas. Kui teil on rakendus, mis nõuab spetsiaalselt D: ketas, peate järgmiste juhiste abil ajutiselt soovitud pagefile.sys, lisada uusi andmeid kettal ja selle määramine täht D ja seal liikumine on pagefile.sys tagasi ajutine kettale. Kui olete lõpetanud, Azure ei võta tagasi D: kui VM viib erinevate hypervisor.

Lisateavet selle kohta, kuidas Azure'i kasutab ajutist ketas, lugege teemat [mõistmine ajutine kettale klõpsake Microsoft Azure'i Virtuaalmasinates](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Andmed kettale manustamine

Esmalt peate virtuaalse masina andmed kettale manustamiseks. 

- Portaali kasutamiseks lugege teemat [Kuidas manustada Azure'i portaalis kettal andmed](virtual-machines-windows-attach-disk-portal.md)
- Klassikaline portaali kasutamiseks lugege teemat [Kuidas lisada andmed kettale Windows virtuaalse masina](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>C-ketas ajutiselt pagefile.sys teisaldamine

1. Ühenduse, virtuaalse masina. 

2. Paremklõpsake menüü **Start** ja valige **süsteem**.

3. Valige vasakpoolses menüüs **Täpsemad**.

4. Valige jaotises **jõudluse** **sätted**.

5. Valige vahekaart **Täpsemalt** .

5. Valige jaotises **virtuaalmälu** **muutmine**.

6. Valige **C** -ketas ja seejärel klõpsake nuppu **süsteemi hallatav maht** ja seejärel klõpsake käsku **Sea**.

7. Valige ketas **D** ja klõpsake **Piipar faili** ja seejärel klõpsake käsku **Sea**.

8. Klõpsake nuppu Rakenda. Saate hoiatus, et arvuti tuleb muudatuste jõustumiseks tuleb taaskäivitada.

9. Taaskäivitage arvuti virtuaalne.




## <a name="change-the-drive-letters"></a>Tähti muutmine 

1. Kui VM taaskäivitamist, logige uuesti VM.

2. Klõpsake menüü **Start** ja tippige **diskmgmt.msc** ja vajutage sisestusklahvi. Hakkavad ketta haldus.

3. **D**, ajutised salvestusruumi ketas, paremklõpsake ja valige **Muuda draivi nime ja teed**.

4. Täht, klõpsake jaotises Valige ketas **G** ja seejärel klõpsake nuppu **OK**. 

5. Paremklõpsake andmed kettale ja valige **Muuda Drive Letter ja teed**.

6. Täht, klõpsake jaotises Valige ketas **D** ja seejärel klõpsake nuppu **OK**. 

7. **G**, ajutised salvestusruumi ketas, paremklõpsake ja valige **Muuda draivi nime ja teed**.

8. Täht, klõpsake jaotises Valige ketas **E** ja seejärel klõpsake nuppu **OK**. 

> [AZURE.NOTE] Kui teie VM muud ketast või draivid, sama meetodi abil määrata tähti ketast ja draivid. Soovite ketta konfiguratsiooni olema.  
>- C: ketas OS  
>- D: andmed kettale  
>- E: ajutist



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Liikumine tagasi pagefile.sys ketas ajutine salvestusruum 

1. Paremklõpsake menüü **Start** ja valige **süsteem**

2. Valige vasakpoolses menüüs **Täpsemad**.

3. Valige jaotises **jõudluse** **sätted**.

4. Valige vahekaart **Täpsemalt** .

5. Valige jaotises **virtuaalmälu** **muutmine**.

6. Valige OS **C** -ketas ja klõpsake **Piipar faili** ja seejärel klõpsake käsku **Sea**.

7. Valige ajutine salvestusruumi ketas **E** ja klõpsake **süsteemi hallatav maht** ja seejärel klõpsake käsku **Sea**.

8. Klõpsake nuppu **Rakenda**. Saate hoiatus, et arvuti tuleb muudatuste jõustumiseks tuleb taaskäivitada.

9. Taaskäivitage arvuti virtuaalne.




## <a name="next-steps"></a>Järgmised sammud
- Lisades [täiendavad andmed kettale](virtual-machines-windows-attach-disk-portal.md)saate suurendada arvuti virtuaalne saadaval salvestusruumi.



