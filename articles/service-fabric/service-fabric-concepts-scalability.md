<properties
   pageTitle="Teenuse struktuuri teenuste skaleeritavus | Microsoft Azure'i"
   description="Kirjeldab, kuidas mastaapimiseks teenuse struktuuri teenused"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="scaling-service-fabric-applications"></a>Skaleerimise teenuse struktuuri rakendused
Azure teenuse struktuuri lihtne koostada sõlme klaster scalable rakenduste koormust tasakaalustavad teenused, sektsioonid ning koopiad. See võimaldab suurima ressursi kasutamine.

Suure ulatuse teenuse struktuuri rakendusi saate teha kahel viisil:

1. Skaleerimist tasemel partition

2. Skaleerimist tasemel teenuse nimi

## <a name="scaling-at-the-partition-level"></a>Skaleerimist tasemel partition
Teenuse struktuuri toetab eraldamine üksikute teenuse mitu väiksema sektsiooni sisse. [Eraldamine ülevaade](service-fabric-concepts-partitioning.md) annab eraldamine skeemid, mis on toetatud tüüpi teavet. Iga sektsiooni koopiad on laiali sõlmed klaster. Kaaluge võimalust teenus, mis kasutab jäi eraldamine kava madal klahvi 0, kõrge klahvi 99 ja neli sektsiooni. Kolm sõlme klaster, võib teenuse nähtud neli koopiad, ressursside iga sõlme ühiskasutusse, nagu järgmisel joonisel:

![Kolm sõlmed paigutuse sektsioon](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Arvu sõlmed võimaldab kasutada uusi sõlmi ressursse, kui teisaldate mõne kujundusmuudatusi tühja sõlmed teenuse struktuuri. Suurendades sõlmed nelja arvu, teenus on nüüd töötab iga sõlme (erinevad sektsioonid), mis võimaldab ressursikasutuse ja parema jõudluse kolme koopiad.

![Sektsiooni paigutuse sõlmed neli](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>Skaleerimist tasemel teenuse nimi
Teenuse eksemplari on teatud eksemplari rakenduse nimi ja tüüp teenuse nimi (vt teemat [teenuse struktuuri rakenduse elutsükli](service-fabric-application-lifecycle.md)). Teenuse loomise ajal teie määratud sektsioon kava kasutatava (vt teemat [teenuse struktuuri eraldamine teenused](service-fabric-concepts-partitioning.md)).

Esimese taseme skaleerimist on teenuse nime järgi. Saate luua uue eksemplari teenus, erineva nimega oma vanemate eksemplaride muutuvad hõivatud eraldamine. See võimaldab uue teenuse tarbijad kasutada vähem hõivatud teenuse eksemplari, mitte kuvab rohkem teatisi need.

Üheks võimaluseks suurendamiseks võimsus, samuti suurenevad või kahanevas partition loendab on uue sektsiooni kava luua uue eksemplari. See lisab keerukuse, kuigi nõudvate kliendid vaja teada, millal ja kuidas kasutada teisiti teenuse.

### <a name="example-scenario-embedded-dates"></a>Näiteks sel juhul: manustatud kuupäevad
Üks võimalik stsenaarium oleks kasutada kuupäevateavet osana teenuse nimi. Näiteks võite kasutada teenuse eksemplari kõik kliendid, kes on liitunud 2013 teatud nime ja teine nimi klientidele, kes on liitunud 2014. Selle nime kava võimaldab programmiliselt suurendada sõltuvalt kuupäeva nimed (nagu 2014 lähenemisel, 2014 eksemplari saab luua nõudmisel).

Seda moodust põhineb siiski konkreetse rakenduse nime teave, mis ei hõlma teenuse struktuuri teadmisi kasutavate klientide.

- *Kasutades nimereeglistik*: 2013, kui rakenduse läheb reaalajas, saate luua teenust nimega struktuuri: / rakendus/service2013. Lähedal kvartalis 2013, saate luua muu teenuse, mida nimetatakse struktuuri: / rakendus/service2014. Mõlemad teenused on sama tüüpi teenuse. Seda moodust oma kliendi tuleb tööle loogika ehitada aasta alusel asjakohane nimi.

- *Otsingu teenuse kasutamine*: teise muster on sekundaarne otsingu teenuse, mis annab teenuse jaoks soovitud klahvi nimi. Otsingu teenuse seejärel loomist teenuse uued eksemplarid. Otsingu teenusesse ei säili rakenduse andmeid, vaid andmeid teenuste nimed, mis loob. Seega aasta-põhiste Ülaltoodud näites kliendi oleks esmalt välja selgitada aasta andmete töötlemise teenuse nime pöörduge otsing ja seejärel kasutage toimingut tegelik selle teenuse nimi. Tulemus esimese otsingu saab kopeerida.

## <a name="next-steps"></a>Järgmised sammud

Teenuse struktuuri põhimõtet kohta lisateabe saamiseks vaadake järgmist:

- [Teenuse struktuuri teenuste kättesaadavus](service-fabric-availability-services.md)

- [Eraldamine teenuse struktuuri teenused](service-fabric-concepts-partitioning.md)

- [Loomine ja haldamine olek](service-fabric-concepts-state.md)
