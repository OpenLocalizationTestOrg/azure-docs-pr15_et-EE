<properties
    pageTitle="Vertikaalselt mastaapimiseks Azure'i virtuaalmasinates Azure automatiseerimine | Microsoft Azure'i"
    description="Kuidas vertikaalselt mastaapimiseks Windowsi virtuaalse masina vastuseks jälgimine teatiste Azure automatiseerimine"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machines-with-azure-automation"></a>Vertikaalselt mastaapimiseks Azure'i virtuaalmasinates Azure automatiseerimine

Vertikaalne skaleerimist on suurendada või vähendada ressursside masina vastuseks töökoormus. Azure'i saate seda teha, muutes virtuaalse masina suurus. See aitab järgmistel juhtudel.

- Kui sageli ei kasutata virtuaalse masina, saate muuta selle kuu kulude vähendamise väiksem allapoole
- Kui virtuaalse masina on näha tippväärtus laadi, selle suurust saab muuta oma suurendamiseks suuremaks

Juhised selle saavutamiseks liigendus on all

1. Azure'i automaatika juurde pääseda oma Virtuaalmasinates häälestamine
2. Azure'i automaatika vertikaalne skaala tegevusraamatud importida oma tellimuse
3. Teie käitusjuhendi on webhook lisamine
4. Arvuti virtuaalne teatise lisamine

> [AZURE.NOTE] Suuruse esimese virtuaalse masina suurused, saate selle ülespoole, võib olla piiratud ainult saadavuse klaster suurused praeguse virtuaalse masina on juurutatud. Avaldatud automaatika tegevusraamatud, mis kasutab sellest artiklist eest sel juhul ja ainult mastaapimiseks sees on allpool paari VM suurus. See tähendab, et Standard_D1v2 virtuaalse masina mitte ootamatult Standard_G5 ülespoole või Basic_A0 neid.

>| VM mahud skaleerimise sidumiseks |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Azure'i automaatika juurde pääseda oma Virtuaalmasinates häälestamine

Kõigepealt peate tegema on Azure'i automaatika tegevusraamatud, kasutatud skaala virtuaalse masina majutavad konto loomiseks. Hiljuti kasutusele automatiseerimine teenuse "Run kontona" funktsioon, mis muudab teenuse põhilise üles säte töötab automaatselt selle tegevusraamatud, kasutaja nimel on väga lihtne. Lugege selle artikli alla kohta:

* [Autentida tegevusraamatud Azure'i käivitada nimega kontoga](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Azure'i automaatika vertikaalne skaala tegevusraamatud importida oma tellimuse

Tegevusraamatud, mida on vaja vertikaalselt skaleerimist Virtual arvuti on juba avaldatud Azure automatiseerimine Käitusjuhendi Galerii. Peate tellimuse importida. Saate teada, kuidas järgmises artiklis tegevusraamatud importimise kohta.

* [Azure'i automaatika Käitusjuhendi ja mooduli galeriid](../automation/automation-runbook-gallery.md)

Alloleval pildil on näidatud tegevusraamatud, mida soovite importida

![Tegevusraamatud importimine](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Teie käitusjuhendi on webhook lisamine

Kui olete importinud tegevusraamatud, tuleb teil lisada mõne webhook käitusjuhendi nii, et seda saab käivitada teatise kaudu virtuaalse masina järgi. Üksikasjade lisamine webhook loomise oma Käitusjuhendi saate lugeda siin

* [Azure'i automaatika webhooks](../automation/automation-webhooks.md)

Veenduge, et saate kopeerida soovitud webhook enne sulgemist webhook dialoogiboksi, peate selle järgmises jaotises.

## <a name="add-an-alert-to-your-virtual-machine"></a>Arvuti virtuaalne teatise lisamine

1. Valige virtuaalse masina sätted
2. Valige "Teade reeglid"
3. Valige "Lisa teade"
4. Valige mõõdiku tule teatise kohta
5. Valige tingimus, mis, kui täidetud on põhjustada tule teatise
6. Valige tingimus piirmäära samm 5. peavad olema täidetud
7. Valige tingimus ja lävi üle, mis kontrollib jälgimisega seotud teenuse perioodi juhiseid 5 ja 6
8. Kleepige webhook, kopeeritud eelmisest jaotisest.

![Teatise lisamine virtuaalse masina 1](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Teatise lisamine virtuaalse masina 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)
