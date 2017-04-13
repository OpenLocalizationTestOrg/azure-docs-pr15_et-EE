<properties
    pageTitle="Saate luua mitme virtuaalmasinates | Microsoft Azure'i"
    description="Mitme virtuaalmasinates loomise Windows suvandid"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Saate luua mitme Azure'i virtuaalmasinates

On palju stsenaariume, kus peate looma suure hulga sarnaseid virtuaalmasinates (VMs). Mõned näited suure jõudlusega arvutite (HPC), suurte Andmeanalüüs, scalable ja sageli kodakondsuseta Kesk-astme või kirjutamata servers (nt Balanceri) ja jaotatud andmebaasid.

Selles artiklis käsitletakse loomine mitme VMs Azure saadaolevad suvandid. Need suvandid kaugemale lihtne juhul, kui loote käsitsi VMs sarja. Palju VMs loomiseks protsessid, mida kasutate tavaliselt ei skaala ka siis, kui teil on vaja luua rohkem kui üksikud VMs.

Üks võimalus luua palju sarnaseid VMs on kasutada Azure ressursihaldur ehitada _Ressursi tsüklid_.

## <a name="resource-loops"></a>Ressursi silmuseid

Ressursi silmuseid on süntaktilistest kiirtähisena Azure ressursihaldur malle. Ressursi silmuseid saab luua sarnaselt konfigureeritud ressursid esitatavaks. Saate luua salvestusruumi kontod, võrgu liidesed või virtuaalmasinates ressursi silmuseid. Ressursi silmuseid kohta lisateabe saamiseks vaadake [kättesaadavus VMs loomine määrab ressursside silmuseid abil](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Probleemid skaala

Kuigi ressursi silmuseid lihtsam rajamise pilvetaristu, tasandil ja kompaktsem Mallid, jäävad probleemide lahendamisel. Kui kasutate ressursside esitatavaks 100 virtuaalmasinates loomiseks, peate oleksid võrgukontrolleritele kasutajaliidese (NICs) koos vastavate VMs ja salvestusruumi kontod. Kuna VMs arv tõenäoliselt erineda arvu salvestusruumi kontod, siis on teil tegeleda ka muu ressursi tsükkel suurused. Need on lahendatav probleeme, kuid keerukus suureneb märkimisväärselt skaala.

Mõne muu ülesanne juhul, kui peate taristu, mis elastically skaala. Näiteks võite soovida autoscale taristu, automaatselt suureneb või väheneb VMs arv vastuseks töökoormus. VMs ei paku on integreeritud süsteem muuta number (skaala läbi ja skaala). Kui muudate, eemaldades VMs, on keeruline, veendudes, et VMs on tasakaalustatud värskendamise ja viga domeenides kättesaadavuse tagamiseks.

Lõpuks ressursi tsükkel kasutamisel kõnede ressursid loomiseks avage aluseks oleva struktuuri. Mitme kõne loomisel sarnased ressursid on Azure peidetud võimalus täiustada selle kujundus ja optimeerida juurutamise töökindlust ja jõudlust. See on, kus _virtuaalse masina skaala määrab_ tulevad.

## <a name="virtual-machine-scale-sets"></a>Virtuaalse masina skaala komplektid

Virtuaalse masina skaala terminikomplektid on Azure arvutada ressursi juurutada ja hallata identse VMs kogumist. Kõik VMs koos konfigureeritud samas, VM skaala terminikomplektid on lihtne mastaapimiseks sisse ja välja skaala. Te lihtsalt muuta VMs arvu määramine. Samuti saate konfigureerida VM skaala komplekti autoscale põhjal töökoormus nõudmistele.

Rakendusi, mis tuleb ulatuse ja Arvuta ressursse, skaala toimingud on peidetult tasakaalustatud viga ja Värskenda domeenides.

Asemel tõestusmeetodid mitme ressursse, nt NICs ja VMs, on VM skaala kogum võrgu, salvestusruumi, virtuaalse masina ja laiend atribuudid, mida saate konfigureerida ühes keskses kohas.

Vaadake lühitutvustust VM skaala komplektid, [virtuaalse masina skaala määrab toote lehele](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Üksikasjalikumat teavet, minge [virtuaalmasinates skaala määrab dokumendid](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).
