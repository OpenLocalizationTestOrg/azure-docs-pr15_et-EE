<properties
    pageTitle="Lõpp-punktid klassikaline Linux VM häälestamine | Microsoft Azure'i"
    description="Saate teada, kuidas häälestada lõpp-punktide jaoks Linux VM Azure klassikaline portaalis luba suhtlus Linux virtuaalse masina Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Kuidas häälestada Linux klassikaline virtual arvutisse Azure lõpp-punktid

Kõik Linux virtuaalmasinates Azure klassikaline juurutamise mudeli abil loodud saate automaatselt üle privaatvõrgu kanali koos teiste sama pilveteenuses või virtuaalse võrgu virtuaalmasinates suhelda. Interneti-ühendus või mõne muu virtuaalse võrguga ühendatud arvutites nõuda virtuaalse masina sissetuleva võrgu-liikluse suunamiseks lõpp-punktid. See artikkel kehtib ka [Windowsi virtuaalmasinates](virtual-machines-windows-classic-setup-endpoints.md)jaoks saadaval.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]**Ressursihaldur** juurutamise mudeli, lõpp-punktid on konfigureeritud, kasutades **Võrgu turberühmad (NSGs)**. Lisateavet leiate artiklist [avamine pordid ja lõpp-punktid] (virtuaalne-masinad-linux-nsg-quickstart.md).

Azure'i klassikaline portaalis Linux virtuaalse masina loomisel lõpp-punkti jaoks Secure Shell (SSH) tavaliselt luuakse automaatselt. Saate konfigureerida täiendavaid lõpp-punktid virtuaalse masina loomisel või hiljem vastavalt vajadusele.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Järgmised sammud

* [Azure'i käsurea kasutajaliidese](../virtual-machines-command-line-tools.md)abil saate luua ka VM endpoint. **Azure'i vm lõpp-punkti loomiseks** käsu käivitamine.

* Kui olete loonud virtuaalse masina ressursihaldur juurutamise mudeli, saate kasutada Azure CLI ressursihaldur režiimis, et [luua võrgu turberühmad](../virtual-network/virtual-networks-create-nsg-arm-cli.md) juhtelement liiklust VM.