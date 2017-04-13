<properties
   pageTitle="Azure'i rakenduste avariitaastet | Microsoft Azure'i"
   description="Tehniline ülevaade ja põhjalikumat teavet Microsoft Azure tõrkejärgseks rakenduste kujundamine."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Avariitaastet rakenduste Microsoft Azure'i sisse ehitatud

Kõrge-saadavus on ajutine tõrge haldustoimingute kohta, katastroofiabi (DR) on rakenduse funktsionaalsust katastroofiline kadu. Näiteks, võiksite seda stsenaariumi, kus piirkonnas läheb alla. Sel juhul peate on leping rakenduse käivitada või Azure väljaspool teie andmetele juurde pääseda. Selle lepingu täitmise hõlmab inimesi, protsesside ja täiendavad rakendused, mis võimaldavad süsteemi funktsiooni. Äri- ja omanikud, kes määratlemine süsteemi funktsionaalseid režiimi katastroof määrata ka teenuse funktsioonid taset õnnetuse ajal. Funktsioonide tase võib olla mõni vormis: saadaval täielikult, osaliselt saadaval (halvenenud funktsioone või viivitada töötlemine) või täielikult saadaval.

##<a name="azure-disaster-recovery-features"></a>Azure'i katastroofi taastamine funktsioonid

Sarnaselt kättesaadavus peaksite arvesse võtma, on Azure [paindlikkust tehnilised juhendid](./resiliency-technical-guidance.md) , mis on mõeldud toetama katastroofiabi. Olemas on ka seose osa kättesaadavus funktsioonid Azure ja Avariijärgne taaste vahel. Näiteks viga domeenides rollide haldamine suurendab rakenduse kättesaadavus. Ilma, et haldamine muutub töötlemata riistvara tõrge "katastroof" stsenaarium. Seega on õige rakendus funktsioonide saadavaloleku ja strateegiad oluline osa katastroofi Õigekeelsuskontroll rakenduse. Aga selles artiklis ületab üldiselt kättesaadav probleemid veel raske (ja harvem) katastroofi sündmused.

##<a name="multiple-datacenter-regions"></a>Mitmes andmekeskuses piirkondade

Azure'i säilitab andmekeskuste mitmes piirkonnas kogu maailmas. Selle taristu toetab mitut Avariijärgne taaste stsenaariumid, nagu on esitatud süsteemi geo-dispersioonanalüüs Azure'i salvestusruumi teisene piirkondadele. See tähendab ka, saate hõlpsalt ja odavalt juurutada pilveteenus mitme asukohtadesse kogu maailmas. Võrrelda seda maksumuse ja raskusi käitamise oma andmekeskuste mitme piirkondades. Mitme piirkonna andmete ja teenuste juurutamine aitab kaitsta teie taotlus põhi katkestuste ühe piirkonna.

##<a name="azure-traffic-manager"></a>Azure'i liikluse haldur

Piirkonnakohase tõrke ilmnemisel peate häälestama ümbersuunamise liikluse teenuseid või mõne muu piirkonna juurutuste. Saate teha selle marsruudi käsitsi, kuid see on otstarbekam kasutada automatiseeritud protsessi. Azure'i liikluse haldur on mõeldud see toiming. Saate automaatselt hallata kasutaja liikluse teises regioonis Tõrkesiirde esmane piirkond ei. Kuna korraldamise on üldine strateegia oluline osa, on oluline põhialuste liikluse haldur.

