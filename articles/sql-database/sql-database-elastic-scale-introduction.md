<properties
    pageTitle="Azure'i SQL-andmebaasi skaleerimise välja | Microsoft Azure'i"
    description="Service (SaaS) arendajate tarkvara saate hõlpsasti luua elastne, scalable andmebaaside nende tööriistade abil pilves"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Skaala läbi Azure SQL-i andmebaasiga

Muudate saate hõlpsalt SQL Azure'i andmebaasid **Elastne andmebaasi** tööriistade abil. Nende tööriistade ja funktsioonide abil saate andmebaasi praktiliselt piiramatu ressursse **Azure'i SQL-andmebaasi** abil saate luua lahendused selgituseks töökoormus, ja eriti tarkvara on teenuserakenduste (SaaS). Elastne andmebaasifunktsioone koosneb järgmistest:

* [Elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md): kliendi teek on funktsioon, mis võimaldab teil luua ja hallata sharded andmebaasid.  Vt [Alustamine elastne Andmebaasiriistad](sql-database-elastic-scale-get-started.md).
* [Elastne andmebaasiriista tükeldatud ühendamine](sql-database-elastic-scale-overview-split-and-merge.md): liikumisel andmete sharded andmebaasid. See on abiks andmete teisaldamine mitme rentniku andmebaasist ühe – rentniku andmebaasi (või vastupidi). Lugege teemat [elastne andmebaasi tükeldamine ühendamine tööriista õpetuse](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Elastne andmebaasi tööde haldamine](sql-database-elastic-jobs-overview.md) (eelvaade): töö abil suure hulga Azure SQL-i andmebaaside haldamine. Hõlpsalt haldus toiminguid nagu skeemi muudatusi, identimisteabe haldamine, viide andmete värskenduste, jõudluse andmete kogumine või rentniku (klient) telemeetria saidikogumi kasutades tööd teha.
* [Elastne andmebaasi päring](sql-database-elastic-query-overview.md) (eelvaade): võimaldab teil Transact-SQL-i päringut, mis ulatub üle mitme andmebaasid. See võimaldab ühenduse aruandlustööriistu nagu Exceli, PowerBI sellele jne.
* [Elastne tehingud](sql-database-elastic-transactions-overview.md): See funktsioon võimaldab käivitada tehingud ulatuvad andmebaasidest Azure SQL-andmebaasis. Elastne andmebaasitoiminguid ADO .net-i kasutades .net-i rakendustele on saadaval ning integreerida tuttavad programmeerimise kogemus, kasutades [System.Transaction tunnid](https://msdn.microsoft.com/library/system.transactions.aspx).

Allpool pilt kuvatakse arhitektuur, mis sisaldab **elastne andmebaasifunktsioone** seoses andmebaaside kogum.

Selle pildi värvide andmebaasi tähistavad skeemid. Andmebaaside sama värvi sama skeemi ühiskasutusse anda.

1. **Azure SQL** andmebaase, on majutatud veebisaidil Azure'i sharding arhitektuur abil.
2. **Elastne andmebaasi kliendi teegi** abil saate hallata Kildu kogum.
3. Andmebaaside Alamhulk on **elastne andmebaasi pool**pannakse. (Vt [mis on?](sql-database-elastic-pool.md)).
4. On **elastne andmebaasi töö** töötab ajastatud või erakorralist T-SQL skriptid vastu kõik andmebaasid.
5. **Tükeldatud ühendamine tööriista** kasutatakse andmete üks Kildu teise teisaldada.
6. **Elastne andmebaasi päring** võimaldab kirjutada päring, mis hõlmavad kõik andmebaasid Kildu määramine.
7. **Elastne tehingud** võimaldab käivitada tehingud ulatuvad andmebaasidest. 


![Elastne Andmebaasiriistad][1]


## <a name="why-use-the-tools"></a>Miks kasutada tööriistu?

Saavutada elastsuse ja skaala pilv rakendusi on lihtne VMs ja bloobimälu--lihtsalt lisada lahutamine või suurendada power. Kuid see on jäänud probleemiks stateful andmete töötlemiseks relatsioonandmebaasidest. Probleeme tekkis need stsenaariumid:

* Kasvab ja kahanemine võimsus relatsiooniandmebaasist osa oma töökoormus.
* Haldamise pääsupunktidega, mis võivad tekkida, mis mõjutavad teatud andmete alamkogumit, – näiteks eriti kiire lõpp-kliendi (rentnik).

Traditsiooniliselt stsenaariumid, nagu need on adresseeritud investeerida suuremas ulatuses andmebaasi serverid rakenduse toetamiseks. Kuid see suvand on piiratud pilves, kus kõik töötlemine juhtub eelmääratletud kaup riistvara. Selle asemel levitada andmeid ja töötlemise üle mitme ühtemoodi struktureeritud andmebaaside pakub (skaala-out mustri tuntud "sharding") asemel traditsiooniline skaala üles lähenemisel nii maksumuse ja elastsuse.

## <a name="horizontal-and-vertical-scaling"></a>Horisontaal- ja mastaapimine

Alloleval joonisel horisontaalse ja vertikaalse mõõtmed mastaapimist, mis on elastne andmebaasid saate mastaabitud põhilised viisid.

![Horisontaalne ja vertikaalne Scaleout][2]

Horisontaalne mastaapimine viitab lisamise või eemaldamise andmebaaside võimsuse või üldise jõudluse kohandamiseks. Seda nimetatakse ka "mastaapimine". Sharding, kus on liigendatud andmete kogum ühtemoodi struktureeritud andmebaaside, pole levinud horisontaalne mastaapimine rakendada.  

Vertikaalne skaleerimist viitab suureneb või väheneb jõudlusega üksikute andmebaasi – seda nimetatakse ka "ülespoole."

Enamik cloud-skaala andmebaasirakendusi kasutatakse lõpuks kombinatsiooni. Näiteks nimega teenuserakenduse tarkvara abil horisontaalne mastaapimine ettevalmistamise uute lõppkasutajate ja vertikaalne mastaapimist luba iga end kliendi andmebaasi kasvata või Kahanda ressursid vajadusel töökoormus.

* Horisontaalne mastaapimine hallatakse [elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md).

* Vertikaalne skaleerimist saab teha teenuse kiht, muuta Azure PowerShelli cmdlet-käskude abil või paigutades andmebaaside kohapeal on elastne.

## <a name="sharding"></a>Sharding

*Sharding* on tehnika levitamine suurte andmehulkade ühtemoodi liigendatud kogu sõltumatu andmebaaside arv. See on eriti populaarne cloud arendajad luua tarkvara Service (SAAS) pakkumisi end klientide või ettevõtete jaoks. Nende klientidele nimetatakse sageli "rentnikud". Sharding võidakse nõuda mis tahes mitmel põhjusel:  

* Andmete kogusumma on liiga suur, et see mahuks ühe andmebaasi piiranguid
* Läbilaskevõimet üldine töökoormus ületab ühe andmebaasi võimaluste
* Rentnike jaoks võib olla vaja füüsilise eraldi üksteisest, nii, et iga rentniku jaoks on vaja eraldi andmebaasid
* Andmebaasi erinevate jaotiste võib tekkida vajadus asu erinevates geograafilistes vastavuse, jõudluse või geopoliitiliste põhjustel.

Teiste stsenaariumide andmete jaotatud seadmed, näiteks saab sharding andmebaase, mis on korraldatud ajutiselt täita. Näiteks saate iga päev või nädala töötama eraldi andmebaasi. Sel juhul sharding võti võib olla täisarv, mis tähistab kuupäeva (sharded tabeli kõigi ridade kohal) ja päringute teavet ajavahemiku toomine peab marsruudib rakenduse alamkogumile andmebaaside kõnealuse hulka.

Sharding toimib kõige paremini, kui iga tehingu rakenduses võib olla piiratud sharding klahvi ühe väärtuse. Mis tagab, et kõik tehingud kohaliku kindla andmebaasi.

## <a name="multi-tenant-and-single-tenant"></a>Mitme rentniku ja ühe – rentniku

Mõned rakendused kasutada iga rentniku jaoks eraldi andmebaasi loomise lihtsaim meetodit. See on **ühe rentniku sharding mustri** , mis pakub eraldamise, varundus ja taaste võimalus ja ressursside rentniku granulaarsus eemaldamine. Ühe rentniku sharding, iga andmebaas on seostatud teatud rentniku ID väärtus (või kliendi võtmeväärtuse), kuid selle klahvi ei pea alati olema kohal ise andmeid. On sobiv andmebaasi – iga taotluse marsruutimiseks rakenduse ülesanne ja kliendi teek, saate seda lihtsustada.

![Ühe või mitme rentniku rentniku][4]

Teiste stsenaariumide pakkida mitme rentniku andmebaaside, mitte eemaldada need eraldi andmebaasi. See on tüüpilised **mitme rentniku sharding muster** - ja see võib olla tingitud asjaolu, et rakendus haldab suure hulga väga väike rentnikke. Mitme rentniku sharding, andmebaasi tabelite read on kõik kavandatud läbi mõne tuvastamise rentniku ID või sharding klahvi. Uuesti rakenduse taseme vastutab marsruutimine on rentniku taotluse vastav andmebaas ja seda saab toetada elastne andmebaasi kliendi teek. Lisaks saab rea-turvalisuse filter, millised read iga rentniku pääsete juurde – lisateabe saamiseks vt [mitme rentniku rakendused elastne Andmebaasiriistad ja rea taseme turvalisus](sql-database-elastic-tools-multi-tenant-row-level-security.md). Andmebaaside andmete levitamine võib olla vajalik mitme rentniku sharding mustriga ja seda hõlbustavad elastne tükeldatud ühendamine andmebaasiriista. Kujundus mustrite abil elastne kaustu SaaS rakenduste kohta leiate lisateavet teemast [Mitme rentniku SaaS rakenduste Azure'i SQL-andmebaasi kujunduse mustrid](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Teisaldage andmed mitme ühe-rendiõigus andmebaasidele

SaaS rakenduse loomisel on tüüpilised pakkuda potentsiaalsetele klientidele prooviversiooni tarkvara. Sel juhul on kulutõhus, mitme rentniku andmebaasi andmete jaoks kasutama. Aga kui selle saab klient, ühe – rentniku andmebaasi on parem kuna see pakub parema jõudluse. Kui kliendi loonud andmete prooviperioodil, [tükeldatud ühendamine tööriista](sql-database-elastic-scale-overview-split-and-merge.md) abil mitme rentniku andmete teisaldamine ühe – rentniku uue andmebaasi.

## <a name="next-steps"></a>Järgmised sammud

Valimi rakenduse, mis näitab kliendi teek, lugege teemat [Alustamine elastne Datababase tööriistad](sql-database-elastic-scale-get-started.md).

Teisendada tööriistu saate kasutada olemasolevaid andmebaase, lugege teemat [migreerimine olemasolevaid andmebaase skaala välja](sql-database-elastic-convert-to-use-elastic-tools.md).

Üksikasjad elastne andmebaasi kogumi kuvamiseks [hinna ja jõudluse kaalutluste kohta elastne andmebaasi kohapeal on](sql-database-elastic-pool-guidance.md)näha või luua uue kausta [õpetuse](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

