<properties
   pageTitle="SQL Azure'i andmebaasi Azure juhtumianalüüsi - Daxko/CSI | Microsoft Azure'i"
   description="Teada, kuidas Daxko/CSI kasutab SQL-andmebaasi oma arengutsükli kiirendamiseks ja selle klienditeenindus ja jõudluse parandamiseks"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/08/2016"
   ms.author="carlrab"/>
   
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI kasutatud Azure'i oma arengutsükli kiirendamiseks ja selle klienditeenindus ja jõudluse parandamiseks

![Daxko/CSI Logo](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI tarkvara ees probleemiks: selle klient treeningu ja puhkuse alus oli kasvab kiiresti, tänu edu ettevõtte-tarkvara täielik lahendus, kuid ajakohaste IT-taristu vajadustele kasvab kliendi alus oli ettevõtte IT-töötajat katsetamine. Ettevõtte oli järjest piirab tõusev toimingute pea kohal, eriti haldamiseks oma andmebaase. On selle tegevuse pea kohal lõikamine arengu ressursid uue algatused, näiteks ettevõtte tarkvara mobility uued funktsioonid.

Vastavalt David Molina, Director toote arendamise veebisaidil Daxko/CSI andnud Azure'i platvormi-kui-a-service (PaaS) mudelit, mis seda vaja lihtsustada andmebaasi haldamine, skaleeritavus suurendada ja vabastada ressursse keskenduda tarkvara asemel ops CSI tarkvara. "SQL Azure'i andmebaas on suurepärane võimalus meile. Ei peaks muretsema SQL serveri tõrkesiirdeklastrite ja kõik muud taristu vajab oli meie jaoks parim."

Migreerimine Azure, kuna CSI tarkvara peab toimingute töötajad ainult kaks üle 600 kliendi andmebaaside haldamine. Ettevõte kasutab Azure'i SQL-andmebaasi elastne kaustu kliendi andmebaaside suurusest ja vaja.

Molina ei lahene, "meie klientide tunda muudatus kohe. Enne elastne kaustu, aeg-ajalt neil ajalõpud ja ka muude probleemide plahvatuse ajal. Azure'i elastne kaustu, mille nad lõhkeda vastavalt vajadusele ja kasutada tarkvara probleemideta."

Lisaks parandada klientidele, andmebaasi Azure elastne kaustu luua uusi teenuseid ja funktsioone, mitte toimingute ja haldus CSI tarkvara ressursse. IT-ressursid aidanud CSI tarkvara täiustada oma ettevõtte tarkvara pakkuv SpectrumNG aidata tegeleda võimla liikmed, töötajate tõhustada ja anda töötajate ja liikmete mobiilse juurdepääsu interaktiivsed ülesanded ja reaalajas teatised.

Azure'i aitas CSI tarkvara kiirendada ja parandada arendamise ja kvaliteedi-tagamine (KV) tsükli, võimaldades automatiseerimise suvandid. Ettevõtte Azure täitmisega saate paketti Koosta haldurid luua komponendid, klõpsake nupu. Kui Molina kirjeldab, "osana release tsükli kv on nüüd võimalik juurutada testimiskeskkonnas Azure, mis jäljendab lähemalt meie tootmise virnas. Me saate juurutada järgud kohe meie arenduskeskkond teha ettevõtjalikke muudatusi. Mis on suur võit meile, kuna me ei ole võrdse enne seda testimiseks."

## <a name="offloading-to-the-cloud"></a>Mahalaadimine pilveteenusesse

Enne teisaldada pilve, CSI tarkvara oli edukalt loonud omaette rentnikuga taristu kohaliku andmekeskuses Houston. Ettevõtte laiendatud, selle ees suurenevad kasvuvalud ostmist, ettevalmistamise ja säilitades kõik riistvara ja tarkvara vaja toetada oma klientidele. Personali reageerimine toimingute sai teise kitsaskoht, mis viis ettevalmistamise uute ressursside ja jooksvalt läbi uusi teenuseid klientidele vähendada.

Pilveteenuse suvandite kõrvaldamise pea selle kohal, uurinud CSI tarkvara nii, et see võiks keskenduda selle kood oma tegevust mitte. Ettevõtte avastanud, et paljude ülaosas cloud pakkujate ainult pakuvad taristu-kui-a-service (IaaS) lahendusi nõudvate endiselt suurte IT-töötajat IaaS virnas haldamiseks. Lõpuks CSI tarkvara kindlaks teinud, et Azure'i PaaS lahendus oma vajadustele kõige sobivam. Molina selgitatakse, "Azure saab riistvara ja süsteemi tarkvara ära, et saaksime keskenduda meie tarkvara pakkumisi, vähendades IT-kulusid."

## <a name="making-the-transition-to-azure"></a>Azure'i võtvaid

Pärast valimist oma PaaS lahenduse Azure'i CSI tarkvara algas selle kirjutamata taristu ja andmebaaside migreerimine pilve. Enne Azure, vaja installida Windows Communication Foundation (WCF) teenuse tagasi lõpuks klientrakendusega, mida SpectrumNG kliendid. Vastavalt Molina, "Kuigi mõned kliendid majutatud kõik oma andmekeskuste meil ehitatud välja toote olevat rentnikuga. Oleme majutatud kõik andmekeskuses, mis Houston, kasutades SQL serveri andmesalve.

"Meie toote pakuvad ka sisalduv liikme suunatud veebiportaali (kirjutatud ASP.net-i), mis oli mõeldud valge sildistatud vastavaks kliendi web kohaloleku- ja SOAP API Online'i lehtede ja mis tahes muu integratsiooni toetamiseks."

Migreerimise pilveteenusesse ei võta kaua aega arhitektuur jaoks. Vastavalt Molina "Enamik vaeva tegelenud muutmine nii, et me lugemiseks config faili teabe, tsentraliseeritud ühendusstringi muutmist, ja automatiseerimine pakendit, üleslaadimine ja juurutamise meie versioonides."

Koosta automatiseerimise arendada CSI tarkvarainseneri kasutatakse Azure PowerShelli ja REST API-de loomiseks pakettide ja üleslaadimine lavastus keskkonna jaoks vabastage iga öösel.
Üldine ülemineku Azure pilvepõhise juurutusega läks kiiresti ja sujuvalt CSI tarkvara selle meeskonna jaoks. Molina selgitatakse, "kõik oli meil beeta keskkonna pilveteenuses võttes projekt on nelja nädala jooksul. See oli üllatav võit meile."

Pärast konfigureerimise ja testimise keskkonna CSI tarkvara algas migreerimine kliendid. Klientidele, kes juba kasutab CSI tarkvara majutusteenuse, oli peaaegu sujuv üleminek. Klientidele, kes on kohapealse juurutuse opsüsteemilt, migreerimise pilveteenusesse võttis täiendavad aega, kuid on endiselt peamiselt valutu nii kliendid ja CSI tarkvara.

Uutele klientidele, CSI tarkvara IT-töötajat järgmiste protsess rongis neid kasutada Azure:

1.  Azure'i PowerShelli skriptide abil tööasendisse uue andmebaasi jaoks kliendi. Kõik kliendid alustada premium taseme üleminekuks piisavalt algse läbilaskevõime tagamiseks.
2.  Kui võimalik, kasutatakse CSI tarkvara Azure SQL-i migratsiooni viisardi olemasolevate andmete teisaldamiseks eksemplari Azure'i SQL-andmebaasi.
3.  Lõpetuseks, kasutatakse Microsoft SQL serveri Services (SSIS) erinevused andmeid sobitada või mis tahes andmete puhastamine vastavalt vajadusele.

Täna, umbes 99% CSI tarkvara kliendid on majutatud Azure, üle nelja piirkondliku andmekeskuste (keskse Põhja, Lõuna Central, Ida, ja Lääne). Iga kliendi geograafilise asukoha, millel andmekeskuste, latentsus on võimalikult vähe.

## <a name="azure-elastic-database-pools-free-up-it-resources"></a>Azure'i elastne andmebaasi kaustu IT ressursse

Mitmesuguseid funktsioone Azure aidanud CSI tarkvara on taristu ja toimingud, mis on funktsioon ja arengu värskendustest värskendustest shift. Suurim kasu on võib-olla alates elastne andmebaasi rühmituse.

CSI tarkvara pakub praegu umbes 550 andmebaaside klientidele. Enne elastne kaustu, on raske taseme struktuuri, et paljud andmebaaside haldamine. Ops haldurid oli jõudlus astme põhjal lõhkemist klientide vajaduste rahuldamiseks, mida vaja märgatavat IT-ressursside kohal määramiseks. Elastne andmebaasi kaustu, mille juhtide saate premium või standard pargis, vastavalt vajadusele rentnikud määramine teisaldamine suurusest kliendid ja vaja. Klientide tundnud andmebaasi elastne kaustu mõju peaaegu kohe; enne elastne kaustu, kliendid oli ajalõpud ja ka muude probleemide ajal lõhkemist kasutamine, kuid elastne kaustu, mille kliendid saavad kogemusi tegevuse lõhkemiseni vastavalt vajadusele ja neid edasi kasutada SpectrumNG probleemideta.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure Active Geo Dispersioonanalüüs kiirendab teatamine

Mitme CSI tarkvara kliendid on ka ära, Azure Active Geo-Dispersioonanalüüs. Aktiivse Geo koos – kopeerimine, kuni nelja loetavad teisene andmebaaside saab konfigureerida samas või mõnes muus andmekeskuse piirkondades. CSI tarkvara kasutab aktiivse Geo-kopeerimine kaks võimalust: esmalt teisene andmebaaside on saadaval andmekeskuse katkestuste või ei suuda ühenduse esmane andmebaasi; ja teiseks teisene andmebaaside on loetav ning et kirjutuskaitstud teenustest, nt aruandlusteenuste töö saab kasutada. Mõned kliendid CSI tarkvara abil see kasu kiirendada aruandlusteenuste töövood.

## <a name="csi-software-application-logic-and-architecture"></a>CSI tarkvara rakenduse loogika ja arhitektuur

SpectrumNG kasutab web rollid. Kuna rakendus on mitme rentniku, WCF-teenus abil esmakordse päringu klientide töötlemiseks. Nagu Molina "taotluse tuvastab iga kliendi seejärel võimaldab meile koostada ühendusstringi välja teha, mida iganes läheb vaja teha andmebaase."

Selle teenuse käsitleb web CSI tarkvara ära Azure automaatse skaala päeva ja kellaaja alusel. Saadaval ressursid automaatselt suurendatakse mahutamiseks suurema kasutamise tööajal, iga piirkondliku andmekeskuse ajavööndi järgi. Ressursid määratakse ka nädalavahetustel, kui klientide vajaduste väiksemad mahtu.

     
![Daxko/CSI arhitektuur](./media/sql-database-implementation-daxko/figure1.png)

Joonis 1. Cloud services töötaja roll juhib Liigendatud andmete Azure'i SQL-andmebaasi ja poolstruktuurandmed andmete tabelimäluga. SpectrumNG kasutajate suhelda, et pilv kaudu teenuste web roll.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Mobiilirakendused veebirakendusi ja veebi-leping taseme abil

Azure'i SQL-andmebaasi luua ressursid CSI tarkvara abil saate lubada uued algatused, sh täielik mobiilne platvorm põhjal kohandatud API, mis on majutatud Azure veebirakendustes. Platvormi võimaldab mobiilsideseadmete abil kontrollida ajakavade, broneerida tunnid ja sõnumeid, mis võimla liikmed ja personalile.

Platvormi kasutab teenusele suunatud arhitektuuri (SOA) ühe komponendi – näiteks esmamüügikohti süsteemi (POS) või müügi süsteemi – liigutada pealt web leping mõne muu ja seejärel tööasendisse teenus, mis toetavad komponendi, jättes veel algse web lepingut. Selle võimaluse annab CSI tarkvara suurt paindlikkust ja see aitab hoida kulud.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure'i võimaldab CSI tarkvara arendajate keskenduda rakendused ja teenused

SQL Azure'i andmebaas pole lihtsalt õnnistuseks SpectrumNG kliendid, kellel on kiire, usaldusväärseid teenus, samuti on suur võit CSI tarkvara arendajad ja personali. Mahalaadimine ops Azure'i pilves, CSI tarkvara piiratud oma pea kohal ressursside ja taristu, oluliselt kiirendada oma arengu tsüklit ja enam vajadustele micromanage andmebaasidele optimeerida jõudlust selle rentnike jaoks.

## <a name="more-information"></a>Lisateave

- Azure'i elastne andmebaasi kaustu kohta leiate lisateavet teemast [elastne andmebaasi kaustu](sql-database-elastic-pool.md).

- Andmebaasiriistad ja elastne mastaapimist kohta leiate lisateavet teemast [elastne Andmebaasiriistad ja elastne mastaapimist](sql-database-elastic-scale-get-started.md).

- SQL serveri andmebaasi migreerimine kohta leiate lisateavet teemast [Azure SQL-i migratsiooni viisard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md).

- Lisateavet kohta aktiivse Geo-kopeerimine, lugege teemat [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md).

- Web rollid ja töötaja rolli kohta leiate lisateavet teemast [töötaja rollid](../fundamentals-introduction-to-azure.md#compute). 

- Azure'i teenus siini kohta leiate lisateavet teemast [Azure teenuse siini](https://azure.microsoft.com/services/service-bus/).

- Mastaabi automaatselt kohta leiate lisateavet teemast [skaleerimist pilveteenustega](../cloud-services/cloud-services-how-to-scale.md).
