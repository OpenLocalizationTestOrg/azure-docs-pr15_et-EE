<properties
    pageTitle="Tellimuse ja kontode juhised | Microsoft Azure'i"
    description="Teavet ja rakendamist suuniseid tellimused ja Azure kontod."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="subscription-and-accounts-guidelines"></a>Tellimuse ja kontode juhised

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

See artikkel keskendub mõistmist, kuidas lähenemine tellimust ja konto juhtimise keskkonna ja kasutajale base kasvab.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Rakendamise juhised tellimused ja kontod

Otsuseid:

- Mida kogum tellimused ja kontod tehke peate majutada oma IT töökoormus või taristu?
- Kuidas murda vastavalt ettevõtte hierarhia?

Ülesanded:

- Kui soovite hallata tellimuse tase, määratlege oma loogilise ettevõtte hierarhia.
- Loogika hierarhiat vastavaks määratlevad nõutav kontod ja tellimuste iga konto all.
- Looge tellimused ja kontode abil oma nimereeglistikku.


## <a name="subscriptions-and-accounts"></a>Tellimused ja kontod

Azure'i töötamiseks peate ühe või mitme Azure'i tellimused. Ressursside nagu virtuaalmasinates (VMs) või virtuaalse võrgustik on olemas nende tellimustest.

- Ettevõtte kliendid on tavaliselt ettevõtte registreerimine, on kõrgeim ressurss hierarhia, mis on seotud ühe või mitme konto.
- Tarbijad ja kliendid on liitumise ilma, kõrgeim ressurss on konto.
- Tellimused on seotud kontod ja võib olla üks või mitu tellimust konto kohta. Azure'i kirjete arveldusteave tellimuse tasemel.

Tõttu piirata kahe hierarhia taset konto/tellimuse seoste kohta, on oluline joondamiseks nimetamistava kontod ja tellimuste arvelduse vajadustele. Näiteks kui globaalne ettevõte kasutab Azure, nad võivad valida ühe konto müüginäitajaid piirkonna kohta ja on tellimuste hallatav piirkond taset:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Võite kasutada näiteks järgmine struktuur:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Kui piirkonnas otsustab on rohkem kui üks tellimus, mis on seotud konkreetse rühma, lisada nimereeglistik võimalus kodeerida lisaandmed konto või tellimuse nime. See asutus lubab masseerimise arvete andmeid ajal arvelduse aruannete uuele tasemele hierarhia loomiseks:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Organisatsiooni näeb välja järgmine:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Pakume Enterprise'i lepinguga üksikasjalik arveldamine allalaaditavast ühe konto või kõigi kontode kaudu.


## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 