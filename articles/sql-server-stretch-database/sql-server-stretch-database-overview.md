<properties
    pageTitle="Venitage andmebaasi ülevaade | Microsoft Azure'i"
    description="Siit saate teada, kuidas venitada andmebaasi migreerib külma andmete läbipaistev ja turvaline Microsoft Azure'i pilve."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Venitage andmebaasi ülevaade

Venitus andmebaasi migreerib külma andmete läbipaistev ja turvaline Microsoft Azure'i pilve.

Kui soovite lihtsalt alustada kasvõi kohe venitamine andmebaasi, vt [Alustuseks käitamise lubamine andmebaasi venitamine viisard](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Mis on venitamine andmebaasi eelised?
Venitus andmebaas sisaldab järgmised eelised:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Pakub maksumus\-külma andmete tõhus kättesaadavus
Venitage sooja ja külma kandeandmete dünaamiliselt SQL Serverist Microsoft Azure SQL serveri venitamine andmebaasi abil. Erinevalt tüüpiline külma andmesalv teie andmed on alati võrgus ja saadaval päringu. Saate sisestada pikem andmete säilitamise ajaskaalade ajamata suured tabelid, nt klientide tellimused. Azure'i asemel skaleerimist kallis, madala hinna kasu\-ruumide salvestusruumi. Valige hinnakirjad taseme ja Azure portaalis säilitada kontrolli kulude sätete konfigureerimine. Skaala üles või alla vastavalt vajadusele. Külastage [SQL serveri venitamine andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) lehelt.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Päringute või rakenduste muudatused pole nõutav.
SQL serveri andmeid sujuvalt olenemata sellest, kas see on juurdepääs\-ruumide või tõmmatud pilveteenusesse.  Saate määrata poliitika, mis määratleb, kus talletatakse andmeid ja SQL serveri käsitleb andmete liikumine taustal. Kogu tabel on alati võrgus ja päringuvõimalusega. Ja, venitamine andmebaasi ei nõua olemasolevad päringud või rakenduste muudatused – asukoha andmed on täielikult läbipaistev rakendus.

### <a name="streamlines-on-premises-data-maintenance"></a>Klõpsake ühtlustab\-ruumide andmete hooldustööd
Vähendada\-ruumide hooldus ja mäluruumi teie andmete jaoks. Varukoopiate jaoks oma kohta\-ettevõtte andmeid kiiremini käitada ja hooldus aknas sees valmis. Varukoopiate pilveteenuse osa teie andmete jaoks automaatselt käivituma. Teie kohta\-ruumide salvestusruumi vajadustele oluliselt vähendada. Azure'i salvestusruumi saab 80% kui lisamise kohta\-ruumide SSD.

### <a name="keeps-your-data-secure-even-during-migration"></a>Tagab andmete turvalisuse isegi migreerimisel
Nautige meelerahu, nagu te venitada pilveteenusesse turvaliseks olulisimatest rakenduste. SQL serveri alati krüptitud pakub liikumistee andmete krüptimine. Rea taseme turvalisus (RLS) ja muid SQL serveri turvalisuse funktsioone töötada ka venitamine andmebaasi andmete kaitsmine.

## <a name="what-does-stretch-database-do"></a>Mida teeb venitamine andmebaasi?
Kui olete lubanud venitamine andmebaasi SQL serveri eksemplar, andmebaasi ja vähemalt üks tabel, venitamine andmebaasi vaikselt hakkab külm andmete migreerimiseks Azure.

-   Kui salvestate külma andmete eraldi tabelisse, saate migreerida kogu tabel.

-   Kui teie tabel sisaldab nii ja kuuma andmeid, saate määrata migreerida ridade filtreerimine funktsiooni.

**Teil pole muuta olemasolevad päringud ja kliendi rakendused.** Teil on jätkuvalt sujuvalt kohalike ning kaugarvutite andmetele juurdepääs, isegi andmete migreerimisel. On väike hulk latentsuse remote päringuid, kuid ilmneda ainult selle latentsus, kui teete päringu külma andmed.

Kui tõrge ilmneb migreerimisel **venitamine andmebaasi tagab, et andmed on kadunud** . Samuti on uuesti loogika käsitlema ühenduse probleemid, mis võivad ilmneda migreerimisel. Dünaamilise juhtimise vaadata pakub migreerimise olek.

**Saate peatada andmete migreerimise** tõrkeotsing kohalikus serveris või maksimeerimiseks võrgu läbilaskevõime.

![Venitage andmebaasi ülevaade][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Venitamine andmebaas on teie jaoks?
Kui saate teha järgmist laused, venitamine andmebaasi võib aidata teie nõuetele ja oma probleemide lahendamine.

|Kui olete otsust maker|Kui olete mõne DBA|
|------------------------------|-------------------|
|Mul on hoida kandeandmete pikka aega.|Minu tabeli suurus muutub kontrolli all.|
|Mõnikord on mul külm andmepäringu.|Minu kasutajad teatavad, et külma andmetele juurdepääsu soovivad, et ta harva seda kasutada.|
|Mul on rakendused, sealhulgas vanemate rakendused, mida ma ei soovi, et värskendada.|Mul on säilitada osta ja lisada rohkem ruumi.|
|Soovin leida viis, kuidas säästa salvestusruumi.|Ma ei saa varundada või taastada selliste suurte tabelite SLA sees.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Millist andmebaasid ja tabelid on leviloendite puhul venitamine andmebaasi?
Venitage andmebaasi sihtkohtade selgituseks andmebaaside suurte andmemahtude külma, tavaliselt talletatud väheste tabelid. Järgmised tabelid võib sisaldada rohkem kui miljardit read.

Kui kasutate funktsiooni ajaline tabeli SQL Server 2016, abil venitamine andmebaasi migreerida täielikult või osaliselt seotud ajalugu tabeli maksumus\-efektiivse salvestusruumi Azure. Kohta leiate teemast [Säilitamine, ajalooliste andmete haldamine süsteemi versiooniga ajaline tabelites](https://msdn.microsoft.com/library/mt637341.aspx).

Venitamine andmebaasi Advisor, SQL Server 2016 täiendamine Advisor, funktsioon abil saate tuvastada andmebaase ja tabeleid andmebaasi venitamine. Lisateavet leiate teemast [tuvastamine andmebaasid ja venitamine andmebaasi tabelid](sql-server-stretch-database-identify-databases.md). Blokeerimise võimalike probleemide kohta leiate lisateavet teemast [venitamine andmebaasi piirangud](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Ketas venitamine andmebaasi testimine
**Testige ketas näidisandmebaasi AdventureWorks venitamine andmebaasi.** Näidisandmebaasi AdventureWorks saamiseks vähemalt andmebaasifaili ja näidiseid ja skriptide faili alla laadida [siin](https://www.microsoft.com/download/details.aspx?id=49502). Pärast näidisandmebaasi eksemplari SQL Server 2016 taastamiseks pakkige see lahti näidised faili ja avage fail, venitamine DB näidised venitamine DB kaustast. Skripte selle faili kontrollida oma andmeid enne kasutatavat ruumi ja kui olete lubanud venitamine andmebaasi andmete migreerimise edenemist jälgida ja kinnitada, et võite jätkata päringu olemasolevad andmed ja uute andmete lisamiseks, nii jooksul pärast andmete migreerimist.

## <a name="next-step"></a>Järgmise juhise juurde
**Leidke andmebaasid ja tabelid, mis venitamine andmebaasi, saavad.** SQL Server 2016 täiendamine Advisor alla laadida ja installida tuvastamiseks andmebaasid ja tabelid, mis venitamine andmebaasi, saavad selle venitamine andmebaasi. Venitus andmebaasi Advisor tuvastab ka blokeerimise probleemid. Lisateavet leiate teemast [tuvastamine andmebaasid ja venitamine andmebaasi tabelid](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
