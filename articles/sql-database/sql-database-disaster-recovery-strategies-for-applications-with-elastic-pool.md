<properties 
   pageTitle="Kujundamise pilve lahendusi tõrkejärgseks SQL-i andmebaasi Geo-kopeerimise abil | Microsoft Azure'i"
   description="Saate teada, kuidas kujundada oma pilve lahendus tõrkejärgseks, valides õige Tõrkesiirde mustri."
   services="sql-database"
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Katastroofi taastamine rakendustes, mis kasutavad SQL-i andmebaasi elastne rakenduskausta strateegiad 

Oleme õppinud pilveteenustega ei ole lollikindel ja katastroofiline juhtumite aastate saate ja juhtub. SQL-andmebaasi pakub mitmesuguseid võimalusi ette rakenduse business järjepidevuse, kui need ilmnevad. [Elastne kaustu](sql-database-elastic-pool.md) ja autonoomse andmebaaside toetada sama liiki funktsioonid. Selles artiklis kirjeldatakse mitut DR strateegiad jaoks elastne ühendab, et ära kasutada nende SQL-andmebaasi äri järjepidevus funktsioonid.

Selles artiklis me kasutame kanoonilise SaaS ISV rakenduse mustri:

<i>Tänapäevane cloud vastavalt web rakenduse sätteid ühe SQL-andmebaasi iga lõppkasutaja jaoks. Kontrollimissertifikaadid on suur hulk kliendid ja seetõttu kasutab palju andmebaase, nimetatakse rentniku andmebaasid. Kuna rentniku andmebaasid on tavaliselt ettearvamatuid tegevuse mustrite, kasutab Kontrollimissertifikaadid elastne kohapeal on teha maksumus andmebaasi väga hõlpsalt prognoosida laiendatud aja jooksul. Elastne rakenduskausta lihtsustab ka jõudluse haldamine, kui kasutaja tegevuste naelu. Lisaks rentniku andmebaaside rakendus kasutab ka andmebaasidest turvalisus kasutajaprofiilide haldamine, koguda mustreid jne. Üksikute rentnikud kättesaadavus ei mõjuta rakenduse kättesaadavus kogu. Siiski kättesaadavust ja jõudluse haldamise andmebaasid on kriitiline rakenduse funktsiooni ja kui halduse andmebaasid on ühenduseta kogu rakenduse on ühenduseta.</i>  

Ülejäänud dokumendis me arutada DR strateegiad, mis hõlmab maksumus tundliku käivitus rakendustest ranged-saadavus nõuetele need stsenaariumid hulka.  

## <a name="scenario-1-cost-sensitive-startup"></a>Stsenaarium 1. Tundliku loomuga käivitus maksumus

<i>Olen käivitus business ja põhjus väga maksumus tundliku sisuga.  Soovin lihtsustada juurutus- ja rakenduse ja ma olen valmis on piiratud SLA üksikute klientide. Kuid tahan tagada rakenduse tervikuna on kunagi ühenduseta.</i>

Lihtne tingimusele vastavaks tuleks juurutada kõigi rentniku andmebaaside sisse ühe elastne pool oma valik Azure piirkonna ja juurutamine halduse andmebaasi nimega autonoomse geo tiražeeritud andmebaasi. Kasutage rentnikega tõrkejärgseks geo taastada, mis sisaldab täiendava tasuta. Andmebaaside haldamine kättesaadavuse tagamiseks peaks olema geo kopeeritud teises regioonis (samm 1). Poolelioleva kulu katastroofi taastamine konfiguratsiooni sel on võrdne teisene andmebaasi kogukulu. Selle konfiguratsiooni on kujutatud järgmisel diagrammil.

