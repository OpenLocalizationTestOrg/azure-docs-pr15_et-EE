<properties
   pageTitle="Ülevaade: Azure'i andmesalve Lake | Microsoft Azure'i"
   description="Mis on Azure andmesalve Lake ja muude andmete poed üle pakub väärtuse mõistmine"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Azure'i andmesalve Lake ülevaade

Azure'i andmesalve Lake on suur andmete analüütiline töökoormus on kogu ettevõtte hyper skaala hoidla. Azure'i andmed Lake võimaldab mis tahes suurus, -tüübi ja manustamisest kiirus ühe ühes kohas funktsionaalseid ja uurimusliku analytics andmete talletamiseks.

> [AZURE.TIP] Kasutada [Lake andmesalve õppeteema](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) häälestamisega Azure'i andmed Lake Store service.

Azure'i andmesalve Lake pääseb Hadoopi (saadaval Hdinsightiga kobar) WebHDFS ühilduv REST API-de kaudu. See on loodud lubada Kasutusanalüüsi salvestatud andmeid ja jõudluse andmete analytics stsenaariumid on häälestatud. Välja kasti, see sisaldab äriklassi võimaluste – turvalisus, hallatavust, skaleeritavus, töökindluse ja kättesaadavus – olulised tegelike Enterprise'i kasutamise juhtudel.