Järgmisel joonisel kasutajate ühenduse URL, mis on määratud liikluse Manager (__http://myATMURL.trafficmanager.net__) ja selle kokkuvõtteid tegelik saidi URL-id (__http://app1URL.cloudapp.net__ ja __http://app2URL.cloudapp.net__). Põhineb konfigureerimise kriteeriumid marsruutimiseks kasutajale, kasutajatele saadetakse õige tegelik saidi kui poliitika ütleb. Poliitika valikud on ringi-jaan, jõudluse või Tõrkesiirde. Pärast käesoleva artikli meil on seotud Tõrkesiirde valik ainult.

![Marsruutimise kaudu Azure'i liikluse haldur](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Liikluse haldur konfigureerimisel esitate uue liikluse haldur DNS-i eesliide. See on URL-i eesliide, mis teil tuleb sisestada oma teenuse kasutajatele. Nüüd abstracts liikluse haldur laadi ühe taseme üles- ja regiooni tasemel pole tasakaalustamiseks. DNS-i haldur liikluse kaardid juurutuste, mida haldab ka CNAME.

Sees liikluse haldur, saate määrata juurutuste, mida kasutajad suunatakse tõrke ilmnemisel prioriteet. Liikluse Manager jälgib lõpp-punktid juurutuste ja märkmete esmane juurutamise nurjub. Tõrge, liikluse haldur analüüsib juurutuste tähtsuse loendit ja marsruudib kasutajate kõrval üks loendis.

Kuigi liikluse haldur määrab, kus soovite minna Tõrkesiirde, saate otsustada, kas domeeni Tõrkesiirde on soikus või aktiivne, samal ajal, kui te pole Tõrkesiirde režiimis. Seda funktsiooni ei ole midagi Azure'i liikluse haldur. Liikluse haldur tuvastab tõrke esmane saidi ja koondab Tõrkesiirde saidile. Liikluse haldur koondab sõltumata sellest, kas selle saidi on praegu serveeritakse kasutajate või mitte.

Azure'i liikluse haldur toimimise kohta lisateabe saamiseks vaadake:

 * [Liikluse haldur ülevaade](../traffic-manager/traffic-manager-overview.md)
 * [Tõrkesiirde marsruutimise meetod konfigureerimine](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Azure'i katastroofi stsenaariumid

Järgmistes jaotistes katta mitmesuguseid eri tüüpi katastroof stsenaariumi. Piirkond kogu teenuse häirete ei ole ainult rakenduse kogu tõrkeid põhjustada. Kehv kujundus või Administreerimine vigu võib põhjustada ka katkestuste. See on oluline silmas pidada kujundus ja testimise faasis oma taastamise lepingu tõrke võimalikud põhjused. Hea plaan võtab Azure funktsioone ja suurendab nende rakenduste kohased strateegiad. Valitud vastus tuleb valida rakenduse, tähtsus taastamine punkti eesmärk (RPO) ja taastamise aja eesmärk (RTO).

###<a name="application-failure"></a>Rakenduse nurjumine

Azure'i liikluse haldur tegeleb automaatselt aluseks oleva riistvara või operatsioonisüsteem tarkvara host virtuaalse masina põhjustavad tõrkeid. Azure'i loob uue eksemplari roll toimimise serveris ja lisab selle laadi-koormusetasakaalustusteenuse pööre. Kui rolli eksemplaride arv on suurem kui üks, Nihutab Azure'i töötlemise ajal asendades nurjunud sõlm muud rolli töötavad rakenduse.

On raske rakenduse tõrked, mis tehakse sõltumatult mis tahes riistvara või operatsioonisüsteem ebaõnnestumist. Rakendus ei pruugi katastroofiline erandid halb loogika või andmete terviklusega seotud probleemide tõttu. Piisavalt telemeetria tuleb lisada koodi, et jälgimise süsteemi tuvastab nõuded ja teavitamise rakenduse administraator. Administraator, kellel on täielik teadmisi katastroofi taastamise protsessid saab teha otsus autonoomsest Tõrkesiirde protsess. Teise võimalusena lihtsalt aktsepteerida administraator on kättesaadavus katkestuste lahendamiseks kriitilisi tõrkeid.

###<a name="data-corruption"></a>Andmete rikkumist

Azure'i salvestab automaatselt Azure'i SQL-andmebaasi ja Azure Storage andmete kolm korda redundantly erinevate viga domeenide piirkonna sees. Kui kasutate geo-kopeerimine, talletatakse andmeid teises regioonis kolm korda. Juhul, kui kasutajate või rakenduse rikub esmane Kopeeri andmeid, andmed kiiresti tiražeerib teised koopiad. Kahjuks on tulemuseks kolm eksemplari andmed.

Võimalikud rikutud oma andmete haldamiseks, on teil kaks võimalust. Esmalt saate hallata kohandatud varukoopia strateegia. Varukoopiate salvestamiseks saate talletada Azure'i või kohapealse, sõltuvalt teie ettevõtte nõuetele või juhtimise õigus. Teine võimalus on kasutada SQL-andmebaasi taastamisel uue punkti /-kellaaja Taasta valik. Lisateavet leiate jaotisest [andmete strateegiad katastroofiabi](#data-strategies-for-disaster-recovery)kohta.

###<a name="network-outage"></a>Võrgu katkestuste

Kui Azure võrgu osad ei pääse, ei pruugi te saada teie rakendus või andmete. Kui üks või mitu rolli eksemplari pole võrguprobleemide tõttu saadaval, kasutab Azure ülejäänud saadaval rakenduse eksemplari. Kui teie rakendus ei pääse oma andmete on Azure võrgu katkestuste tõttu, saate potentsiaalselt käivitate halvenenud kohalikult abil vahemällu talletatud andmetega. Peate arhitekt katastroofi taastamise strateegia oma rakenduse halvenenud režiimis. Teatud rakenduste puhul see võib olla vajalik.

Teine võimalus on andmete talletamiseks alternatiivse asukoha kuni ühendus on taastatud. Kui halvenenud pole suvandi, ülejäänud suvandid on rakenduse tööseisakute või Tõrkesiirde alternatiivse piirkonnale. Rakendus töötab halvenenud on nii palju äri otsust tehniline. Seda käsitletakse edasise jaotises [halvenenud rakenduse funktsionaalsust](#degraded-application-functionality).

###<a name="failure-of-a-dependent-service"></a>Sõltuvad teenuse tõrge

Azure'i pakub palju teenuseid, mis võib tekkida perioodiliste tööseisakute. Kaaluge võimalust näidisena [Azure'i Redis vahemälu](https://azure.microsoft.com/services/cache/) . Mitme rentniku see teenus pakub vahemällu talletamise võimalused rakenduse. See on oluline silmas pidada, mis juhtub teie rakenduses, kui sõltuvad teenus pole saadaval. Mitmel viisil on sarnane võrgu katkestuste stsenaarium seda stsenaariumi. Tõstutundlikkuse siiski iga teenuse sõltumatult tulemuseks võimalikud täiustused üldine lepingule.

Azure'i Redis vahemälu pakub rakenduse pilvepõhise teenuse juurutamise, mis pakub katastroofi taastamine eelised: vahemällu. Esmalt töötab teenus nüüd rollid, mis on kohalik juurutamise. Seega saate paremini jälgida ja hallata oma üldise halduse protsessid pilveteenusesse osana vahemälu olekut. Seda tüüpi vahemällu seab ka uusi funktsioone. Üks uusi funktsioone on kõrge kättesaadavus vahemällu talletatud andmetega. See aitab säilitada teiste sõlmed koopiate säilitades ühe sõlme nurjumisel vahemällu talletatud andmetega.

Pange tähele, et kõrge-saadavus väheneb läbilaskevõime suureneb latentsuse tõttu teisene koopia värskendamine kirjutab. See ka kahekordistab mälu, mida kasutatakse iga üksuse jaoks, mis nii kavandamine. Näites konkreetse näitab, et iga sõltuvad teenuse võib-olla võimalusi, mida parandada oma üldist kättesaadavust ja vastupanuvõimet katastroofiline ebaõnnestumist.

Iga sõltuvad teenusega peaks mõistate teenuse katkemise. Vahemällu näites oleks võimalik andmetele juurdepääsuks otse andmebaasi kuni vahemälu. See oleks halvenenud, jõudluse osas, kuid annaks täisfunktsionaalsuse andmeid.

###<a name="region-wide-service-disruption"></a>Piirkonna kogu teenuse häirete

Eelmise tõrkeid on peamiselt tõrkeid, mida saab hallata Azure piirkonna sees. Siiski peate ka ette võimalust, et on teenuse häirete kogu piirkonna jaoks. Piirkonna kogu teenuse häirete juhul kohalikult liigsete andmete eksemplarid pole saadaval. Kui olete lubanud geo-dispersioonanalüüs, on kolm täiendavad koopiat teie plekid ja tabelite teises regioonis. Kui Microsoft kinnitab piirkonna kaotsi, Azure'i remaps kõik DNS-i kirjed geo kopeeritud alale.

>[AZURE.NOTE] Pange tähele, et teil pole mõni juhtida seda toimingut, ja see ei puuduta ainult piirkond kogu teenuse häirete. Seetõttu tuleb toetuvad teiste kättesaadavuse kõrgeima taseme saavutamiseks rakendusele vastav varukoopia strateegiad. Lisateavet leiate jaotisest [andmete strateegiad katastroofiabi](#data-strategies-for-disaster-recovery)kohta.

###<a name="azure-wide-service-disruption"></a>Azure'i kogu teenuse häirete

Klõpsake tegevuse kavandamine, peate arvesse võtma võimalik katastroofe terve lahtrivahemik. Üks kõige teenuse häirete nõuaks Azure kõigi piirkondade korraga. Sarnaselt muude teenuse häirete, võib otsustada, et teil ajutine tööseisakute riski sellisel juhul võtta. Levinud teenuse häirete piirkondade ulatuvad peaks olema palju harvem, kui isoleeritakse teenuse häirete sõltuvad teenused või ühe regioonid.

Siiski mõne olulise rakenduse võib juhtuda, et peate olema varukoopia leping selle stsenaariumi korral. Sel juhul kavandamine võivad suuda üle mõne [alternatiivse pilveteenuses](#alternative-cloud) või on [hübriid kohapealse ja pilveteenuse lahenduse](#hybrid-on-premises-and-cloud-solution)Services.

###<a name="degraded-application-functionality"></a>Halvenenud rakenduse funktsionaalsust

Hästi kujundatud rakendus kasutab tavaliselt saidikogumi moodulid, mis omavahel suhelda küll lõdvalt seotud teavet-andmevahetuse mustrite rakendamine. DR-sõbralik rakenduse jaoks on vaja ülesannete mooduli tasemel lahutamine. See on takistada sõltuvad teenuse häirete kogu rakenduse alla. Näiteks Kujutage äri veebirakenduse ettevõte y. Järgmised moodulid võib olla rakendus:

 * __Tootekataloogi__ võimaldab kasutajatel sirvida tooted.
 * __Ostukorv__ võimaldab kasutajatel oma ostukorv toodete lisamine või eemaldamine.
 * __Tellimuse olek__ kuvatakse kasutaja tellimuste kohaletoimetamise olek.
 * __Tellimuse__ lõpetab ostude seansi makse tellimuse esitamisega.
 * __Tellimuse töötlemine__ kinnitatakse andmetervikluse võimaldamaks ja sooritab kogus saadavaloleku kontrollimine.

Kui sõltuv selle rakenduse mooduli läheb alla, kuidas ei mooduli funktsioon kuni selle osa taastab? Hästi architected süsteemi rakendab eraldamise piirmäärad ülesannete lahutamine koostamise ajal nii käitusajal kaudu. Saate iga tõrge Taastatavad ja muutudes kategoriseerida. Mitte-Taastatavad tõrgete võtab mooduli alla, kuid te saate leevendada taastuv tõrge alternatiive kaudu. Nagu kõrge-saadavus jaotise, saate peita mõned probleemid kasutajate eest töötlemise vead ja alternatiivne toiminguid. Ajal veel raske teenuse häirete, võib rakenduse täielikult saadaval. Kolmas valik aga jätkamiseks teenindamine halvenenud kasutajad.

Näiteks kui andmebaasi majutamiseks tellimused läheb alla, lähevad tellimuse töötlemine mooduli kaotsi selle võimalus müügitehingud töötlemine. Sõltuvalt arhitektuur, võib olla suur või tellimuse esitamise ja tellimuse töötlemine osad jätkamiseks rakendus ei ole võimalik. Kui rakendus on mõeldud pole sel juhul käsitlema, kogu rakenduse avage ühenduseta.

Sama stsenaariumi on võimalik, et toote põhiandmed on salvestatud mõnes muus asukohas. Sel juhul tootekataloogi moodul saate siiski kasutada toodete vaatamiseks. Halvenenud režiimis endiselt rakenduse jaoks saadaolevate funktsioonide nagu vaatamise tootekataloogi kasutajatele saadaval. Muud osadele, on siiski saadaval, nt päringud tellimisel või laoseisu.

Teine variatsioon halvenenud keskendub jõudlusega, mitte võimalusi. Näiteks arvesse võtta stsenaariumi, kus tootekataloogi vahemälurežiimi Azure'i Redis vahemälu kaudu. Kui vahemällu pole saadaval, rakendus võib minge otse serveri talletusruumi toote kataloogi teabe. Kuid see access võib olla aeglasem kui vahemällu talletatud versiooni. Seetõttu on rakenduse jõudlus halvenenud kuni vahemällu teenus on taastatud täielikult.

Otsustada, kui palju rakenduse jätkab funktsioon halvenenud on nii business otsust ja tehnilise otsust. Rakendus peab otsustada, kuidas teavitada kasutajaid ajutisi probleeme. Selles näites rakenduse lubada toodete vaatamiseks ja isegi lisamist ostukorvi. Juhul, kui kasutaja üritab ostu, rakenduse teavitab kasutaja, et müügi mooduli on ajutiselt kättesaamatuks. See pole optimaalne kliendi jaoks, kuid takista see on rakenduse kogu teenuse häirete.

##<a name="data-strategies-for-disaster-recovery"></a>Andmete avariitaastet strateegiad

Andmete töötlemise õigesti on õige raskem ala mis tahes katastroofi taastamine kavandamine. Andmete taastamine on osa, mis kulub tavaliselt taastamise protsess kõige rohkem aega. Erinevate valikute degradeerumine režiimid tulemuseks keeruline väljakutsed andmete taastamine tõrge ja järjepidevuse pärast ebaõnnestumist.

Üks tegureid on vaja taastamiseks või säilitamiseks koopia rakenduse andmeid. Viide ja selgituseks eesmärgil teisene saidil kasutatakse andmed. Asutusesisese säte nõuab kallis ja aeganõudev kavandamisprotsessi mitme piirkonna katastroofi taastamise strateegia rakendamiseks. Enamik teenusepakkujaid pilveteenuses, sh Azure, luba mugavalt, hõlpsasti kasutusele mitme piirkonna. Regioonides on geograafiliselt laiali nii, et mitme piirkonna teenuse häirete tuleks väga harva. Strateegia andmete töötlemise piirkondades on üks lepinguid katastroofi taastamise edu tegurid.

Järgmistes lõikudes käsitletakse seotud andmete varundamise, lähteandmed ja kandeandmete katastroofi taastamise meetodite.

###<a name="backup-and-restore"></a>Varundus ja taaste

Korrapäraste varukoopiate plaanimine rakenduse andmete toetab mõni Avariijärgne taaste stsenaariumid. Erinevate salvestusruumi ressursse vaja meetodit.

Jaoks astme Basic, Standard ja Premium SQL-andmebaasiga, saate ära punkti õigel ajal taastamine andmebaasi taastada. Lisateavet leiate teemast [Ülevaade: Cloud business järjepidevus ja andmebaasi Õnnetusejärgne taastamine SQL-andmebaasi](../sql-database/sql-database-business-continuity.md). Teine võimalus on kasutada aktiivse Geo-kopeerimine SQL-andmebaasi. See automaatselt tiražeerib andmebaasi muudatusi teisene andmebaaside Azure piirkonna või isegi muu Azure piirkonna. See pakub rohkem käsitsi andmete sünkroonimine tehnika käesolevas artiklis esitatud mõned võimalikud alternatiiv. Lisateavet leiate teemast [Ülevaade: SQL-i andmebaasi aktiivse Geo kopeerimine](../sql-database/sql-database-geo-replication-overview.md).

Saate kasutada rohkem käsitsi lähenemine varundamiseks ja taastamiseks. Andmebaasi koopia loomiseks kasutada käsku KOPEERI andmebaasi. Kasutage selle käsu saada selgituseks järjepidevus varukoopia. Samuti saate Azure'i SQL-andmebaasi teenuse impordi/ekspordi. See toetab BACPAC faile, mis on talletatud Azure'i bloobimälu eksportiva andmebaase.

Sisseehitatud komponentide Azure'i salvestusruumi luuakse kaks koopiad varufaili piirkonna. Siiski sagedus varukoopia protsess määratleb oma RPO, mis on andmehulga võite kaotada katastroofi stsenaariumide. Oletagem näiteks, varukoopia tegemist ülaosas tund, ja katastroof ilmneb kaks minutit enne tunni algusse. Sellega kaotate andmeid, mis on juhtunud pärast viimast varundamist tehtud 58 minutit. Samuti, piirkond kogu teenuse häirete kaitsta, tuleks BACPAC faile kopeerida alternatiivse piirkonnale. Seejärel on teil võimalik taastamise nende alternatiivse piirkonna. Lisateavet leiate teemast [Ülevaade: Cloud business järjepidevus ja andmebaasi Õnnetusejärgne taastamine SQL-andmebaasi](../sql-database/sql-database-business-continuity.md).

Azure Storage, saate arendada oma kohandatud varundamist või kasutada ühte paljudest kolmanda osapoole varukoopia tööriistu. Pange tähele, et enamik rakenduse kujundused täiendavad keerukus kus salvestusruumi ressursid viide üksteisest. Näiteks vt SQL-andmebaasi, mis sisaldab veergu mis on lingitud mõne bloobimälu Azure Storage. Kui varukoopiaid samal ajal ei juhtu, peate andmebaasi kursori bloobimälu, mis oli varundada enne tõrke. Rakenduse või katastroofi taastamise kava rakendama protsesside käsitlema see vastuolu pärast.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Viide andmete mustri avariitaastet jaoks

Andmed on kirjutuskaitstud andmeid, mis toetab rakenduse funktsionaalsust. Tavaliselt ei muuda sagedamini. Varundus ja taaste on üks meetod piirkond kogu teenuse häirete käsitlema, on selle RTO suhteliselt pikka. Rakenduse teisene alale juurutamisel mõned strateegiad parandada RTO viide andmete jaoks.

Kuna viide andmemuudatuste harva, saab parandada selle RTO, säilitades viide andmed teisel piirkonna alalise koopia. See kaob õnnetuste varukoopiate taastamiseks vajalik aeg. Mitme piirkonna katastroofi taastamine nõuete täitmiseks peab juurutada rakendus ja mitme piirkonnad koos viide andmeid. Nagu [viide andmete mustri kõrge-saadavus](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability), saate juurutada roll, välisesse salvestusruumi või kombinatsiooni mõlemad andmed.

Viide juurutamise andmemudeli jooksul Arvuta sõlmed peidetult vastab katastroofi taastamine. Viide andmete juurutamise SQL-andmebaasiga nõuab viide andmete koopia juurutama iga piirkonna. Sama strateegiat kehtib Azure Storage. Peate juurutama viide andmeid, mida talletatakse Azure Storage esmaseid ja teiseseid piirkondadele koopia.

![Viide andmete avaldamine nii põhi- ja piirkonnad](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

Peate oma rakenduse kohased varukoopia moodulid kõik andmed, sh lähteandmed rakendama. Piirkondade Geo kopeeritud eksemplaride kasutatakse ainult piirkond kogu teenuse häirete. Laiendatud tööseisakute vältimiseks juurutada olulise osad rakenduse andmete teisese piirkond. Näiteks see topoloogia, leiate teemast [aktiivne passiivne mudel](#active-passive).

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Kandeandmete mustri tõrkejärgseks

Täielikult toimiv katastroofi režiimi strateegia nõuab asünkroonne kopeerimine kandeandmete teisene alale. Praktiline aja aknad sees, mis võivad tekkida selle dispersioonanalüüs määrab rakenduse RPO omadused. Siiski võib taastada andmed, mida iga katkes esmane piirkonnast ajal dispersioonanalüüs akna. Samuti võib ühendada teisene piirkond hiljem on.

Järgmised näited arhitektuur pakuvad ideid töötlemise kandeandmete Tõrkesiirde stsenaarium erineval viisil. See on oluline Pange tähele, et need näited pole täielik. Näiteks võib vahe salvestusruumi kohtadesse, nt järjekorrad Azure'i SQL-andmebaasi asendada. Järjekorrad ise võib olla kas Azure Storage või Azure'i teenus siini järjekorrad (vt teemat [Azure järjekorrad ja teenuse siini järjekorrad--võrreldes ja ja](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Serveri talletusruumi sihtkohta võib erinevad, nt Azure SQL-andmebaasi asemel tabeleid. Lisaks võib töörühm erinevate toimingutes lisada töötaja rollid. Oluline on pole neid arhitektuurides täpselt jäljendada, kuid erinevaid võimalusi taastamise kandeandmete ja seotud moodulid silmas pidada.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Kandeandmete ettevalmistamisel avariitaastet kopeerimine

Kaaluge rakendus kasutab Azure Storage järjekorrad kandeandmete hoida. See võimaldab töötaja rolli selgituseks andmete serveri andmebaasiga sidumata arhitektuur. Selleks on vaja kasutada ajutist vahemällu kui ees rollid nõuab viivitamatut päringu andmeid mingi tehingud. Sõltuvalt andmete kaotsimineku hälbe, võite ise järjekorrad, andmebaasi või kõik salvestusruumi ressursse. Andmebaasi tiražeerimine, kus kui esmane piirkond läheb allapoole, saate siiski taastada andmed järjekordades kui esmane piirkond tagasi.

Järgmisel joonisel on esitatud arhitektuur kus serveri andmebaasi sünkroonitakse piirkondade lõikes.

![Kandeandmete ettevalmistamisel avariitaastet kopeerimine](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

Suurim ülesanne rakendada selle arhitektuur on dispersioonanalüüs strateegia piirkondade vahel. Seda tüüpi dispersioonanalüüs võimaldab Azure SQL-i andmete sünkroonimine teenus. Siiski teenus on endiselt eelvaade ja ei ole soovitatav tootmise keskkonnas. Lisateavet leiate teemast [Ülevaade: Cloud business järjepidevus ja andmebaasi Õnnetusejärgne taastamine SQL-andmebaasi](../sql-database/sql-database-business-continuity.md). Tootmise rakendused, peate investeerida muu lahendus või saate luua oma dispersioonanalüüs loogika-koodi. Olenevalt arhitektuur, võib selle dispersioonanalüüs kahesuunaline, mis on keerulisem.

Üks võimalik rakendamiseks võib teha kasutada vahe järjekorra eelmises näites. Töötaja roll, töötleb andmeid lõplik salvestusruumi sihtkohta võib esmane piirkonna-ja teisene piirkond tehke soovitud muudatused. Need pole triviaalne tööülesanded ja täielik juhised dispersioonanalüüs kood on selles artiklis ei käsitleta. Oluline on see, et palju aega ja testimine tuleb keskenduda kuidas ise andmete teisese piirkond. Täiendavate ja testimine aitab tagada õigesti Tõrkesiirde ja taastamise protsessid hakkama mis tahes võimalike andmete vastuolusid või dubleeritud tehingud.

>[AZURE.NOTE] Enamik st keskendub platform teenus (PaaS). Kopeerimine ja -saadavus lisasuvandid hübriid rakenduste abil siiski Azure'i Virtuaalmasinates. Need rakendused hübriid abil taristu (IaaS) teenust majutada SQL Server Azure'i virtuaalmasinates. See võimaldab traditsiooniline-saadavus lähenemisel SQL Server, nt Kättesaadavusrühmad või Logi saatmine. Mõningaid viise AlwaysOn, nt töötada ainult asutusesisese SQL Serveri eksemplari ja Azure'i virtuaalmasinates vahel. Lisateabe saamiseks vt [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

####<a name="degraded-application-mode-for-transaction-capture"></a>Tehingu jäädvustada halvenenud rakenduse režiim

Kaaluge teise arhitektuur, mis toimib halvenenud režiimis. Teisene piirkond rakendus Desaktiveeri kõik funktsioonid, näiteks aruandlus, ärianalüüsi (BI) või tühjendamine järjekorrad. See aktsepteerib ainult kõige olulisemad tüüpi selgituseks töövood, määratletud äri nõuetele. Süsteemi jäädvustab tehingud ja kirjutab need järjekorrad. Süsteemi võite edasi lükata, andmete töötlemise ajal algsete teenuse häirete. Kui süsteemi esmane piirkonna kohta on oodatud ajaakna jooksul desaktiveerinud, saate töötaja rolli esmane piirkonna tühjendada järjekorrad. Selle protsessi välistab andmebaasi ühendamiseks mõeldud. Kui esmane piirkond teenuse häirete ületab lubatud akna, rakendus on töötlusjärjekorda alustada.

Selle stsenaariumi korral teisese andmebaas sisaldab suureneva kandeandmete, mis tuleb pärast esmast on uuesti aktiveerida. Järgmisel joonisel on esitatud selle strateegia ajutiselt salvestamiseks kandeandmete kuni esmane regioon on taastatud.

![Tehingu jäädvustada halvenenud rakenduse režiim](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Andmete haldamise viisid olles Azure rakenduste mitme arutelu, lugege teemat [Failsafe: juhised olles Cloud struktuur](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Juurutamise topoloogiatest tõrkejärgseks

Peate ette olulise rakenduste piirkond kogu teenuse häirete võimalus. Tehke seda lisades mitme piirkonna kasutuselevõtu strateegia tegevuse planeerimine.

Mitme piirkonna juurutuste võib hõlmata IT-pro protsesside pärast katastroofi teisene piirkond rakenduse- ja viitamisfunktsioonid andmete avaldamiseks. Kui rakendus nõuab kiirsõnumite Tõrkesiirde, võib juurutamise protsessi kaasata on aktiivne/passiivne häälestamine või on aktiivne/aktiivne häälestus. Seda tüüpi juurutamise on olemasoleva eksemplari alternatiivse piirkonna rakendus. Marsruutimise tööriist nagu Azure'i liikluse haldur pakub koormust tasakaalustavad teenuste DNS-i tasemel. See tuvastab teenuse häirete ja kasutajate marsruutida eri regioonide vastavalt vajadusele.

Eduka Azure avariitaastet osa on architecting selle taastamise lahendus algusest sisse. Pilveteenuses pakub lisasuvandid taastamise õnnetuse ajal tõrkeid, mis ei ole saadaval traditsiooniline majutusteenuse pakkuja. Täpsemalt, saate kiiresti ja dünaamiliselt määrata ressursse muule alale. Seetõttu ei maksate palju jõude ressursid kui olete oodanud tekib tõrge.

Järgmistes jaotistes katta erinevate juurutamise topoloogiatest tõrkejärgseks. Tavaliselt on suurem või keerukuse jaoks täiendavate kättesaadavus on Miinuseks.

###<a name="single-region-deployment"></a>Ühe piirkonna juurutamine

Ühe piirkonna juurutuse ei ole eriti Avariijärgne taaste topoloogia, kuid on mõeldud muu arhitektuurides kontrasti. Ühe piirkonna juurutuste on levinud rakenduste Azure. Seda tüüpi juurutamise ei ole raske contender katastroofi taastamine lepingule.

Järgmisel joonisel on kujutatud rakendus töötab ühe Azure piirkonnas. Rakenduse piirkonnas kättesaadavuse parandamiseks Azure liikluse juhataja ja viga ja täiendamine domeenide kasutamine.

![Ühe piirkonna juurutamine](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Siin on nähtav, et andmebaas on ühtne tõrge. Ehkki Azure'i tiražeerib andmed muu viga domeenides, et ettevõttesisese koopiad, ilmneb see kõik piirkonna. Rakendus ei pea vastu katastroofiline tõrge. Kui sama piirkonna läheb alla, kõik viga domeenid minna--koos kõigi eksemplaride ja salvestusruumi ressursid.

Kõik vaid vähemalt kriitilised esitamise, tuleb töötada plaani juurutamiseks rakenduste mitme piirkondade lõikes. Tuleks arvesse võtta RTO ja maksumus piiranguid otsustamisel, milline juurutamise topoloogia kasutada.

Vaatame lähemalt praegu mustriga toetamiseks Tõrkesiirde erinevate piirkondade lõikes. Nendes näidetes kõik kasutada mõlema piirkonna protsessi kirjeldamiseks.

###<a name="redeployment-to-a-secondary-azure-region"></a>Teisene Azure piirkonnas temaga

Teisene piirkond temaga struktuuris on ainult esmane piirkond rakenduste ja andmebaaside. Teisene piirkonna jaoks automaatselt Tõrkesiirde on häälestatud. Nii katastroof ilmnemisel peate spin üles uues teenuse osi. See hõlmab üleslaadimise pilveteenus Azure juurutamine pilveteenusesse, andmete taastamine ja DNS-i abil ümber marsruutida liiklust muutmine.

Kuigi see on soodsaim mitme piirkonna suvandid, see on kõige kehvemad RTO omadused. Selle mudeli talletatakse teenuse paketi ja andmebaasi varukoopiate kas kohapeal või sekundaarne piirkonna Azure'i bloobimälu salvestusruumi eksemplari. Siiski peate uue teenuse juurutamine ja andmete taastamine enne seda kasutataks toiming. Isegi juhul, kui te täielikult automatiseerida andmete edastamist varukoopia salvestusruumi, valmistamata üles uue andmebaasi keskkonna tarbib palju aega. Jooksev andmete varukoopia ketta salvestusruumi tühja andmebaasi teisene piirkonna kohta on kõige kallim osa taastamine. Peate tegema, kuid tuua uue andmebaasi toimiv riik, sest seda pole kopeeritud.

Parim lahendus on salvestada teenuse pakettide bloobimälu teisene piirkonna. See kaob vajadust paketi Azure'i, mis on, mis juhtub, kui juurutate kohapealse arengu arvutist üles laadida. Saate kiiresti juurutada teenuse pakettide uue pilveteenusesse kaudu bloobimälu PowerShelli skriptide abil.

See suvand on kasulik ainult mitte-kriitilised rakendused, mis saab luba kõrge RTO. Näiteks võib see toimib, kuid peaks töötama 24 tunni jooksul uuesti alla, mis võib olla mitu tundi rakenduse.

![Teisene Azure piirkonnas temaga](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Aktiivne passiivne.

Aktiivne passiivne mustri on palju ettevõtete kasuks valik. See muster pakub täiustused RTO maksumus suhteliselt kasvuga positsioonide mustri üle.
Selle stsenaariumi korral on uuesti esmane ja teisene Azure piirkond. Kõik liiklus läheb aktiivne juurutamise esmane piirkonna kohta. Teisene regioon on paremini valmis avariitaastet kuna mõlemad piirkonnad töötab andmebaasi. Lisaks on omavahel sünkroonimise süsteem. Seda valige moodust saab kaasata kaks variatsioonid: ainult andmebaasi lähenemine või täielik juurutamise teisene piirkonna.

####<a name="database-only"></a>Ainult andmebaasi

Esimese variatsiooni aktiivne passiivne mustri, ainult esmane piirkonnas on juurutatud cloud teenuserakenduse. Erinevalt positsioonide mustri, sünkroonitakse mõlemad piirkonnad koos andmebaasi sisu. (Lisateavet leiate jaotisest [kandeandmete](#transactional-data-pattern-for-disaster-recovery)muster tõrkejärgseks.) Katastroof ilmnemisel on vähem aktiveerimise nõuded. Käivitage rakendus teisene piirkonna, muuta ühendusstringi uue andmebaasi ja muuta selle DNS-i kirjete marsruudi liikluse.

Positsioonide mustri, nagu saate peaks on juba salvestatud teenuse pakettide Azure'i bloobimälu teisene piirkonna kiirem juurutamiseks. Erinevalt positsioonide mustri, ei pea kohal, mis nõuab andmebaasi taastetoimingu enamik kaasa tuua. Andmebaas on valmis ja töötab. See säästab palju aega, muutes selle mõne taskukohane DR mustri. Samuti on kõige populaarsemate DR mustri.

![Ainult passiivne, aktiivne andmebaas](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Täieliku koopia

Teine variatsioon aktiivne passiivne mustri, esmane piirkonna- ja teisene piirkond on täielik juurutamine. See juurutus sisaldab pilveteenuseid ja sünkroonitud andmebaasi. Ainult esmane piirkond aktiivselt on siiski käitlemine võrgu taotluste kasutajad. Teisene piirkond aktiveerub ainult siis, kui esmane piirkond kogemusi teenuse häirete. Sel juhul kõik uued võrgu taotlused marsruuditakse teisene alale. Azure'i liikluse haldur saate hallata selle Tõrkesiirde automaatselt.

Tõrkesiirde põhjuseks kiiremini andmebaasi ainult variatsioonide teenused on juba juurutanud. See muster pakub väga väike RTO. Teisene Tõrkesiirde piirkond peab olema valmis kohe pärast esmane piirkond.

Koos kiiremini vastuse aja, on see muster ära eelnevalt eraldamiseks ja varukoopia teenuste juurutamine. Te ei pea muretsema, ei ole ruumi eraldada uued eksemplarid katastroof piirkonna kohta. See on oluline, kui teie teisene Azure regioonis on lähenemas võimsus. Ei ole kindel (teenusetaseme leping), et kohe saab juurutada arvu uue pilveteenustega mis tahes ala.

Kiireima vastuse korda mudeli, peab teil olema esmaseid ja teiseseid piirkondades sarnase suurusega (rolli eksemplaride arv). Vaatamata eeliseid, kasutamata Arvuta eksemplarid maksavad on kulukas ja see ei pruugi olla kõige usaldusväärse rahandus valik. Seetõttu on sagedamini pilveteenustega veidi mastaabitud alla versiooni kasutamine teisene piirkond. Seejärel saate kiiresti ei õnnestu üle ja vajadusel välja teisene juurutamise mastaapimiseks. Tõrkesiirde peaks automatiseerida nii, et pärast esmane regioon on juurdepääs, saate aktiveerida täiendavad eksemplarid, olenevalt laadi. See võib hõlmata ka autoscaling süsteem nagu [virtuaalse masina skaala määrab](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)kasutamine.

Järgmisel joonisel on esitatud kui esmaseid ja teiseseid piirkondade sisaldavad täiesti juurutatud pilveteenuses on aktiivne passiivne muster mudel.

![Aktiivne passiivne, täielik koopia](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Aktiivne aktiivne

Nüüd, te olete ilmselt välja selgitada, uuendus mustrite: väheneb soovitud RTO suureneb kulud ja keerukuse. Aktiivne-active lahenduse piirid tegelikult selle andmete osas keskmise maksma.

On aktiivne aktiivne muster, on täielikult pilveteenustega ja andmebaasi mõlemad piirkonnad juurutatud. Erinevalt aktiivne passiivne mudel, saavad mõlemad piirkonnad kasutaja liikluse. See suvand annab kiireim taastamise aeg. Teenused on juba mastaabitud käsitlema laadi veebisaidil iga piirkonna osa. DNS-i on juba lubatud kasutada teisene piirkond. On täiendavaid keerukuse kindlaks teha, kuidas kasutajad marsruutimiseks sobiv piirkond. Funktsiooni Round-jaan võib olla võimalik. On tõenäoline, et teatud kasutajate kasutaks teatud piirkond, kus asub nende andmete esmane koopia.

Tõrkesiirde korral lihtsalt keelata DNS-i esmane alale. See marsruudib liikluse kõik teisene piirkond.

Isegi seda mudelit on mõned variandid. Näiteks järgmisel joonisel on esitatud mudeli kui esmane piirkond kuulub andmebaasi juhtslaidi koopia. Pilveteenustega mõlema piirkonna kirjutamise see peamine andmebaasi. Teisene juurutamise saate lugeda esmane või tiražeeritud andmebaasi. Selles näites dispersioonanalüüs toimub üks viis.

![Aktiivne aktiivne](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

On aktiivne aktiivne arhitektuur eelmisel joonisel on negatiivne. Teise regiooni peab esimese piirkonna andmebaasile juurdepääsu, sest seal asub juhtslaidi kopeerimine. Jõudluse langeb oluliselt, kui avate andmete piirkonnast väljaspool. Rist-piirkond andmebaasi kõned, kaaluge teatud tüüpi partiide strateegia need kõned jõudluse parandamiseks. Lisateabe saamiseks vaadake, [Kuidas kasutada SQL-andmebaasi rakenduse jõudluse parandamiseks partiide](../sql-database/sql-database-use-batching-to-improve-performance.md).

Alternatiivne arhitektuur võivad hõlmata iga piirkonna juurdepääs otse oma andmebaasi. Selle mudeli kahesuunaline koopia teatud tüüpi sünkroonimiseks on vaja iga piirkonna andmebaasid.

Aktiivne aktiivne muster, peate pole nii palju eksemplari esmane piirkond, nagu teeksite aktiivne passiivne muster. Kui teil on 10 eksemplari esmane piirkonna aktiivne passiivne arhitektuur, võib juhtuda ainult iga piirkonna aktiivne aktiivne arhitektuur 5. Mõlemad piirkonnad kohe ühiskasutusse laadi. See võib olla kulude kokkuhoid aktiivne passiivne mustri üle, kui hoiate soe valige passiivne piirkonna 10 eksemplari Tõrkesiirde ootamine.

Mõistan, et kuni esmane piirkond taastamiseks teisene piirkond võite saada ootamatu uute kasutajate. Kui määratud on 10 000 kasutajat iga server, kui esmane piirkond kogemusi teenuse häirete, teisene piirkonnas on äkki käsitlema 20 000 kasutajad. Kontrolli reeglid teisene piirkond peab avastada suurendamine ja kahekordseks eksemplarid teisene piirkonna. Lisateavet selle kohta leiate jaotisest [tõrge](#failure-detection)tuvastamise.

##<a name="hybrid-on-premises-and-cloud-solution"></a>Hübriid kohapealse ja pilveteenuse lahenduse

Ühe täiendava strateegia avariitaastet on arhitekt hübriid rakendus, mis töötab kohapealse ja pilveteenuses. Sõltuvalt rakendusest võib esmane piirkond kas asukoht. Kaaluge eelmise arhitektuurides ja oletage, põhi- või piirkond kohapealse asukoht.

Need hübriid arhitektuur on mõned probleemid. Esmalt enamik selles artiklis on adresseeritud PaaS arhitektuur mustrid. Tüüpilised PaaS rakendused Azure sõltuvad Azure'i seotud importida, nt rollid, pilveteenustega ja liikluse haldur. Seda tüüpi PaaS rakendus kohapealse lahenduse loomine nõuaks oluliselt erinev arhitektuur. See ei pruugi olla võimalik halduse või maksumus perspektiivi.

Hübriidjuurutuse avariitaastet lahendus on siiski vähem väljakutsed traditsiooniline arhitektuurides, mis on lihtsalt teisaldatud pilveteenusesse. See kehtib arhitektuurides, mis kasutavad IaaS. IaaS rakendusi kasutada virtuaalmasinates pilves, mis võib olla otsene asutusesisene, võrdlus. Saate virtuaalse võrguga ühenduse pilves masinad kohapealse võrgu ressurssidega. See avab mitmeid võimalusi, mida ei ole võimalik ainult PaaS rakendustega. Näiteks SQL serveri saate ära katastroofi taastamine lahendusi, nt Kättesaadavusrühmad ja andmebaasi peegeldus. Lisateavet leiate teemast [kättesaadavuse ja SQL Server Azure'i virtuaalmasinates katastroofiabi](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

IaaS lahendused pakuvad ka kohapealse rakendusi kasutada Azure'i Tõrkesiirde suvand lihtsam tee. Olemasoleva kohapealse piirkonnas olla täielikult töötavaks rakenduse. Siiski, mida teha, kui teil puudub säilitamiseks geograafiliselt eraldi piirkonnas Tõrkesiirde ressursse? Otsustate kasutada teie rakendus Azure'i virtuaalmasinates ja virtuaalse võrgu. Sel juhul määratleda protsessid, mida saate sünkroonida pilveteenusesse. Azure'i juurutamise seejärel muutub teisene piirkonna jaoks Tõrkesiirde. Esmane piirkond jääb kohapealse rakendus. IaaS arhitektuur ja võimaluste kohta leiate lisateavet teemast [Virtuaalmasinates dokumentatsiooni](https://azure.microsoft.com/documentation/services/virtual-machines/).

##<a name="alternative-cloud"></a>Alternatiivne pilveteenuses

On olukordi, kus isegi töökindluse Microsoft Cloud ei vasta sisemise vastavuse reeglite või poliitikate, mis nõuab teie asutuses. Isegi parim ettevalmistamine ja kujunduse rakendamiseks varukoopia süsteemide õnnetuse ajal ei piisa kui on globaalne teenuse häirete pilve teenusepakkuja.

Mida soovite võrrelda kättesaadavus nõuded hind ja keerukus parem kättesaadavus. Risk analüüsi ja määratlevad RTO ja RPO oma lahenduse. Kui teie rakendus ei saa mis tahes luba, selle võib teha tunde teile kaaluge pilve muu lahendus. Kui kogu Internetis läheb alla, olla teise pilve lahendus saadaval, kui Azure'i muutub globaalselt kättesaamatuks.

Sarnaselt hübriidjuurutuse stsenaariumi puhul saate Tõrkesiirde juurutuste rakenduses eelmise katastroofi taastamine arhitektuurides eksisteerivad teise pilve lahendus. Alternatiivne cloud DR saitide tuleks kasutada ainult lahendusi, mille RTO võimaldab väga vähe tööseisakute. Pange tähele, et lahenduse, mis kasutab DR saidi Azure nõuab rohkem tööd konfigureerimine arendada, juurutada ja hallata. Samuti on keerulisem rakendada rist-cloud arhitektuuri parimaid tavasid. Kuigi pilveteenuste platvormid on sarnane üksikasjalik põhimõtet, API-d ja arhitektuur on erinevad.

Kui otsustate jagada oma DR erinevate platvormide vahel, oleks mõistlik arhitekt lahendus kujunduse võtmiseks kihid. Kui te ei tee seda, pole vaja arendamise ja säilitada kaks erinevaid versioone sama rakenduse eri pilveteenuse platvormid katastroofi korral. Kui koos hübriidjuurutuse stsenaariumi Azure'i Virtuaalmasinates või Azure'i teenus võib kasutada lihtsam sellisel juhul, kui cloud kohased PaaS Kujunduste kasutamine.

##<a name="automation"></a>Automatiseerimine

Mõned mustrite, mida me lihtsalt arutada nõuavad kiirülevaate aktiveerimine ühenduseta juurutuste ka teatud osade süsteemi taastamine. Automaatika või skriptimine toetab aktiveerimine ressursid nõudmisel ja juurutada kiiresti lahendusi. Selle artikli DR seotud automatiseerimine on samastada [Azure PowerShelli](https://msdn.microsoft.com/library/azure/jj156055.aspx), kuid [Teenuse haldamine REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) on ka võimalus.

Arendamise skriptide aitab hallata DR, mis Azure'i ei oska läbipaistev osa. See on kasu tootma ühtsed tulemused iga kord, mis vähendab võimalus inimeste tõrge. On eelmääratletud DR skriptide vähendab ka taastada süsteemi ja selle osade keskel katastroof aeg. Te ei soovi proovida käsitsi aru saada, kuidas taastada saidi samal ajal, kui see on alla ja kaotanud raha iga minut.

Pärast skriptide loomist katsetage neid korduvalt algusest lõpuni. Pärast seda, saate kontrollida oma põhifunktsioone, veenduge, et need testida [katastroofi](#disaster-simulation)simulatsioon. See aitab avastada vigu skriptide või protsessid.

Hea tava automatiseerimine on luua PowerShelli skriptide või käsurea liides (CLI) skriptide Azure tõrkejärgseks hoidla. Selgelt märkida ja liigitada neid lihtne otsingu jaoks. Määrata üks inimene hoidla versioonimise skripte ja haldamiseks. Dokumendi neid hästi selgitused parameetrite ja skripti kasutamise näited. Tagama, et hoiate dokumentide sünkroonis oma Azure juurutuste. See rõhutab eesmärk on üks inimene kõigi osade hoidla eest.

##<a name="failure-detection"></a>Tõrge automaattuvastus

Kättesaadavus ja avariitaastet probleemide õigesti käsitlema peab olema võimalus avastada ja diagnoosimine ebaõnnestumist. Tehke täpsemalt serveri ja juurutamise jälgimine nii saate kiiresti teada, kui süsteemi või selle osade on äkki alla. Tööriistu, mis pilk pilveteenusesse ja sõltuvustega üldise seisundi jälgimine saate teha selle töö. Ühe Microsofti tööriista on [süsteemi Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Kolmanda osapoole tööriistu saate sisestada ka jälgimise võimalusi. Enamik jälgimisega seotud lahenduste jälitada tootluse hinnale ja teenuse kättesaadavus.

Kuigi nende tööriistade on oluline, need ei asenda vajadust vea tuvastamise ja aruandlust pilveteenus kavandamine. Kavatsete peab korralikult kasutada Azure diagnostika. Kohandatud jõudluse hinnale või kirjeid saab ka üldine strateegia. See pakub rohkem andmeid kasutamisel kiiresti probleemi diagnoosimine ja taastada kõiki võimalusi. See sisaldab ka täiendavad mõõdikud jälgimisega seotud tööriistade abil saate määrata rakenduse seisund. Lisateabe saamiseks vt [Lubada Azure diagnostika Azure'i pilveteenustega](../cloud-services/cloud-services-dotnet-diagnostics.md). Kuidas kavandamine "seisund mudel" Üldine arutelu, vt [Failsafe: juhised olles Cloud struktuur](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Katastroofi simulatsioon

Simulatsioon testimine hõlmab loomine väike tegeliku elu olukordi töö floor jälgida, kuidas reageerida meeskonna liikmed. Simulatsioonid ka kuvada, kui tõhusad lahendused on taastamise kava. Teostada simulatsioonid nii, et loodud stsenaariumid ei häiriks tegelik business ajal endiselt tunne reaal olukordades.

Kaaluge architecting tüüpi "lülituskilbid?" rakenduse jäljendamiseks käsitsi probleeme. Näiteks lülitita pehmed kaudu käivitada Accessi andmebaasi erandid on tellimise mooduli põhjustab rikkeid. Saate sarnased kerge hõlbustusvahendite kasutamise muud moodulid võrgu kasutajaliidese tasemel.

Simulatsiooni probleemidest, mis on adresseeritud ei tõsta esile. Jäljendatud stsenaariumid peavad olema täielikult kontrollitavad. See tähendab, et isegi juhul, kui taastamise kava tundub, et ei saa, saate taastada olukord tagasi tavaline ilma mõnda olulist kahju. See on oluline sellest kõrgema taseme haldamise kohta, millal ja kuidas selle simulatsioonõppuste teostada. See leping peaks sisaldama teavet aega ja ressursse, mis võib muutuda ebaproduktiivne mudelkatse töötamise kohta. Kui te olete allutades oma katastroofi taastamise kava test, on ka oluline, et määrata, kuidas edu mõõta.

On mitu teiste meetodite abil, testige katastroofijärgne taastamise kava kasutavate. Enamik neist on siiski lihtsalt muudetud versiooni nende põhimeetodist. Peamine motiiv see testimine on hindamaks, kui võimalik ja kuidas toimiv taastamise plaan on. Avariitaastet testimine keskendub avastada auke põhi taastamise lepingus üksikasjad.

##<a name="next-steps"></a>Järgmised sammud

See artikkel on osa sarjast artiklite [avariitaastet](./resiliency-disaster-recovery-high-availability-azure-applications.md)ja kõrge kättesaadavus loodud Microsoft Azure. Eelmise artiklis toodud selle sarja on [loodud Microsoft Azure kõrge kättesaadavus](./resiliency-high-availability-azure-applications.md).