![Joonis 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Korral on katkestuste esmane piirkonna, illustreerivad taastamise juhiseid rakenduse võrgus tuua järgmine skeem.

- Kohe Tõrkesiirde haldamise andmebaasid (2) alale DR. 
- Muuda selle rakenduse ühendusstringi osutamiseks DR piirkond. Kõik uued kontod ja rentniku andmebaaside luuakse ala DR. Olemasolevad kliendid kuvatakse nende andmete ajutiselt saadaval.
- Looge elastne rakenduskausta sama konfiguratsiooni nimega algse rakenduskausta (3). 
- Geo-taastamine abil saate luua koopiate rentniku andmebaaside (4). Saate kaaluge käivitamise üksikute taastab lõppkasutaja ühendusi või kasutage mõnda muud rakenduse konkreetsete värviskeemi.

Selles etapis on rakenduse võrguühenduse taastamisel DR piirkonna, kuid mõned kliendid kogemus viivitus kui andmetega.

![Joonis 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Kui soovitud katkestuste ajutine, on võimalik esmane piirkond taastub, Azure'i enne kõigi taastab on lõpetatud DR piirkonna. Sel juhul tuleks korraldab teisaldamine rakenduse esmane piirkonda. Toiming võtab Järgmine diagramm näitab juhiseid.
 
- Kõik tasumata geo-taastamine taotlused tühistada.   
- Tõrkesiirde halduse andmebaasi esmane alale (5). Märkus: Pärast piirkonna taastamise vana primaries automaatselt muutunud sekundaaride. Nüüd need lülitub rollid uuesti. 
- Muuda selle rakenduse ühendusstringi osutama esmane piirkonda. Nüüd kõik uued kontod ja rentniku andmebaaside luuakse esmane piirkonna. Mõned olemasolevad kliendid kuvatakse nende andmete ajutiselt saadaval.   
- Saate seada kõik andmebaasid DR pool, ainult lugemiseks tagamaks, et nad ei saa muuta DR piirkonnas (6). 
- Iga andmebaasi pärast taastamis muutunud DR kogumi, Nimeta ümber või Kustuta vastavate andmebaaside esmane kogumi (7). 
- Kopeerige värskendatud andmebaaside DR rakenduskausta esmane pool (8). 
- Kustutage DR rakenduskausta (9)

Selles etapis saab rakenduse esmane piirkonna kõik rentniku andmebaasid saadaval esmane pool online.

![Joonis 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Funktsiooni klahv **kasulik** strateegia on vähese poolelioleva hind andmete taseme koondamise. Varukoopiate võetakse automaatselt SQL-andmebaasi teenus pole rakenduse ümberkirjutamine ja täiendava tasuta.  Maksumus tekib ainult siis, kui elastne andmebaasid on taastatud. **Kompromiss** on see, et kõigi rentniku andmebaaside täieliku taastamise võtab oluliselt aega. See sõltub kogusumma taastab annate DR piirkonna arv ja üldist suurus rentniku andmebaasid. Isegi juhul, kui te tähtsuse mõned rentnike jaoks taastab teiste, osalevad saate koos kõigi muude taastab, mis alustatakse sama piirkonna, kui teenus vahekohtunikuks ja throttle olemasolevad kliendid andmebaaside üldine mõju minimeerimiseks. Lisaks rentniku andmebaaside taastamine ei saa käivitada enne, kui luuakse uus elastne rakenduskausta DR piirkonna.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Stsenaarium 2. Astmeline teenusega küps rakendus 

<i>Ma olen küps SaaS rakenduse astmeline teenuse pakkumised ja muu SLAs prooviversiooni kliendid ja tellijad maksma. Prooviversiooni klientidele, mul on võimalikult palju kuidagi vähendada. Prooviversiooni kliendid saavad võtta tööseisakute, kuid soovin vähendada selle tõenäosus. Intressi klientidele, mis tahes on lendude riski. Seega Soovin veenduge, et kliendid on alati pääsevad oma andmetele juurde.</i> 

Selle stsenaariumi toetamiseks peaks eraldi prooviversiooni rentnikud alates makstud rentnikud paigutades neid eraldi elastne kaustadesse. Prooviversiooni kliendid peavad alumise eDTU kohta rentniku ja alumise SLA taastamine rohkem aega. Pöörates klientidele oleks pargis koos kõrgema eDTU rentniku ja kõrgema SLA kohta. Selleks, et tagada madalaimate taastamise aeg, tuleks pöörates klientidele rentniku andmebaaside geo kopeeritud. Selle konfiguratsiooni on kujutatud järgmisel diagrammil. 

![Joonis 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Nagu esimese puhul, on andmebaasi haldamine üsna aktiivne, et autonoomse geo tiražeeritud andmebaasi kasutada seda (1). See tagab prognoositavad jõudlus uue kliendi tellimused, profiili värskendused ja muude toimingute jaoks. Andmebaasi halduse primaries asuma piirkonnas on esmane piirkond ja piirkonna asuma sekundaaride halduse andmebaasi, saab DR piirkond.

Pöörates klientidele rentniku andmebaasid on aktiivne andmebaaside "makstud" pool esmane piirkonna ette valmistatud. Tuleks teisel pool sama nimega DR piirkonna ette valmistada. Iga rentniku oleks geo kopeeritud teisel pool (2). See võimaldab kõigi rentniku andmebaaside abil Tõrkesiirde kiireks taastamiseks. 

Mõne katkestuste juhul esmane piirkonna taastamise juhiseid rakenduse võrgus tuua on näidatud järgmisel joonisel:

![Joonis 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Kohe ei õnnestu üle halduse andmebaasi DR alale (3).
- Rakenduse ühendusstringi osutamiseks DR piirkonna muutmine Nüüd kõik uued kontod ja rentniku andmebaaside luuakse DR piirkonna. Olemasolevate prooviversiooni klientide kuvatakse nende andmete ajutiselt saadaval.
- Tõrkesiirde makstud rentniku andmebaaside pool DR piirkonna taastamiseks kohe tema kättesaadavus (4). Kuna selle Tõrkesiirde on kiire metaandmete taseme muutmine võite kaaluda on optimeerimise, kus üksikute failovers käivitatakse nõudmisel lõppkasutaja ühendused. 
- Kui oma teisel pool eDTU suurus oli esmane väiksem, kuna teisene andmebaaside nõutav ainult võimalik töödelda logid muutmine, kui need olid sekundaaride, mida peaks kohe suurendama rakenduskausta nüüd mahutamiseks täielik töökoormus kõik rentnikega (5). 
- Saate luua uue elastne rakenduskausta sama nime ja sama konfiguratsiooni DR piirkonna prooviversiooni klientide andmebaaside (6). 
- Kui prooviversiooni kliendid rakenduskausta on loodud, kasutage geo-taastamine taastamiseks üksikute prooviversiooni rentniku andmebaaside uue kausta (7). Saate kaaluge käivitamise üksikute taastab lõppkasutaja ühendusi või kasutage mõnda muud rakenduse konkreetsete värviskeemi.

Selles etapis rakenduse on võrguühenduse taastamisel DR piirkond. Kõik intressi kliendid pääsevad oma andmetele ajal prooviversiooni klientide kogemus viivitus kui andmetega.

Kui esmane regioon on taastatud Azure *pärast* , saate jätkata selle piirkonna rakendus töötab DR piirkonna rakenduse taastatud või saate otsustada, esmane piirkonda nurjumise. Kui esmane regioon on taastatud *enne* Tõrkesiirde protsess on lõpetatud, võiksite kaaluda puudumisel tagasi kohe. Funktsiooni failback võtab juhiseid, mis on näidatud järgmisel joonisel: 
 
![Joonis 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Kõik tasumata geo-taastamine taotlused tühistada.   
- Tõrkesiirde halduse andmebaasi (8). Pärast piirkonna taastamise automaatselt muutunud vana primaarne teisese. Nüüd saab esmast uuesti.  
- Tõrkesiirde makstud rentniku andmebaasid (9). Samuti pärast piirkonna taastamise vana primaries muutuvad automaatselt selle sekundaaride. Nüüd, kui need muutuvad primaries uuesti. 
- Määrake taastatud prooviversiooni andmebaase, mis on muutunud DR piirkonna kirjutuskaitstud (10).
- Iga andmebaasi muutunud taastamis prooviversiooni kliendid DR kogumi, ümber nimetada või kustutada vastava andmebaasi prooviversiooni klientide esmane kogumi (11). 
- Kopeerige värskendatud andmebaaside DR rakenduskausta esmane pool (12). 
- Kustutage DR rakenduskausta (13) 

> [AZURE.NOTE] Tõrkesiirde toiming on asünkroonne. Minimeerimiseks on oluline rentniku andmebaaside Tõrkesiirde käsu täitmine pakettidena vähemalt 20 andmebaaside taastamise aeg. 

Funktsiooni klahv **kasulik** strateegia on see kõrgeim SLA ette pöörates klientidele. Tagatakse ka, et uute katsete arv on blokeering niipea, kui luuakse prooviversiooni DR pool. **Kompromiss** on selle setup suureneb kogumaksumuse rentniku andmebaaside maksumus DR teisel pool makstud klientidele. Lisaks kui teisel pool on erineva suuruse, pöörates klientidele on jõudlus alumise pärast Tõrkesiirde rakenduskausta versioonitäienduse DR piirkonna lõpetamiseni. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Stsenaarium 3. Astmeline teenusega geograafiliselt jaotatud rakendus

<i>Mul on küps SaaS rakenduse astmeline teenus pakub. Soovin pakkuda väga agressiivne SLA minu makstud kliendid ja minimeerida mõju katkestuste ilmnemisel kuna isegi lühike katkestamine, võivad põhjustada klientide rahulolematust. On oluline, et pöörates klientidele alati pääsevad oma andmetele. Katsete on tasuta ja SLA prooviperioodil pole saadaval.</i> 

Toeta seda stsenaariumi, peaks teil olema elastne kolmest eraldi. Kahe võrdse suurusega, mille ääres kõrge eDTUs andmebaasi kohta peaks sisaldama makstud kliendid rentniku andmebaaside kahe eri piirkondades ette valmistada. Kolmas pool prooviversiooni rentnikke sisaldavate oleks alumise eDTUs andmebaasi kohta ja ühel kahe piirkonna ette valmistada.

Selleks, et tagada madalaimate taastamise ajal katkestuste pöörates klientidele rentniku andmebaaside peaks olema geo paljundada 50% iga mõlema piirkonna esmane andmebaasid. Samuti on iga piirkonna teisene andmebaaside 50%. Sellisel viisil kui piirkonnas on ühenduseta ainult 50% makstud klientide andmebaaside oleks mõjutab ja oleks Tõrkesiirde. Muid andmebaase jäävad samaks. Selle konfiguratsiooni on kujutatud järgmisel joonisel:

![Joonis 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Eelmise stsenaariumid, nagu halduse andmebaasi on üsna aktiivne nii, et te peaksite konfigureerida neid eraldi geo tiražeeritud andmebaasi (1). See tagab uue kliendi tellimused, profiili värskendused ja muude toimingute prognoositavad jõudlus. Piirkond A oleks halduse andmebaasi esmane piirkonna ja regiooni B kasutatakse halduse andmebaasi taastamine.

Pöörates klientidele rentniku andmebaaside saab ka geo kopeeritud, kuid primaries ja sekundaaride jagatud piirkond A ja B (2) piirkond. Sellisel viisil rentniku esmane andmebaaside mõjutab selle katkestuste saate soovitud alale Tõrkesiirde ja muutuvad kättesaadavaks. Üldse mitte mõjutada rentniku andmebaaside teine pool. 

Järgmine diagramm näitab taastamise mida ette võtta juhul on katkestuste piirkonnas A.

![Joonis 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Kohe ei õnnestu üle haldamise andmebaasid alale B (3).
- Saate muuta rakenduse ühendusstringi osutamiseks halduse andmebaasi piirkonnas veendumaks, et uued kontod ja rentniku andmebaaside luuakse ala B ja olemasolevat rentniku andmebaaside leitakse olemas ka B. Muuda andmebaasi haldamine. Olemasolevate prooviversiooni klientide kuvatakse nende andmete ajutiselt saadaval.
- Tõrkesiirde makstud rentniku andmebaase pool 2 piirkonna B kohe taastada nende kättesaadavus (4). Kuna selle Tõrkesiirde on kiire metaandmete taseme muutmine võite kaaluda on optimeerimise, kus üksikute failovers käivitatakse nõudmisel lõppkasutaja ühendused. 
- Kuna nüüd pool 2 sisaldab ainult esmane andmebaaside kogumi kokku töökoormus suureneb nii, et te peaksite kohe suurendamine oma eDTU (5). 
- Saate luua uue elastne rakenduskausta sama nime ja sama konfiguratsiooni piirkonna B prooviversiooni klientide andmebaaside (6). 
- Pärast kausta loomist kasutada geo-taastamine üksikute prooviversiooni rentniku andmebaasi taastamiseks rakenduskausta (7). Saate kaaluge käivitamise üksikute taastab lõppkasutaja ühendusi või kasutage mõnda muud rakenduse konkreetsete värviskeemi.


> [AZURE.NOTE] Tõrkesiirde toiming on asünkroonne. Minimeerimiseks on oluline rentniku andmebaaside Tõrkesiirde käsu täitmine pakettidena vähemalt 20 andmebaaside taastamise aeg. 

Selles etapis rakenduse on võrguühenduse taastamisel regioonis B. Kõik intressi kliendid pääsevad oma andmetele ajal prooviversiooni klientide kogemus viivitus kui andmetega.

Piirkonna A on taastatud peate otsustada, kui soovite kasutada piirkond B prooviversiooni kliendid või failback kasutamise katse kliendid rakenduskausta piirkonnas A. Üks kriteerium võib olla prooviversiooni rentniku andmebaaside muudetud pärast taastamis %. Sõltumata sellest, et otsus peate tasakaalustamiseks uuesti makstud rentnikud vahel kaks kaustu. Järgmine diagramm näitab protsessi kui prooviversiooni rentniku andmebaasi ei õnnestu tagasi piirkond A.  
 
![Joonis 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Tühistage kõigi tasumata geo-taastamine taotluste prooviversiooni DR pool.   
- Tõrkesiirde halduse andmebaasi (8). Pärast piirkonna taastamise, oli vana primaarne automaatselt sai teisese. Nüüd saab esmast uuesti.  
- Valige mis makstud rentniku andmebaasi ei õnnestu tagasi pool 1 ja algatada Tõrkesiirde oma sekundaaride (9). Pärast piirkonna taastamise kõik andmebaasid rakenduskausta 1 sai automaatselt sekundaaride. Nüüd 50% muutub primaries uuesti. 
- Pool 2, et algne eDTU (10) mahu vähendamine.
- Sea kõik taastatud prooviversiooni andmebaaside piirkonna B kirjutuskaitstud ainult (11).
- Iga katse DR kogumi, mis on muutunud taastamis andmebaasi ümbernimetamine või kustutamine vastava andmebaasi prooviversiooni esmane kogumi (12). 
- Kopeerige värskendatud andmebaaside DR rakenduskausta esmane pool (13). 
- Kustutage DR rakenduskausta (14) 

Selle strateegia peamised **eelised** on:

- See toetab kõige agressiivne SLA intressi klientidele, kuna see tagab, et ei saa mõne katkestuste mõju rohkem kui 50% rentniku andmebaaside. 
- Tagatakse, et uute katsete arv on blokeering niipea, kui selle taastamisel on loodud rada DR pool. 
- See lubab tõhusamalt kasutada pargi maht olema väiksem aktiivne siis esmane andmebaasid on tagatud teisene andmebaaside pool 1 ja pool 2 50%.

Peamised **kompromisse** on:

- CRUD toimingute suhtes halduse andmebaasi on ühendatud piirkond A kui ühendatud alale B, nagu need käivitatakse suhtes halduse andmebaasi esmane lõppkasutajatele lõppkasutajatele alumise latentsus.
- Selleks on vaja keerukamaid halduse andmebaasi kujundust. Näiteks oleks iga rentniku kirje on asukoha silt, mis on vaja Tõrkesiirde ja failback ajal muuta.  
- Pöörates klientidele võib jõudlus olla väiksem tavalisest kuni pool versioonitäienduse piirkonnas B on lõpule viidud. 

## <a name="summary"></a>Kokkuvõte

See artikkel keskendub andmebaasi taseme, SaaS ISV mitme rentniku rakendus kasutab katastroofi taastamine strateegiad. Valitav strateegia põhinema vajadustele rakendus nagu business mudeli, SLA, mida soovite pakkuda oma klientidele, eelarve piirang jne … Iga kirjeldatud strateegia esitatakse eelised ja kompromiss nii, et teil otsus. Teie konkreetse rakenduse tõenäoliselt sisaldab ka muud Azure komponendid. Nii, et peaksite nende business järjepidevuse juhised läbi vaadata ja korraldab taastamise andmebaasi taseme nendega. Taastamine Azure'i andmebaasi rakenduste haldamise kohta lisateabe saamiseks vaadake [kujundamisel pilve lahendusi tõrkejärgseks](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Järgmised sammud

- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Äri järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [äri järjepidevuse ülevaade](sql-database-business-continuity.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md)