![Azure'i andmed Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Mõned Azure'i andmed järve võtme funktsioonid on järgmised.

### <a name="built-for-hadoop"></a>Hadoopi ehitatud

Azure'i andmed Lake pood on Hadoopi jaotatud faili süsteemi (HDFS) ja töötab Hadoopi ökosüsteemi Apache Hadoop failisüsteemis.  Teie olemasolevad Hdinsightiga rakenduste või teenuseid, mis kasutavad WebHDFS API saate hõlpsasti integreerida Lake andmesalve. Lake andmesalve ka seab WebHDFS ühilduv ülejäänud kasutajaliidese rakenduste

Andmed salvestatakse Lake andmesalve saab hõlpsasti analüüsida Hadoopi analüütiline raamistik, nt MapReduce või taru abil. Microsoft Azure Hdinsightiga kogumite saate ette valmistatud ja otsest juurdepääsu andmetele, mis on talletatud Lake andmesalve konfigureeritud.

### <a name="unlimited-storage-petabyte-files"></a>Piiramatu salvestusruum petabyte failid

Azure'i andmesalve Lake pakub piiramatu salvestusruum ja sobib mitmesuguseid analytics andmete talletamiseks. See ei too konto suurused, failide või andmehulga talletatavate andmete järv piirangud. Üksikuid faile saate ulatuvad kilobaiti terabait suurus, mistõttu on hea valik, mis tahes tüüpi andmete talletamiseks. Andmeid talletatakse püsivalt, muutes mitmest eksemplarist ja ajaks, mille andmeid saab salvestada andmed järve, pole piiratud.

### <a name="performance-tuned-for-big-data-analytics"></a>Jõudluse häälestatud suur andmeanalüüsi

Azure'i andmesalve Lake on loodud töötavad suures ulatuses analüütiliste süsteemide, mis nõuavad suuri läbilaskevõime päringu ja analüüsi suurte andmehulkade jaoks. Andmete järve kahel faili osa üksikud serverit arvu. See parandab loetuks läbilaskevõime faili paralleelselt läbimiseks analytics andmete lugemise ajal.


### <a name="enterprise-ready-highly-available-and-secure"></a>Ettevõttele kasutusvalmis: Tugevalt-saadaval ja turvaline

Azure'i andmesalve Lake pakub industry-standard-saadavus ja töökindlus. Oma andmete varasid talletatakse püsivalt, muutes liigsete eksemplarid, mis kaitsevad mis tahes ootamatuid tõrkeid. Suurettevõtete Azure'i andmed Lake kasutada oma olemasolevad andmed platvormi olulised osana nende lahendused.

Lake andmesalve pakub ka ettevõtte tasemel turvalisust talletatud andmed. Lisateavet leiate teemast [turvamine andmete Azure'i Lake poes](#DataLakeStoreSecurity).


### <a name="all-data"></a>Kõik andmed

Azure'i andmesalve Lake saate mis tahes andmete talletamiseks kohalikke vormingus, nagu on, mis tahes eelneva teisendused nõudmata. Lake andmesalve ei nõua skeemi määratleda enne andmete hooleks üksikute analüütiline raames tõlgendada andmeid ja määratleda skeemi analüüsi ajal. On võimalik salvestada faile suvalise suuruse ja vormingud võimaldab Lake andmesalve käsitlema liigendatud, poolstruktuur- ja struktureerimata andmed.

Azure'i andmesalve Lake ümbriste andmete jaoks on põhiosas kaustad ja failid. Saate töötada SDK-d, Azure portaali ja Azure PowerShelli abil salvestatud andmeid. Kui teie andmed panema liideste kaudu ja kasutamise korral ümbriste salvestuskohta, saate salvestada mis tahes tüüpi andmeid. Lake andmesalve sooritada mis tahes käsitsemise andmete põhjal salvestab andmete tüüpi.


## <a name="DataLakeStoreSecurity"></a>Andmete turvamise Azure'i Lake poes

Azure'i andmesalve Lake kasutab Azure Active Directory autentimise ja juurdepääsu reguleerimine (ACL) on loetletud teie andmetele juurdepääsu haldamiseks.

| Funktsioon                                 | Kirjeldus                              |
|-----------------------------------------|------------------------------------------|
| Autentimine | Azure'i andmesalve Lake ühendab kõik andmed, mis on talletatud Azure'i andmesalve Lake identiteedi ja juurdepääsu halduse ja Azure Active Directory (AAD). Tulemusena integreerimine, Azure'i andmed Lake kasu kõiki AAD funktsioone, sh mitmikautentimise, juurdepääsu, Rollipõhine juurdepääsu reguleerimine, rakenduse kasutamine jälgimine, jälgimine ja hoiatada, Turve jne. Azure'i andmesalve Lake toetab protokolli OAuth 2.0 koos ülejäänud liideses autentimiseks. |
| Juurdepääsu reguleerimine                          | Azure'i andmesalve Lake pakub juurdepääsu reguleerimine täiendavad POSIX-laadi õigused, mis on esitatud WebHDFS Protocol (protokoll). Praeguses versioonis saate ACL lubatud juurkausta, alamkaustadeks, samuti üksikuid faile. ACL-ID, saate rakendada juurkausta on kohaldatav ka kõik lapse kaustad ja failid ka.|

Kas soovite lisateavet Lake andmesalve andmete turvamise kohta. Järgige saamiseks allpool olevaid linke.

* Turvaline andmetega Lake andmesalve juhised leiate artiklist [turvamine andmete Azure'i Lake poes](data-lake-store-secure-data.md).
* Kas eelistate videod? [Vaadake seda videot,](https://mix.office.com/watch/1q2mgzh9nn5lx) kuidas tagada Lake andmesalve talletatavad andmed.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Rakenduste ühildu Azure'i andmesalve Lake

Azure'i andmesalve Lake sobib kõige avatud allika komponendid Hadoopi ökosüsteemis. See ka ühendab kenasti Azure muude teenustega. See muudab Lake andmesalve suurepärane võimalus teie andmete salvestusruumi vajadustele. Järgige rohkem teada saada, kuidas Lake andmesalve saab kasutada nii avatud allika komponendid, samuti muud Azure teenused linkidest.

* Lugege teemat [rakenduste ja teenuste ühildu Azure'i andmesalve Lake](data-lake-store-compatible-oss-other-applications.md) avatud allika rakenduste koostalitlusvõimelised Lake andmesalve loendi.
* Lugege teemat mõista, kuidas Lake andmesalve saab kasutada muude Azure teenustega lubamiseks suurema hulga stsenaariumid [integreerimine Azure muude teenustega](data-lake-store-integrate-with-other-services.md) .
* Artiklist saate teada, kuidas kasutada Lake andmesalve stsenaariumid, nt söömisega andmeid, andmete töötlemise, andmete allalaadimine ja andmete visualiseerimine [Lake andmesalve kasutamise stsenaariumid](data-lake-store-data-scenarios.md) .

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Mis on Azure andmesalve Lake failisüsteemi (adl: / /)?

Uue failisüsteemi, kuvatakse AzureDataLakeFilesystem pääseb Lake andmesalve (adl: / /), Hadoop keskkonnas (saadaval Hdinsightiga kobar). Rakendused ja teenused, mida kasutada adl: / / saavad täpsemaks jõudluse optimeerimine, mis pole praegu saadaval WebHDFS ära. Selle tulemusena Lake andmesalve kaudu saate kas kasutada parima jõudluse kasutamise adl soovitatav suvandiga: / / või säilitada olemasoleva koodi kasutamist WebHDFS API otse jätkata. Azure Hdinsightiga mõjutab täielikult AzureDataLakeFilesystem Lake andmesalve parima jõudluse anda.

Pääsete oma andmetele Lake andmesalve kasutamise `adl://<data_lake_store_name>.azuredatalakestore.net`. Andmete Lake andmesalve juurdepääsemise kohta lisateabe saamiseks vaadake [salvestatud andmeid atribuutide vaatamine](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Kuidas Azure'i andmesalve Lake kasutusele võtta?

Vt [Alustamine Lake andmesalve abil Azure portaali](data-lake-store-get-started-portal.md), ettevalmistamise Azure'i portaalis andmesalve Lake. Kui on ette valmistatud Azure'i andmed Lake, saate teada, kuidas kasutada suurt andmete pakkumisi nagu Azure'i andmeanalüüsi Lake või Windows Azure Hdinsightiga Lake andmesalve. Saate luua ka .net-i rakenduse Azure'i andmesalve Lake konto loomine ja toiminguid nagu andmete üles, alla laadida, jne.

- [Azure'i andmeanalüüsi Lake kasutamise alustamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Azure'i Lake andmesalve kasutades .NET SDK kasutamise alustamine](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Lake andmesalve videod

Kui eelistate videotest saate teada, Lake andmesalve videod pakub mitmeid funktsioone.

* [Azure'i andmesalve Lake konto loomine](https://mix.office.com/watch/1k1cycy4l4gen)
* [Andmete Exploreri abil Azure'i Lake poes andmete haldamine](https://mix.office.com/watch/icletrxrh6pc)
* [Azure'i andmeanalüüsi Lake ühenduse Azure'i andmesalve Lake](https://mix.office.com/watch/qwji0dc9rx9k)
* [Juurdepääs Azure'i Lake poe kaudu Lake andmeanalüüsi](https://mix.office.com/watch/1n0s45up381a8)
* [Ühenduse Azure'i andmesalve Lake Azure Hdinsightiga](https://mix.office.com/watch/l93xri2yhtp2)
* [Juurdepääs Azure'i Lake poe kaudu taru ja siga](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Andmete kopeerimine, et ja Azure Lake poest DistCp (Hadoopi jaotatud koopia) abil](https://mix.office.com/watch/1liuojvdx6sie)
* [Andmete teisaldamine relatsiooniliste andmeallikate ja Azure andmesalve Lake Apache Sqoop abil](https://mix.office.com/watch/1butcdjxmu114)
* [Andmete korraldamise Azure'i andmed Factory kasutamise Azure'i andmesalve Lake](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Andmete turvamise Azure'i andmesalve Lake](https://mix.office.com/watch/1q2mgzh9nn5lx)



