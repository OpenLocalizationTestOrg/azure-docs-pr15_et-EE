<properties
    pageTitle="Azure'i SQL-andmebaasi võrdlusalus ülevaade"
    description="Selles teemas kirjeldatakse kasutatud Azure'i SQL-andmebaasi toimimist SQL Azure'i andmebaasi võrdlusalus."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Azure'i SQL-andmebaasi võrdlusalus ülevaade

## <a name="overview"></a>Ülevaade
Microsoft Azure'i SQL-andmebaasi pakub kolme [teenuse astme](sql-database-service-tiers.md) jõudluse mitmel tasemel. Iga taseme pakub ressursid, või "power" mõeldud järjest suurem läbilaskevõime suurenevad kogum.

Oluline on võimalik määrata, kuidas järjest iga taseme power tõlgib suurem andmebaasi jõudlust. Tehke seda Microsoft töötanud Azure SQL-i andmebaasi võrdlusalus (ASDB). Võrdlusalus kasutab kombinatsiooni peamiste toimingute leitud kõik OLTP töökoormus. Mõõta läbilaskevõime, saavutada andmebaaside töötab iga taseme jaoks.

Ressursside ja iga teenuse taseme- ja jõudluse taseme power väljendatud [Andmebaasi tehingu üksused (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs võimalda suhteline võimsus põhineva segatud mõõt CPU, mälu, jõudluse taseme kirjeldamiseks ja lugeda ja kirjutada, määr pakutud iga taseme. Kahekordne DTU reiting andmebaasi võrdub kahekordne andmebaasi power. Võrdlusalus võimaldab mõju andmebaasi jõudlust suurenevad power pakutud iga taseme tegeliku andmebaasi toimingud, kasutades ajal skaleerimist andmebaasi mahtu, kasutajate ja tehingu hinnad proportsionaalselt antud andmebaasi vahendite arv.

Väljendada lihtsa teenuse kiht läbilaskevõime, kasutades tehingud iga tunni, Standard teenuse taseme, kasutades tehingud minutis ja Premium teenuse kiht tehingute sekundis, see on hõlpsam kiiresti seostamiseks jõudlus võimalikud iga teenuse tasandi rakenduse nõuetele.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Arvega tõeline andmebaasi jõudluse võrdlusalus tulemused
On oluline, et aru saada, et ASDB, kõik kriteeriumid, nagu on tüüpilised ja soovituslike ainult. Tehingu hinnad, mis on võrdlusalus rakendusega saavutada ei ole samad, mis on võimalik muude rakendustega. Võrdlusalus hõlmab eri tüüpi käivitada sisaldava vahemiku tabelid ja andmetüüpide skeemi suhtes tehingu kogum. Kuigi võrdlusalus kasutab sama põhitoimingutega ühised kõik OLTP töökoormus, ei kajasta mis tahes teatud liiki andmebaasist või rakendusest. Võrdlusalus eesmärk on mõistlik andmebaasi, mis võiks oodata kui skaleerimist üles või alla vahel jõudluse suhteline jõudlust annab. Tegelikult on erineva suurusega ja keerukuse andmebaaside, ilmneda erinevate segab töökoormus, ja vastab erinevatel viisidel. Näiteks rakenduse IO mahukat tabas IO lävede varem või Protsessori jaoks mahukat rakenduse tabas CPU piirangud varem. Ei ole kindel, et kõik kindla andmebaasi tihendab võrdlusalus suureneva laadimise all samal viisil.

Võrdlusalus ja selle meetodi on kirjeldatud allpool üksikasjalikumalt.

## <a name="benchmark-summary"></a>Võrdlusalus Kokkuvõte
ASDB meetmed täitmise kombinatsiooni lihtsa andmebaasi toimingud, mis esinevad kõige sagedamini online tehingu töötlemise (OLTP) töökoormus. Kuigi aluseks on kujundatud cloud meeles Andmebaasiskeem, andmete populatsiooni arvutuste ja tehingud on mõeldud laiemalt tüüpiline olulisemaid kõige sagedamini kasutatavad OLTP töökoormus.

## <a name="schema"></a>Skeemi
Skeemi eesmärk on piisavalt erinevaid ja keerukuse toetamiseks mitmesuguseid toiminguid. Võrdlusalus töötab kuuluvad kuus tabeleid andmebaasi vastu. Tabelid jagunevad kolme rühma: fikseeritud suurus, suurust ja kasvab. On tabelitel fikseeritud suurus kolm skaleerimise tabelid; ja kasvab ühest tabelist. Fikseeritud suurus tabelid on pidev ridade arv. Skaleerimise tabelid on arvutite, mis on proportsionaalne andmebaasi jõudlust, kuid ei muutu ajal võrdlusalus. Kasvab tabel on suurusega nagu skaleerimise tabel esialgne, kuid seejärel arvutite muudatuste käigus võrdlusalus töötab, kui read on lisatud ja kustutatud.

Sisaldab skeemi erinevaid andmetüüpe, sh täisarv, arv, märk ja kuupäev/kellaaeg. Põhi-ja, kuid mitte ühtegi võõrvõtmed sisaldab skeemi – see on on viitamistervikluse piiranguid pole tabelite vahel.

Andmete genereerimine programmi loob algse andmebaasi andmeid. Täisarv- ja arvväärtused andmed luuakse erinevad strateegiad. Mõnel juhul levitatakse väärtusi vahemikus juhusliku ID-ga. Muudel juhtudel permuteeritud väärtuste kogumi juhusliku ID-ga, et tagada konkreetse jaotuse. Tekstiväljad luuakse kaalutud sõnade loendit, andes detsembri ilmega.

Andmebaas on suurusega põhjal "skaala tegurit." Skaala tegur (lühend SF) määratleb arvutite, et ja kasvab tabelid. Nagu allpool kirjeldatud jaotises kasutajad ja Pacing, andmebaasi mahtu, kasutajate ja maksimaalse jõudluse kõik mastaapimiseks proportsionaalselt üksteisest arv.

## <a name="transactions"></a>Tehinguid
Töökoormus koosneb üheksa tehingu tüübid, nagu on näidatud järgmises tabelis. Iga tehingu eesmärk esiletõstmiseks kindla süsteemi omadused andmebaasi mootor ja süsteemi riistvara, kõrge kontrastsusega kaudu muude toimingute kogum. Seda moodust hõlbustab erinevate osade üldise jõudluse mõju. Näiteks "Raske lugeda" tehingu annab toimingud kettalt hulk.

| Kande tüüp | Kirjeldus |
|---|---|
| Lugege Lite | VALIGE; -mälu; kirjutuskaitstud |
| Lugege Keskmine | VALIGE; enamasti-mälu; kirjutuskaitstud |
| Raske lugeda | VALIGE; enamasti mitte-mälu; kirjutuskaitstud |
| Lite värskendamine | VÄRSKENDAGE; -mälu; lugemine kirjutamine |
| Raske värskendamine | VÄRSKENDAGE; enamasti mitte-mälu; lugemine kirjutamine |
| Lisa Lite | LISA; -mälu; lugemine kirjutamine |
| Raske lisamine | LISA; enamasti mitte-mälu; lugemine kirjutamine |
| Kustutamine | KUSTUTAMINE; mälu-ja mitte-mälu; lugemine kirjutamine |
| CPU raske | VALIGE; -mälu; suhteliselt CPU koormuse; kirjutuskaitstud |

## <a name="workload-mix"></a>Töökoormus mix
Tehingud on valitud juhusliku kaalutud jaotuse järgmised üldised segada. Üldine mix on lugemis-ja kirjutamisõigusega suhe 2:1.

| Kande tüüp | % Mix |
|---|---|
| Lugege Lite | 35 |
| Lugege Keskmine | 20 |
| Raske lugeda | 5 |
| Lite värskendamine | 20 |
| Raske värskendamine | 3 |
| Lisa Lite | 3 |
| Raske lisamine | 2 |
| Kustutamine | 2 |
| CPU raske | 10 |

## <a name="users-and-pacing"></a>Kasutajate ja sammumine
Tööriista, mis esitab tehingud üle kogumi ühendused simuleerida arvu samaaegne Kasutajad toimimist juhitakse võrdlusalus töökoormus. Kuigi kõik ühendused ja tehingud on loodud arvutisse, lihtne me vaadake neid seoseid "kasutajate." Kuigi iga kasutaja töötab sõltumatult ülejäänud kasutajad, tehke kõik kasutajad sama tsükkel alltoodud juhiseid.

1. Andmebaasi ühendust luua.
2. Korrake, kuni märku, et väljuda:
    - Valige tehingut juhusliku (kaalutud leviloendi).
    - Valitud kannete ja mõõta vastuse aega.
    - Oodake, kuni kõndimine viivitust.
3. Sulgege andmebaasiühenduse.
4. Välju.

Kõndimine viivituse (juhises 2c) on valitud juhusliku, kuid jaotuse, mis on 1.0 teise keskmiseks. Iga kasutaja saab, keskmiselt luua seega kõige ühele sekundis.

## <a name="scaling-rules"></a>Skaleerimise reeglid
Andmebaasi suurus (mastaabi teguri ühikutes) määratakse nende kasutajate arv. On ühe kasutaja jaoks iga viis mastaabi teguri ühikut. Tõttu kõndimine viivitus ühe kasutaja saab luua kõige ühele sekundis, keskmiselt.

Näiteks skaala tegur 500 (SF = 500) andmebaas on 100 kasutajat ja võite saavutada 100 TPS piirmäär. Suurema TPS juhtida määr nõuab rohkem kasutajaid ja suurem andmebaasi.

Järgmises tabelis antakse ülevaade iga teenuse taseme- ja jõudluse taseme tegelikult püsiv kasutajate arv.

| Teenuse kiht (jõudlusega) | Kasutajatele | Andmebaasi suurus |
|---|---|---|
| Tavaline | 5 | 720 MB |
| Standardne (S0) | 10 | 1 GB |
| Standardne (S1) | 20 | 2.1 GB |
| Standardne (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6/P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Mõõtühikute kestus
Käivitage sobiv kriteerium nõuab ühtlane olekuga mõõtühikute kestus vähemalt üks tund.

## <a name="metrics"></a>Mõõdikud
Võtme mõõdikute rakenduses võrdlusalus on läbilaskevõime ja kutsele vastamise aeg.

- Läbilaskevõime on olulised jõudluse võrdlusalus meetme. Läbilaskevõime on kirjeldatud tehinguid –,-ajaühiku kohta, lugedes kõik tüübid.
- Vastuse aeg on näitaja, mille jõudlust. Vastuse kellaaja piirang erineb klassi teenuse kõrgema klassi teenuse võttes rangemaid vastuse tähtaeg nõue, nagu allpool näidatud.

| Klassi teenuse  | Läbilaskevõime mõõt | Vastuse aja nõue |
|---|---|---|
| Premium | Tehinguid sekundis | 95 protsentiili 0,5 sekundit |
| Standard | Tehingud minutis | 90-nda protsentiili 1,0 sekundit |
| Tavaline | Tehingud tunnis | 80 protsentiili 2.0 sekundit |

## <a name="conclusion"></a>Kokkuvõte
SQL Azure'i andmebaasi võrdlusalus meetmed suhteline täitmise Azure'i SQL-andmebaasi töötab kõigis saadaval teenuste astme ja jõudlust. Võrdlusalus kasutab kombinatsiooni lihtsa andmebaasi toimingud, mis esinevad kõige sagedamini online tehingu töötlemise (OLTP) töökoormus. Tegelike tulemuste mõõta, pakub võrdlusalus tähendusrikkama hindamise mõju läbilaskevõime jõudluse taseme muutmine, kui see on võimalik ainult loetletakse poolt iga taseme, nt CPU kiiruse, mälu suurus ja IOPS vahendite. Tulevikus jätkame arenevad võrdlusalus selle ulatust laiendada ja laiendada andmed.

## <a name="resources"></a>Ressursid
[SQL-andmebaasi tutvustus](sql-database-technical-overview.md)

[Teenuse astme ja jõudluse tasemed](sql-database-service-tiers.md)

[Jõudluse juhiseid ühe andmebaaside jaoks](sql-database-performance-guidance.md)
