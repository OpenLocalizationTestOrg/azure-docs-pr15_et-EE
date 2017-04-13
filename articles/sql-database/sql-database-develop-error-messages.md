<properties
    pageTitle="Tõrkekoodid SQL - andmebaasi ühendus viga | Microsoft Azure'i"
    description="Teavet SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes, nt levinud andmebaasi ühenduse tõrkeid, andmebaasi Kopeeri probleemid ja üldisi tõrkeid. "
    keywords="SQL-i tõrkekood, Accessi SQL-i, andmebaasi ühendus viga, SQL-i kuvatavad tõrkekoodid"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes: andmebaasi ühenduse tõrge ja ka muude probleemide


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Selles artiklis on loetletud tõrkekoodid SQL-i SQL-andmebaasi klientrakendustes, sh andmebaasiga ühenduse tõrkeid, siirdamiseks tõrked (nimetatakse ka siirdamiseks vead), ressursside juhtimise tõrkeid, andmebaasi Kopeeri probleemid, elastne rakenduskausta ja muud. Enamik kategooriad on teatud Azure'i SQL-andmebaasi ja Microsoft SQL Server ei rakendata.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Andmebaasi ühenduse tõrkeid, siirdamiseks tõrgete ja muud ajutised tõrked

Järgmises tabelis antakse ülevaade SQL-i tõrkekoodid ühenduse kaotsimineku tõrgete ja muud siirdamiseks tõrked võivad ilmneda, kui teie rakendus proovib juurde pääseda SQL-andmebaasi. Kuidas alustamise õpetus Azure SQL-andmebaasiga ühenduse loomise, leiate [Azure'i SQL-andmebaasiga ühenduse loomine](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Levinumad vead, andmebaasi ühendus ja siirdamiseks viga tõrked

Azure'i infrastruktuuri on võimalus dünaamiliselt konfigureerima serverid, kui suur töökoormus ilmneb SQL-andmebaasi.  See dünaamiline käitumine võib põhjustada teie kliendi programmi kaotada ühenduse SQL-andmebaasiga. Seda tüüpi tõrke nimetatakse *siirdamiseks viga*.

Kui teie klient programmi uuesti loogika, võite proovida seda taastamiseks pärast töölt siirdamiseks vea parandada ise ühendust.  Soovitame, et te viivitus 5 sekundit enne oma esimese proovi uuesti. Proovitakse uuesti pärast lühem sekundi riske suur pilveteenusesse viivitust. Jaoks iga järgmise proovi uuesti viivituse peaks kasvada, kuni 60 sekundi järel.

Siirdamiseks viga tõrgete näidata tavaliselt ühe oma kliendi programmide järgmised tõrketeated:

- Andmebaasi < db_name > < Azure_instance > server pole praegu saadaval. Proovige ühenduse hiljem uuesti. Kui probleem ei lahene, pöörduge klienditoe poole ja saatke neile < session_id > seansi jälgimine ID.

- Andmebaasi < db_name > < Azure_instance > server pole praegu saadaval. Proovige ühenduse hiljem uuesti. Kui probleem ei lahene, pöörduge klienditoe poole ja anda neile < session_id > ID seansi jälgimine. (Microsoft SQL Server, tõrge: 40613)

- Olemasoleva ühenduse suleti sunniviisiliselt remote host järgi.

- System.Data.Entity.Core.EntityCommandExecutionException: Ilmnes tõrge käivitamisel käsk määratlus. Lugege teemat sisemine erand üksikasju. ---> System.Data.SqlClient.SqlException: transport tasemel tõrge ilmnes tulemuste saamisel serverist. (pakkuja: seansi pakkuja, tõrge: 19 - füüsilise ühenduse ei saa kasutada)

Proovi uuesti loogikat koodi näiteid leiate:

- [Andmeühenduste teegid SQL-andmebaas ja SQL Server](sql-database-libraries.md) 
- [Toimingud SQL-andmebaasi ühendus tõrgete ja siirdamiseks tõrgete parandamine](sql-database-connectivity-issues.md)

Arutelu *blokeerimine perioodi* ADO.net-i kasutavate klientide jaoks on saadaval [SQL Serveri ühenduse ühiskasutus (ADO.net-i)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Tõrkekoodid siirdamiseks viga

Järgmistest tõrketeadetest on ajutine ja peaks proovitakse rakenduse loogika 

| Tõrkekood | Raskusaste | Kirjeldus |
| ---: | ---: | :--- |
| 4060 | 16 | Andmebaasi ei saa avada "% & #x2a; ls" taotleja login. Login nurjus. |
|40197|17|Teenuse ilmnes tõrge päringu töötlemine. Proovige uuesti. Tõrkekood %d.<br/><br/>Saate selle vea, kui teenus on alla tarkvara või riistvara uuendamine, riistvara tõrkeid või muude Tõrkesiirde probleemide tõttu. Tõrge 40197 sõnumis oleva tõrkekoodi (%d) annab Lisateavet tüüpi tõrge või Tõrkesiirde toimunud. Mõned näited viga koodid on kinnistatud tõrge 40197 sõnum on 40020, 40143, 40166 ja 40540.<br/><br/>Uuesti oma SQL-andmebaasi server automaatselt teid ühendatakse andmebaasi terve koopia. Rakenduse peab jaole juba tõrkelogi 40197, manustatud tõrkekoodi (%d) tõrkeotsingu sõnumis ja proovige uuesti SQL-andmebaasiga, kuni ressursid on saadaval ja teie ühendus on loodud uuesti.|
|40501|20|Teenus on hetkel hõivatud. Kutse uuesti 10 sekundi pärast. Langeva ID: %ls. Kood: %d.<br/><br/>*Märkus:* Lisateavet leiate teemast:<br/>• [Azure'i SQL-andmebaasi ressursi piirangud](sql-database-resource-limits.md).
|40613|17|Andmebaasi '% & #x2a; ls' serveris '% & #x2a; ls' pole praegu saadaval. Proovige ühenduse hiljem uuesti. Kui probleem ei lahene, pöörduge klienditoe poole ja saatke neile seansi jälgimise ID-d '% & #x2a; ls'.|
|49918|16|Päringut ei saa töödelda. Taotluse töötlemiseks pole piisavalt ressursse.<br/><br/>Teenus on hetkel hõivatud. Taotluse proovige hiljem uuesti. |
|49919|16|Protsessi ei saa luua ega värskendada taotlus. Liiga palju loomine või värskendamine toimingud tellimuse "% ld".<br/><br/>Teenus on hõivatud töötlemiseks mitme luua või värskendada oma tellimuse või serveri. Taotlused on praegu blokeeritud ressursside optimeerimine. Päringu [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) jaoks ootel toiminguid. Oodake, kuni ootel loomine või värskendamine taotlusi on lõpule jõudnud või kustutada üks teie ootel päringuid ja teie taotlus hiljem uuesti proovida. |
|49920|16|Päringut ei saa töödelda. Tellimuse jaoks liiga palju toimingud "% ld".<br/><br/>Teenus on hõivatud mitme taotluste selle tellimuse jaoks. Taotlused on praegu blokeeritud ressursside optimeerimine. Päringu [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) toimingu olek. Oodake, kuni ootel taotlusi on lõpule jõudnud või kustutada oma ootel kutsed ühte uuesti teie taotlus hiljem |

## <a name="database-copy-errors"></a>Andmebaasi kopeerimine tõrked

Järgmistest tõrketeadetest, saate ilmnes kopeerimine andmebaasi Azure SQL-andmebaasis. Lisateavet leiate teemast [kopeerida Azure'i SQL-andmebaasi](sql-database-copy.md).


|Tõrkekood|Raskusaste|Kirjeldus|
|---:|---:|:---|
|40635|16|Kliendi IP-aadressiga '% & #x2a; ls' ajutiselt on keelatud.|
|40637|16|Luua andmebaasi kopeerimine on praegu keelatud.|
|40561|16|Andmebaasi kopeerimine nurjus. Lähte- või sihtloendil andmebaasi pole olemas.|
|40562|16|Andmebaasi kopeerimine nurjus. Allika andmebaasi on kukkunud.|
|40563|16|Andmebaasi kopeerimine nurjus. Sihtandmebaas on kukkunud.|
|40564|16|Andmebaasi kopeerimine nurjus sisemise tõrke tõttu. Target (sihtkoht) andmebaasist ja proovige uuesti.|
|40565|16|Andmebaasi kopeerimine nurjus. Rohkem kui 1 samaaegseid andmebaasi kopeerimine sama lähtekoha on lubatud. Target (sihtkoht) andmebaasist ja proovige uuesti.|
|40566|16|Andmebaasi kopeerimine nurjus sisemise tõrke tõttu. Target (sihtkoht) andmebaasist ja proovige uuesti.|
|40567|16|Andmebaasi kopeerimine nurjus sisemise tõrke tõttu. Target (sihtkoht) andmebaasist ja proovige uuesti.|
|40568|16|Andmebaasi kopeerimine nurjus. Lähteandmebaasi on muutunud pole saadaval. Target (sihtkoht) andmebaasist ja proovige uuesti.|
|40569|16|Andmebaasi kopeerimine nurjus. Sihtandmebaas on muutunud pole saadaval. Target (sihtkoht) andmebaasist ja proovige uuesti.|
|40570|16|Andmebaasi kopeerimine nurjus sisemise tõrke tõttu. Target (sihtkoht) andmebaasist ja proovige uuesti.|
|40571|16|Andmebaasi kopeerimine nurjus sisemise tõrke tõttu. Target (sihtkoht) andmebaasist ja proovige uuesti.|

## <a name="resource-governance-errors"></a>Ressursside juhtimise tõrked

Järgmistest tõrketeadetest põhjustavad liigne ressursid töötamisel Azure'i SQL-andmebaasi kasutamine. Näiteks:

- Tehingu on liiga kauaks avatud.
- Tehingu hoiab liiga palju lukud.
- Rakenduse tarbib liiga palju mälu.
- Rakenduse tarbib liiga palju `TempDb` ruumi.

Seotud teemad:

* Üksikasjalikumat teavet on saadaval: [Azure'i SQL-andmebaasi ressursi piirangud](sql-database-resource-limits.md)

|Tõrkekood|Raskusaste|Kirjeldus|
|---:|---:|:---|
|10928|20|Ressursi ID: %d. Andmebaasi %s limiit on %d ja saavutamiseni. Lisateavet leiate teemast [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Ressursi ID näitab ressurss, mis on jõudnud. Töötaja teemad, ressursi ID-d = 1. Seansid, ressursi ID-d = 2.<br/><br/>*Märkus:* Selle probleemi lahendamiseks ja selle tõrke kohta lisateabe saamiseks vt<br/>• [Azure'i SQL-andmebaasi ressursi piirangud](sql-database-resource-limits.md). |
|10929|20|Ressursi ID: %d. %S minimaalne tagatis on %d, piirmäär on %d ja praeguse andmebaasi kasutamine on %d. Server on praegu hõivatud toetada taotlusi, mis on suurem kui %d see andmebaas. Lisateavet leiate teemast [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Muul juhul, proovige hiljem uuesti.<br/><br/>Ressursi ID näitab ressurss, mis on jõudnud. Töötaja teemad, ressursi ID-d = 1. Seansid, ressursi ID-d = 2.<br/><br/>*Märkus:* Selle probleemi lahendamiseks ja selle tõrke kohta lisateabe saamiseks vt<br/>• [Azure'i SQL-andmebaasi ressursi piirangud](sql-database-resource-limits.md).|
|40544|20|Andmebaas on jõudnud oma mahupiirangut. Sektsiooni või Kustuta andmed, langev registrid või võimalikud lahendused dokumentatsioonist.|
|40549|16|Seanss lõpetatakse, kuna olete pikaajaline tehingu. Proovige oma tehingu lühendamine.|
|40550|16|Seansi on lõpetatud, kuna see on liiga palju lukud omandanud. Proovige lugemis- või vähem ridade ühe toimingu muutmine.|
|40551|16|Seansi on lõpetatud, kuna liigse `TEMPDB` kasutamine. Proovige vähendada ajutise tabeli ruumi kasutamine oma päringu muutmine.<br/><br/>*Näpunäide:* Ajutiste objektide kasutamisel säästa ruumi, on `TEMPDB` andmebaasi kukutage ajutiste objektide pärast seda, kui nad ei ole enam vaja seanss.|
|40552|16|Seansi on lõpetatud liigse tehingu Logi ruumi kasutuse tõttu. Proovige muuta ühe toimingu vähem ridu.<br/><br/>*Näpunäide:* Kui teete hulgi lisab abil soovitud `bcp.exe` kasuliku või `System.Data.SqlClient.SqlBulkCopy` klassi, proovige kasutada funktsiooni `-b batchsize` või `BatchSize` iga tehingu serveri kopeeritud ridade arv piirata suvandid. Kui teil on taastamine indeks on `ALTER INDEX` lause, proovige kasutada funktsiooni `REBUILD WITH ONLINE = ON` suvand.|
|40553|16|Seansi on lõpetatud liigse mälukasutuse tõttu. Proovige muuta päringu töötlemiseks vähem ridu.<br/><br/>*Näpunäide:* Arvu `ORDER BY` ja `GROUP BY` toimingute Transact-SQL-koodi vähendab päringu mälu nõuetele.|

## <a name="elastic-pool-errors"></a>Elastne rakenduskausta tõrked

Järgmistest tõrketeadetest on seotud loomise ja kasutamise kummid kaustu.

| Tõrkenumber | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | Elastne pool on täis salvestusruumi. Salvestusruumi kasutuse jaoks elastne rakenduskausta ei tohi ületada (%d) MB. | Elastne pool ruumi piirata MB. | Kui talletuslimiidi elastne pool andmebaasi andmete kirjutamise katsel. | Palun kaaluge suurendamist DTUs elastne pool võimalusel selleks, et oma salvestuslimiidi suurendada, vähendada kasutatav üksikute andmebaaside maksimaalselt elastne pool salvestusruum või elastne rakenduskausta andmebaaside eemaldamine. |
| 10929 | EX_USER | %S minimaalne tagatis on %d, piirmäär on %d ja praeguse andmebaasi kasutamine on %d. Server on praegu hõivatud toetada taotlusi, mis on suurem kui %d see andmebaas. Lugege teemat [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) abi. Muul juhul, proovige hiljem uuesti. | DTU min andmebaasi; DTU max andmebaasi kohta | Samaaegseid töötajate (taotluste) üle kõik andmebaasid elastne kogumi koguarvu proovis rakenduskausta limiiti ületada. | Palun kaaluge suurendamist DTUs elastne pool võimaluse korral selle töötaja piiri suurendamiseks või eemaldada andmebaaside elastne kaustast. |
| 40844 | EX_USER | Andmebaasi '%ls' Server '%ls' on '%ls' väljaande andmebaasi kohapeal on elastne ja ei tohi olla pidev Kopeeri seose. | andmebaasi nimi, andmebaasi väljaande, serveri nimi | StartDatabaseCopy käsk antakse-premium db on elastne kogumi. | Tulen varsti |
| 40857 | EX_USER | Server ei leitud elastne rakenduskausta: '%ls', elastne kausta nimi: '%ls'. | serveri; elastne kausta nimi | Määratud server pole määratud elastne pool. | Sisestage kehtiv elastne kausta nimi. |
| 40858 | EX_USER | Elastne rakenduskausta '%ls' on juba olemas server: '%ls' | elastne rakenduskausta serveri nimi | Määratletud elastne kogumi määratud loogilise server juba olemas. | Sisestage uue elastne kausta nimi. |
| 40859 | EX_USER | Elastne rakenduskausta ei toeta teenuse kiht '%ls'. | elastne rakenduskausta teenuse kiht | Määratud teenuse kiht elastne rakenduskausta ettevalmistamise ei toetata. | Sisestage õiget või teenuse kiht kasutada vaikimisi teenuse kiht tühjaks jätta. |
| 40860 | EX_USER | Elastne rakenduskausta '%ls' ja teenuse eesmärk '%ls' kombinatsiooni ei sobi. | elastne kausta nimi; teenuse taseme eesmärk nimi | Elastne rakenduskausta ja teenuse eesmärgi saate määrata koos ainult juhul, kui teenus eesmärk on määratud "ElasticPool". | Määrake elastne rakenduskausta ja teenuse eesmärki õige kombinatsiooni. |
| 40861 | EX_USER | Andmebaasi väljaande ' %. *ls' ei saa erinevas elastne rakenduskausta teenuse kiht, mis on ' %.* LS. | andmebaasi väljaande elastne rakenduskausta teenuse kiht | Andmebaasi väljaande erineb elastne rakenduskausta teenuse kiht. | Määrake andmebaasi väljaanne, mis on erinevas elastne rakenduskausta teenuse kiht.  Pange tähele, et andmebaasi väljaande ei pea olema määratud. |
| 40862 | EX_USER | Elastne kausta nimi peab olema määratud, kui elastne rakenduskausta teenuse eesmärki on määratud. | Ükski | Elastne rakenduskausta teenuse eesmärki ei tuvasta kohapeal on elastne. | Määrake elastne kausta nimi, kui abil elastne rakenduskausta teenuse eesmärk. |
| 40864 | EX_USER | DTUs elastne rakenduskausta jaoks peab olema vähemalt (%d) DTUs jaoks teenuse kiht ' %. * ls'. | DTUs jaoks elastne pool; elastne rakenduskausta teenuse kiht. | Üritades DTUs elastne rakenduskausta alla alampiiri jaoks määrata. | Proovige uuesti säte on DTUs vähemalt alampiiri pool kummipael. |
| 40865 | EX_USER | Mõeldud pool elastne DTUs ei tohi ületada (%d) DTUs jaoks teenuse kiht ' %. * ls'. | DTUs jaoks elastne pool; elastne rakenduskausta teenuse kiht. | Proovite määrata DTUs jaoks elastne rakenduskausta kohal lubatud. | Proovige uuesti seadmine DTUs elastne rakenduskausta jaoks lubatud üle. |
| 40867 | EX_USER | Max andmebaasi kohta DTU peab olema vähemalt (%d) jaoks teenuse kiht ' %. * ls'. | DTU max andmebaasi; elastne rakenduskausta teenuse kiht | Proovite määrata DTU max all toetatud arvu piirangu andmebaasi kohta. | Palun kaaluge elastne rakenduskausta teenuse kiht, mis toetab soovitud seade. |
| 40868 | EX_USER | Max andmebaasi kohta DTU ei tohi ületada (%d) teenuse kiht ' %. * ls'. | DTU max andmebaasi; elastne rakenduskausta teenuse kiht. | Proovite määrata DTU max toetatud limiidi andmebaasi kohta. | Palun kaaluge elastne rakenduskausta teenuse kiht, mis toetab soovitud seade. |
| 40870 | EX_USER | DTU min andmebaasi kohta ei tohi ületada (%d) teenuse kiht ' %. * ls'. | DTU min andmebaasi; elastne rakenduskausta teenuse kiht. | Proovite määrata DTU min toetatud limiidi andmebaasi kohta. | Palun kaaluge elastne rakenduskausta teenuse kiht, mis toetab soovitud seade. |
| 40873 | EX_USER | Andmebaaside (%d) ja DTU min (%d) andmebaasi kohta arv ei tohi ületada DTUs elastne pool (%d). | Andmebaaside arv elastne kogumi; DTU min andmebaasi; DTUs elastne pool. | Proovite määrata DTU min andmebaaside ületab DTUs elastne pool elastne kogumi. | Kaaluge suurendamist DTUs elastne pool või vähenda DTU min andmebaasi kohta või andmebaaside elastne kogumi arvu vähendada. |
| 40877 | EX_USER | Elastne kohapeal on ei saa kustutada, kui see ei sisalda kõik andmebaasid. | Ükski | Elastne rakenduskausta sisaldab ühe või mitme andmebaasid ja seetõttu ei saa kustutada. | Eemaldage andmebaaside elastne pool selleks, et kustutada. |
| 40881 | EX_USER | Elastne rakenduskausta ' %. * ls' jõudnud oma andmebaasi arv piirata.  Elastne rakenduskausta limiit andmebaasi arv ei tohi ületada (%d) on elastne rakenduskausta (% d) DTUs jaoks. | Nime elastne pool; andmebaasi count piirata elastne pool; e DTUs ressursivaru jaoks. | Proovite luua või lisada andmebaasi elastne pool, kui andmebaasi count piirata elastne pool jõudnud. | Palun kaaluge suurendamist DTUs elastne pool võimalusel oma andmebaasi piirata suurendamiseks või elastne rakenduskausta andmebaaside eemaldamine. |
| 40889 | EX_USER | DTUs või elastne rakenduskausta mahupiirang ' %. * ls' ei saa vähendada, kuna, mis ei paku piisavalt ruumi oma andmebaaside jaoks. | Elastne kausta nimi. | Proovitakse vähendada talletuslimiidi elastne pool oma salvestusruumi kasutuse all. | Üksikute andmebaaside elastne kogumi salvestusruumi kasutuse kaaluma või andmebaaside eemaldamine pool oma DTUs või salvestuslimiidi vähendamiseks. |
| 40891 | EX_USER | DTU min andmebaasi (%d) ei tohi ületada DTU max andmebaasi (%d) kohta. | DTU min andmebaasi; DTU max andmebaasi kohta. | Proovite määrata DTU min andmebaasi, mis on suurem kui DTU max andmebaasi kohta. | Veenduge, et DTU min andmebaaside kohta ei ületa DTU max andmebaasi kohta. |
| MÄÄRATLETAKSE HILJEM | EX_USER | Mälumaht üksikute andmebaasi elastne pargis ei tohi ületada max lubatud maht ' %. * ls' teenuse taseme elastne pool. | elastne rakenduskausta teenuse kiht | Andmebaasi maksimaalne maht ületab lubatud elastne rakenduskausta teenuse kiht max maht. | Seadke max maht andmebaasi piires max elastne rakenduskausta teenuse kiht lubatud maht. |

Seotud teemad:

* [Kohapeal (C#) on elastne andmebaasi loomine](sql-database-elastic-pool-create-csharp.md) 
* [Halda kohapeal on elastne andmebaasi (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Kohapeal on elastne andmebaasi (PowerShelli) loomine](sql-database-elastic-pool-create-powershell.md) 
* [Jälgimine ja haldamine elastne andmebaasi kohapeal (PowerShelli) on](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Üldised tõrked

Järgmistest tõrketeadetest ei kuulu eelmise kategooriad.

|Tõrkekood|Raskusaste|Kirjeldus|
|---:|---:|:---|
|15006|16|<AdministratorLogin>ei ole lubatud nimi, kuna see sisaldab sobimatuid märke.|
|18452|14|Sisselogimine nurjus. Login on ebausaldusväärsete domeeni ja ei saa kasutada authentication.% Windows. & #x2a; ls (Windowsi sisselogimise ei toeta seda SQL serveri versioon.)|
|18456|14|Kasutaja sisselogimine nurjus "% & #x2a;ls'.%. & #x2a ls %. & #x2a; ls (Kasutaja login nurjus"% & #x2a; ls". Parooli muutmine nurjus. Parooli muutmine sisselogimisel ei toeta seda SQL serveri versioon.)|
|18470|14|Kasutaja sisselogimine nurjus "% & #x2a; ls". Põhjus: Konto on disabled.% & #x2a; ls|
|40014|16|Mitme andmebaasi ei saa kasutada sama tehingu.|
|40054|16|Tabelite ilma rühmitatud indeks ei toeta seda SQL serveri versioon. Kobartulpdiagrammi loomine ja proovige uuesti.|
|40133|15|Seda toimingut ei toetata selles versioonis SQL serveri.|
|40506|16|Määratud SID ei sobi selle SQL serveri versioon.|
|40507|16|"% & #x2a; ls' ei saa kasutada parameetriga SQL serveri selles versioonis.|
|40508|16|Andmebaaside vaheldumisi kasutamine lause ei toetata. Ühenduse loomine muu andmebaasiga uue ühenduse abil.|
|40510|16|Lause '% & #x2a; ls' selle SQL serveri versioon ei toeta|
|40511|16|Sisseehitatud funktsioon '% & #x2a; ls' selle SQL serveri versioon ei toeta.|
|40512|16|SQL serveri see versioon ei toeta taunitud funktsiooni '%ls'.|
|40513|16|Serveri muutuv '% & #x2a; ls' selle SQL serveri versioon ei toeta.|
|40514|16|SQL serveri see versioon ei toeta '%ls'.|
|40515|16|Andmebaasi ja/või serveri nime viide tekstivormingus '% & #x2a; ls' selle SQL serveri versioon ei toeta.|
|40516|16|Globaalne temp objektid ei toeta seda SQL serveri versioon.|
|40517|16|Märksõna või lause suvand '% & #x2a; ls' selle SQL serveri versioon ei toeta.|
|40518|16|DBCC käsk '% & #x2a; ls' selle SQL serveri versioon ei toeta.|
|40520|16|S_MSG turvatavate class "%" seda SQL serveri versioon ei toeta.|
|40521|16|S_MSG turvatavate class "%" serveri ulatuse SQL serveri selles versioonis ei toetata.|
|40522|16|SQL serveri see versioon ei toeta andmebaasi põhisumma '% & #x2a; ls' tüüp.|
|40523|16|Peidetud kasutaja '% & #x2a; ls' loomine SQL serveri see versioon ei toeta. Kasutaja loomine enne selle kasutamist.|
|40524|16|Andmetüüp '% & #x2a; ls' selle SQL serveri versioon ei toeta.|
|40525|16|KOOS "%.ls" ei toeta seda SQL serveri versioon.|
|40526|16|"% & #x2a; ls' Reahulk pakkuja SQL serveri see versioon ei toeta.|
|40527|16|Lingitud serverid ei toeta seda SQL serveri versioon.|
|40528|16|Kasutajad ei saa vastendada serdid, asümmeetriline võtmed ja Windowsi sisselogimise SQL serveri selles versioonis.|
|40529|16|Sisseehitatud funktsioon '% & #x2a; ls' isikustamine kontekstis on pole toetatud SQL serveri selles versioonis.|
|40532|11|Server ei saa avada "% & #x2a; ls" taotleja login. Login nurjus.|
|40553|16|Seansi on lõpetatud liigse mälukasutuse tõttu. Proovige muuta päringu töötlemiseks vähem ridu.<br/><br/>*Märkus:* Arvu `ORDER BY` ja `GROUP BY` toimingute Transact-SQL-koodi abil vähendada päringu mälu nõuetele.|
|40604|16|Võiks ei loo/Muuda andmebaasi, kuna see ületaks piirmäär serveri.|
|40606|16|SQL serveri see versioon ei toeta manustamise andmebaasid.|
|40607|16|SQL serveri selles versioonis ei toetata Windowsi logimine.|
|40611|16|Serverites võib olla määratletud kõige 128 tulemüüri reeglid.|
|40614|16|Alusta IP-aadress tulemüüri reegel ei tohi ületada End IP-aadress.|
|40615|16|Serveri '{0}' login taotleja ei saa avada. Kliendi IP-aadressiga {1} serverit pole lubatud.  Juurdepääsu lubamiseks kasutada SQL-i andmebaasi portaali või käivitage sp_set_firewall_rule juhtslaidi andmebaasi selle IP-aadress või aadress vahemiku tulemüüri reegli loomiseks.  Võib kuluda kuni viis minutit, et selle muudatuse jõustumiseks.|
|40617|16|Tulemüüri reegli nimi, mis algab <rule name> on liiga pikk. Maksimumpikkus on 128.|
|40618|16|Tulemüüri reegli nimi ei tohi olla tühi.|
|40620|16|Kasutaja login nurjus "% & #x2a; ls". Parooli muutmine nurjus. SQL serveri see versioon ei toeta sisselogimisel parooli muutmine.|
|40627|20|Serveri {0} ja andmebaasi {1} on pooleli. Oodake mõni minut, enne kui proovite uuesti.|
|40630|16|Parooli valideerimine nurjus. Parooli ei vasta poliitika nõuetele, kuna see on liiga lühike.|
|40631|16|Teie määratud parool on liiga pikk. Parooli peaks olema rohkem kui 128 märki.|
|40632|16|Parooli valideerimine nurjus. Parooli ei vasta poliitika nõuetele, kuna seda pole piisavalt keerukas.|
|40636|16|Ei saa kasutada reserveeritud andmebaasi nimi '% & #x2a; ls' selle toiminguga.|
|40638|16|Vigaste tellimuse id < tellimuse id >. Tellimus pole olemas.|
|40639|16|Taotlus ei vasta skeemi: <schema error>.|
|40640|20|Serveris ilmnes ootamatu erand.|
|40641|16|Määratud asukohta ei sobi.|
|40642|17|Server on praegu hõivatud. Proovige hiljem uuesti.|
|40643|16|Määratud x-ms-versiooni päist väärtus ei sobi.|
|40644|14|Lubada juurdepääsu määratud tellimuse nurjus.|
|40645|16|Serverinimi <servername> ei saa olla tühi ega null. Seda saab teha ainult, väiketähti "a"-"y", arvud 0 – 9 ja sidekriipsu. Sidekriipsu võib põhjustada või rada nimi.|
|40646|16|Tellimuse ID ei tohi olla tühi.|
|40647|16|Tellimuse < tellimuse id pole server serverinimi.|
|40648|17|Liiga palju taotlusi viidud. Proovige hiljem uuesti.|
|40649|16|Vigaste sisutüüp on määratud. Toetatakse ainult rakendus-xml.|
|40650|16|Tellimuse < tellimuse id > pole olemas või pole tööks valmis.|
|40651|16|Ei saanud luua server, kuna tellimuse < tellimuse id > on keelatud.|
|40652|16|Ei saa teisaldada ega serveri loomine. Tellimuse < tellimuse id > ületab serveri kvoodi.|
|40671|17|Suhtlus nurjumise lüüsi ja halduse teenuse vahel. Proovige hiljem uuesti.|
|40852|16|Andmebaasi ei saa avada "%. *ls serveris ' %.* ls' taotleja login. Accessi andmebaasi on lubatud ainult turvaline ühendusstringi abil. See andmebaasile juurdepääsu muuta oma ühendusstringi sisaldama turvaline serveri FQDN - 'serveri nimi'.database.windows .net tuleks muuta, et 'serveri nimi'.database. `secure`. windows.net.|
|45168|16|SQL Azure'i süsteemi koormuse, ja paneb ülempiir samaaegseid DB CRUD toimingute ühe server (nt andmebaasi loomine). Määratud tõrketeate server on ületanud maksimaalne arv samaaegseid ühendusi. Proovige hiljem uuesti.|
|45169|16|SQL Azure'i süsteemi koormuse, ja on pannes samaaegseid serveri CRUD toimingute ühe tellimuse arvu ülempiir (nt luua server). Tõrketeate määratud tellimus on ületanud maksimaalne arv samaaegseid ühendusi ja kutse on keelatud. Proovige hiljem uuesti.|

## <a name="related-links"></a>Seotud lingid

- [Juhised ja SQL Azure'i andmebaasi üldine piirangud](sql-database-general-limitations.md)
- [Azure'i SQL-andmebaasi ressursi piirangud](sql-database-resource-limits.md)
