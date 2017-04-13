<properties
    pageTitle="Mõne Azure Storage katkestuste korral | Microsoft Azure'i"
    description="Mida teha ka Azure Storage katkestuste korral"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Mida teha, kui esineb ka Azure Storage katkestuste

Microsofti töötame selle nimel meie teenuste oleks alati saadaval. Mõnikord sunnib Lisaks meie juhtelemendi mõju viisil, mis põhjustavad plaanimata teenus katkestuste ühes või mitmes piirkonnas. Selleks et saaksite toime nende harvaesinevate juhud, pakume Azure Storage teenuste üksikasjalik järgmised juhised.

## <a name="how-to-prepare"></a>Kuidas koostada 

On oluline, et iga kliendi ette valmistada oma katastroofi taastamise kava. Peegeldav taastada salvestusruumi katkestuste tavaliselt hõlmab nii toimingute personali ja automatiseeritud toimingutest, et uuesti toimiv rakendused. Palun vaadake oma katastroofi taastamise kava koostamiseks Azure dokumentatsioonis:

-   [Katastroofiabi ja Azure rakenduste kõrge kättesaadavus](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Azure'i paindlikkust tehnilise abi saada?](../resiliency/resiliency-technical-guidance.md)

-   [Azure'i saidi taastamine teenus](https://azure.microsoft.com/services/site-recovery/)

-   [Azure'i salvestusruumi dispersioonanalüüs](storage-redundancy.md)

-   [Azure'i varundus teenus](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Kuidas tuvastamiseks 

Soovitatav viis määratlemiseks Azure teenuse olek on tellimine [Azure'i teenuste seisundi armatuurlaud](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Mida teha, kui esineb salvestusruumi katkestuste

Kui üks või mitu salvestusruumi teenused on ajutiselt saadaval ühes või mitmes piirkonnas, on kaks võimalust teile silmas pidada. Kui soovite kohe juurde oma andmetega, kaaluge variant 2.

### <a name="option-1-wait-for-recovery"></a>Variant 1: Oodake, kuni taastamine

Sel juhul enam teie pole vaja midagi. Töötame hoolega taastamine Azure'i teenuse kättesaadavus. Saate jälgida teenuse olek [Azure'i teenuste seisundi armatuurlaud](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Variant 2: Andmete kopeerimine teisese

Kui valisite [lugemisõigus – geograafilise liigne salvestusruumi (RA GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (soovitatav) oma salvestusruumi kontod, peate lugemisõigus andmete teisese piirkonnast. Tööriistu saate kasutada näiteks [AzCopy](storage-use-azcopy.md), [Azure PowerShelli](storage-powershell-guide-full.md)ja [Azure andmete liikumine teegi](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) teisene piirkond andmete kopeerimine teise salvestusruumi konto unimpacted piirkonnas ja siis osutage sellele kontole salvestusruumi rakenduste nii lugemine ja kirjutamine kättesaadavus.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Mida oodata, kui esineb Tõrkesiirde salvestusruum

Kui valisite [geograafilise liigne (GRS)](storage-redundancy.md#geo-redundant-storage) või [lugemisõigus – geograafilise liigne salvestusruumi (RA GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (soovitatav), Azure Storage hoiab teie andmete püsival mõlema piirkonna (esmaseid ja teiseseid). Mõlemad piirkonnad Azure Storage pidevalt säilitab andmete mitme koopiad.

Kui piirkondliku õnnetusena mõjutab oma peamine piirkond, saame esmalt proovige taastada selle piirkonna teenuse. Sõltub laad ja katastroofi mõju mõned harva me ei saa taastada esmane piirkond. Sel hetkel me täidab geo Tõrkesiirde. Rist-piirkonna andmete kopeerimine on asünkroonne toiming, mis võib olla viivitus, nii et see on võimalik, et muudatused, mida pole veel teisene alale kopeeritud võib olla kaotsi. Saate teha päringuid ["viimase sünkroonimise aeg" salvestusruumi konto](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) üksikasjade saamiseks kopeerimise olek.

Paar punktide salvestusruumi geo-Tõrkesiirde kogemusi:

-   Salvestusruumi geo-Tõrkesiirde käivitatakse ainult Azure Storage meeskond – on kliendi pole vaja midagi.

-   Olemasoleva salvestusruumi teenuse lõpp-punktide plekid, tabelid, järjekorrad ja failide jäävad samaks pärast Tõrkesiirde; DNS-i kirje on vaja värskendada üleminek esmane piirkond teisene piirkond.

-   Enne ja geo-Tõrkesiirde ajal, ei pea te kirjutamisõigused konto salvestusruumi katastroofi mõju tõttu, kuid saate siiski lugeda teisese kui salvestusruumi konto on konfigureeritud RA-GRS.

-   Kui geo-Tõrkesiirde on lõpule viidud ja DNS-i muudatused levitatud, jätkatakse oma lugemis- ja salvestusruumi kontole. Saate teha päringuid ["Geo Tõrkesiirde viimast" konto salvestusruumi](https://msdn.microsoft.com/library/azure/ee460802.aspx) rohkem üksikasju.

-   Pärast selle Tõrkesiirde salvestusruumi konto on täielikult toimiv, kuid "halvenenud" olek, nagu see on tegelikult majutatud autonoomse piirkonnas, ei ole võimalik geo-dispersioonanalüüs. Selle riski vähendamiseks me taastada algse esmane piirkond ja tehke geo-failback algse oleku taastamiseks. Kui algse esmane regioon on parandamatu, eraldab me teise teisene piirkond.
Taristu Azure Storage geo kopeerimise kohta leiate Lisateavet vaadake artiklis salvestusruumi meeskonna ajaveebist [koondamise suvandid ja RA-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)kohta.

##<a name="best-practices-for-protecting-your-data"></a>Andmete kaitsmine head tavad

On mõni soovitatud lähenemisviisi regulaarselt salvestusruumi andmete varundamine.

-   VM ketast – kasutada [Azure varukoopia teenuse](https://azure.microsoft.com/services/backup/) VM ketast, mis kasutavad teie Azure'i virtuaalmasinates varundada.

-   Blokeeri klimbid – on [hetktõmmise](https://msdn.microsoft.com/library/azure/hh488361.aspx) iga bloobimälu blokeerimine või kopeerimine teise salvestusruumi plekid konto mõne muu piirkonna [AzCopy](storage-use-azcopy.md), [Azure PowerShelli](storage-powershell-guide-full.md)või [Azure'i andmed liikumine teegi](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)loomine.

-   Tabelite – kasutage [AzCopy](storage-use-azcopy.md) eksportida tabeli andmed salvestusruumi teisele kontole teises regioonis.

-   Failide – [AzCopy](storage-use-azcopy.md) või [Azure PowerShelli](storage-powershell-guide-full.md) kopeerimiseks kasutada failide salvestusruumi teisele kontole teises regioonis.
