<properties
    pageTitle="Azure'i SQL-andmebaasi elastne andmebaasi päringu ülevaade | Microsoft Azure'i"
    description="Funktsiooni elastne päringu ülevaade"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure'i SQL-andmebaasi elastne andmebaasi päringu ülevaade (eelvaade)

Elastne andmebaasi päringu funktsiooni (eelvaade) võimaldab teil Transact-SQL-i päringut, mis ulatub üle mitme andmebaasi Azure SQL-i andmebaasi (SQLDB). See võimaldab teil teha rist-andmebaasipäringud tabelite serveri juurdepääsu ja ühenduse Microsofti ja kolmanda osapoole tööriistu (Excel, PowerBI, sellele jne) päringu üle mitme andmebaasidega andmete astme. Selle funktsiooni abil saate päringuid suurte andmete astme SQL-andmebaasis välja mastaapimiseks ja business intelligence (BI) aruannete tulemused visualiseerida.

## <a name="documentation"></a>Dokumentatsioon

* [Rist-andmebaasipäringud kasutamise alustamine](sql-database-elastic-query-getting-started-vertical.md)
* [Aruande üle mastaabitud kontorist pilve andmebaasid](sql-database-elastic-query-getting-started.md)
* [Päringu tegemine sharded cloud andmebaasid (horisontaalselt liigendatud)](sql-database-elastic-query-horizontal-partitioning.md)
* [Päringu tegemine cloud andmebaaside koos eri skeemide (vertikaalselt liigendatud)](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_käivitada \_remote](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Miks kasutada elastne päringud?

**Azure'i SQL-andmebaas**

Päringu SQL Azure'i andmebaasid täielikult T-SQL-i üle. See võimaldab remote andmebaaside päringute kirjutuskaitstud. See pakub suvandit praegune kohapealse SQL serveri klientide migreerida rakenduste abil kolme ja four osalise nimed või lingitud serveri SQL DB.

**Saadaval standard teise kohta** Elastne päring on toetatud jõudlust Standard taseme Lisaks Premium jõudluse taseme. Jõudluse piirangud määras jõudluse kohta leiate jaotisest eelvaade piirangutest all.

**Serveri andmebaasiga push**

Nüüd saate elastne päringute push SQL-i parameetrite remote andmebaaside täitmiseks.

**Salvestatud toimingu teostamine**

Salvestatud kaugprotseduurikutse kõned või remote funktsioonide abil [sp\_käivitada \_remote](https://msdn.microsoft.com/library/mt703714).

**Paindlikkus**

Välise tabeleid elastne päringu viidata saate nüüd remote tabelite eri skeemi või tabeli nimi.

## <a name="elastic-database-query-scenarios"></a>Elastne andmebaasi päringu stsenaariumid

Eesmärk on hõlbustada päringu stsenaariumid, kui mitu andmebaaside kaasa ridade ühe üldise tulemi. Päring võib sisaldada kas kasutaja või rakendus otseselt või kaudselt tööriistu, mis on seotud andmebaasi. See on eriti kasulik, kui luua aruandeid, trükikojas BI või andmete kataloogiintegreerimise tööriistad – või mis tahes rakendus, mis ei saa muuta. Elastne Queryga, saate teha päringuid üle tuttavad SQL serveri Ühenduvus kogemus kasutamine Excelis, PowerBI, sellele või Cognos näiteks andmebaasidest.
Elastne päring võimaldab kogu kogumise andmebaaside päringute SQL Server Management Studio või Visual Studio kaudu hõlpsalt juurde ja hõlbustab risti andmebaasi päringute üksuse raames või muude ORM keskkonnas. Joonis 1 näitab stsenaarium, kus pilveteenuse taotluse (mis kasutab [elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md)), mis põhineb mastaabitud välja andmete taseme ja elastne päring kasutatakse risti andmebaasi aruannete jaoks.

**Joonis 1** Mastaabitud välja andmete taseme kasutatud elastne andmebaasi päring

![Mastaabitud välja andmete taseme kasutatud elastne andmebaasi päring][1]

Kliendi stsenaariumid elastne päringu iseloomustab järgmine topoloogiatest:

* **Vertikaalne eraldamine – rist andmebaasipäringud** (Topoloogia 1): andmed on liigendatud vertikaalselt andmebaaside andmete astme hulga vahel. Tavaliselt erinevaid tabelid asu erinevate andmebaasid. See tähendab, et skeemi erineb eri andmebaasid. Näiteks kõigi tabelite jaoks varude on ühe andmebaasi samas kõik raamatupidamine seotud tabelid on teine andmebaas. Kasutamine levinud olukorda, kus see topoloogia on vaja ühte päringu üle või üle tabelid andmebaasidest aruandeid koostada.
* **Horisontaalne eraldamine – Sharding** (Topoloogia 2): andmed on horisontaalselt liigendatud levitada read üle mastaabitud välja andmete taseme. Seda moodust koos skeemi on identne osalevate kõik andmebaasid. Seda moodust nimetatakse ka "sharding". Sharding saab teha ja hallatavate abil (1) elastne andmebaasi tööriistade teekide või (2) enda loodud sharding. Päringu või koostada aruandeid üle mitme shards kasutatakse elastne päring.

> [AZURE.NOTE] Elastne andmebaasi päringu toimib kõige paremini aeg-ajalt aruandlusteenuste stsenaariumid, kus enamik töötlemise saab teostada andmete taseme. Suur aruandlusteenuste töökoormus või andmete tolliladustamise stsenaariumid keerukamaid päringutega, ka kaaluge [SQL Azure'i andmebaas](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Elastne andmebaasi päringu topoloogiatest

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>Topoloogia 1: Vertikaalne eraldamine – risti andmebaasi päringud

Alustada kodeerimise, lugege teemat [Alustamine risti andmebaasi päringu (vertikaalne eraldamine)](sql-database-elastic-query-getting-started-vertical.md).

Elastne päring saab teha muid andmebaase SQLDB saadaval SQLDB andmebaasi andmeid. See võimaldab päringuid ühe andmebaasist viidata tabelid SQLDB muude kaugandmebaasiga. Kõigepealt tuleb määratleda iga kaugandmebaasiga välise andmeallikaga. Välise andmeallika on määratletud, millest soovite juurde pääseda tabelite serveri andmebaasis asuvate kohalik andmebaas. Muudatusi on vaja serveri andmebaasis. Tüüpilised vertikaalne eraldamine stsenaariumid, kus on erinevate andmebaaside eri skeemide, saab rakendada kasutamine levinud olukorda, nagu viide andmetele juurdepääsu ja risti andmebaasi päringute elastne päringud.

**Lähteandmed**: topoloogia 1 kasutatakse viide andmete haldamine. Kahe tabeli (T1 ja T2) andmeid hoitakse alloleval joonisel asjakohast andmebaasi. Elastne värskenduspäringu nüüd pääsete tabelite T1 ja T2 kaugühenduse teel muid andmebaase, nagu on näidatud joonisel. Kasutage topoloogia 1, kui viite tabelid on väike või kaugandmebaasiga päringute viite tabelisse on valikuline predicates.

**Joonis 2** Vertikaalne eraldamine – elastne päringu päring viide andmete kasutamine

![Vertikaalne eraldamine – elastne päringu päring viide andmete kasutamine][3]

**Risti andmebaasi päringute**: elastne päringute luba kasutamine juhtumeid, mille puhul üle mitme SQLDB andmebaaside päringute. Joonis 3 kuvab erinevate neli: CRM, nimekirja, HR ja tooted. Samuti vajate tehtud andmebaaside päringute juurdepääsu ühe või kõigi muude andmebaasid. Elastne värskenduspäringu saate konfigureerida oma andmebaasi sel juhul käivitades mõne lihtsa DDL laused iga nelja andmebaasid. Pärast selle ühekordse konfiguratsiooni juurdepääsu serveri tabel on sama lihtne kui viidates kohaliku tabeli T-SQL-i päringute või oma Ärianalüüsi tööriistade kohta. Seda moodust on soovitatav kui remote päringud ei tagasta suurte tulemused.

**Joonis 3** Vertikaalne eraldamine – elastne päringu päring üle erinevate andmebaaside kasutamine

![Vertikaalne eraldamine – elastne päringu päring üle erinevate andmebaaside kasutamine][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Topoloogia 2: Horisontaalne eraldamine – sharding

Ülesannete aruannete üle sharded, st, horisontaalselt liigendatud, elastne päringu abil andmete taseme nõuab [elastne andmebaasi Kildu kaardi](sql-database-elastic-scale-shard-map-management.md) tähistada andmete tasandi andmebaasid. Tavaliselt kasutatakse ainult ühe Kildu kaart selle stsenaariumi ja asjakohast andmebaasi elastne päringu võimalustega toimib sisenemiskoha päringute aruannete jaoks. Ainult see sihtotstarbeline andmebaas vajab juurdepääsu Kildu kaarti. Joonis 4 on näidatud selles topoloogia ja konfiguratsioon elastne päringu andmebaas ja Kildu kaart. Andmebaasi andmete astme võib olla mis tahes Azure'i SQL-andmebaasi versiooni või väljaande. Elastne andmebaasi kliendi teek ja Kildu kaartide loomise kohta leiate lisateavet teemast [Kildu vastendamine haldus](sql-database-elastic-scale-shard-map-management.md).

**Joonis 4** Horisontaalne eraldamine – elastne päringu abil üle sharded andmete astme aruannete jaoks

![Horisontaalne eraldamine – elastne päringu abil üle sharded andmete astme aruannete jaoks][5]

> [AZURE.NOTE] Elastne andmebaasi päringu andmebaasi peab olema SQL DB v12 andmebaasi. On mingeid piiranguid shards ise.

Alustada kodeerimise, lugege teemat [Alustamine elastne andmebaasi päringu horisontaalne jagamine (sharding)](sql-database-elastic-query-getting-started.md).

## <a name="implementing-elastic-database-queries"></a>Elastne andmebaasipäringud rakendamine

Järgmistes lõikudes käsitletakse juhiseid rakendada elastne päringu vertikaal- ja eraldamine stsenaariumid. Ka viidata üksikasjalikumat dokumentatsioonist eraldamine stsenaariumit.

### <a name="vertical-partitioning---cross-database-queries"></a>Vertikaalne eraldamine - risti andmebaasi päringud

Järgmised toimingud konfigureerida elastne andmebaasi päringuid vertikaalne eraldamine stsenaariumi, mis asub remote SQLDB andmebaasid sama skeemi tabeli Accessi vaja.

*    [JUHTSLAIDI loomine võti](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Luua andmebaasi VEEBIRAKENDUST MANDAATI](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [Loo/DROP välise ANDMEALLIKA](https://msdn.microsoft.com/library/dn935022.aspx) tüübi **RDBMS** mydatasource
*    [Loo/DROP VÄLISTABEL](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Pärast opsüsteemi DDL laused, pääsete serveri tabeli "mytable", nagu oleks kohaliku tabeli. Azure'i SQL-andmebaasi automaatselt avaneb kaugandmebaas ühenduse, töötleb serveri andmebaasis Teie taotlus ja tagastab väärtuse.
Lisateavet vertikaalne eraldamine stsenaariumi toimingud leiate [elastne päringu vertikaalne jagamine](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Horisontaalne eraldamine - sharding

Järgmised toimingud konfigureerida elastne andmebaasipäringud horisontaalne eraldamine stsenaariumid kogumi tabeli juurdepääsu nõudvate mis asuvad (enamasti) mitme remote SQLDB andmebaasid.

*    [JUHTSLAIDI loomine võti](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Luua andmebaasi VEEBIRAKENDUST MANDAATI](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Oma andmete taseme abil elastne andmebaasi kliendi teek tähistav [Kildu kaardi](sql-database-elastic-scale-shard-map-management.md) loomine.   
*    [Loo/DROP välise ANDMEALLIKA](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource tüüpi **SHARD_MAP_MANAGER**
*    [Loo/DROP VÄLISTABEL](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Kui tegite on neid juhiseid, pääsete horisontaalselt sektsioonitud tabeli "mytable", nagu oleks kohaliku tabeli. Azure'i SQL-andmebaasi automaatselt avaneb mitme paralleelse ühendused remote andmebaasidele, kus talletatakse füüsilise tabelid, töötleb taotlusi remote andmebaasid ja tagastab väärtuse.
Lisateavet horisontaalne eraldamine stsenaariumi toimingud leiate [elastne päringu horisontaalne jagamine](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>T-SQL päring
Kui olete määratlenud oma väliste andmeallikate ja oma välise tabeleid, saate tavalise SQL serveri ühendusstringi ühenduse andmebaaside, kus määratletud oma välise tabeleid. Seejärel käivitada T-SQL-lausete üle oma välise tabeleid selle ühenduse allpool kirjeldatud piirangutega. Leiate lisateavet ja näiteid T-SQL-päringud dokumentatsiooni teemas [eraldamine horisontaalse](sql-database-elastic-query-horizontal-partitioning.md) ja [vertikaalse eraldamine](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Tööriistade Ühenduvus
Tavaline SQL serveri ühendusstringi abil saate oma rakenduste ja andmete või BI kataloogiintegreerimise tööriistad ühenduse välise tabeleid loodud andmebaasi. Veenduge, et teie tööriista andmeallikana on toetatud SQL serveri. Kui ühendus on loodud, vaadake elastne päringu andmebaas ja välise tabeleid andmebaasi täpselt samamoodi, nagu teeksite mõnda SQL Serveri andmebaas, millega loote ühenduse oma tööriistaga.

> [AZURE.IMPORTANT] Azure Active Directory abil elastne päringute autentimine on praegu ei toetata.

## <a name="cost"></a>Maksumus

Elastne päringu, kaasatakse arvesse Azure'i SQL-andmebaasi andmebaasid. Pange tähele, et topoloogiatest, kus teie remote andmebaasid on erinevate andmekeskuse kui elastne päringu lõpp-punkti on toetatud, kuid andmed sealt remote andmebaasidest tuleb maksta regulaarselt [Azure määra](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Eelvaate piirangud
* Esimese elastne päringu käitamise võib kuluda mõni minut jõudlust Standard teise kohta. See aeg on laadima elastne funktsioonide; jõudluse laadimine parandab koos suurema jõudluse astme.
* Väliste andmeallikate või välise tabeleid SSMS või SSDT skriptimise veel ei toetata.
* Import/eksport SQL DB veel ei toeta väliste andmeallikate ja välise tabeleid. Kui peate kasutama impordi/ekspordi, langev neid objekte enne eksportimist ja seejärel uuesti luua neid pärast importimist.
* Elastne andmebaasi päringu praegu toetab ainult kirjutuskaitstud juurdepääsu välise tabeleid. Aga saate täisfunktsionaalsuse T-SQL-i andmebaasi kus välise tabeli on määratletud. See võib olla kasulik, nt ajutiste tulemuste abil, nt ei kao, valige < column_list > < local_table > või salvestatud toimingute määratlemine elastne päringu andmebaasi, mis viitavad välise tabeleid.
* Nvarchar(max), välja arvatud LOB tüübid ei toeta Välistabel määratlusi. Lahendusena saate serveri andmebaasis, mis põhjustab nvarchar(max) LOB tipite vaate loomine, määratlemine väliste tabeli vaate asemel põhiline tabeli üle ja seejärel enamus taas algse LOB Tippige päringu.
* Veeru statistika üle välise tabeleid pole praegu toetatud. Tabelite statistika on toetatud, kuid tuleb luua käsitsi.

## <a name="feedback"></a>Tagasiside
Näidake tagasisidet teie töötamist elastne päringute meiega Disqus allpool MSDN-i foorumites, või Stackoverflow. Oleme huvitatud igasuguseid (defekte, töötlemata servad, funktsioon lünki) teenuse kohta tagasiside.

## <a name="more-information"></a>Lisateave

Leiate lisateavet risti andmebaasi päringu- ja vertikaalne eraldamine stsenaariumid järgmised dokumendid:

* [Risti andmebaasi päringu- ja vertikaalne eraldamine ülevaade](sql-database-elastic-query-vertical-partitioning.md)
* Proovige meie heliprobleeme täielik pilgu järgmisele näitele töötab minutites: [Alustamine risti andmebaasi päringu (vertikaalne eraldamine)](sql-database-elastic-query-getting-started-vertical.md).


Lisateavet horisontaalne eraldamine ja sharding stsenaariumid on saadaval:

* [Horisontaalne eraldamine ja sharding ülevaade](sql-database-elastic-query-horizontal-partitioning.md)
* Proovige meie heliprobleeme täielik pilgu järgmisele näitele töötab minutites: [Alustamine elastne andmebaasi päringu horisontaalne jagamine (sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
