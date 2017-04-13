<properties
   pageTitle="Katastroofiabi ja kõrge-saadavus Azure rakenduste | Microsoft Azure'i"
   description="Tehnilised ülevaated ja sügavus teavet rakenduste jaoks kõrge kättesaadavus ja Avariijärgne taaste tugineb Microsoft Azure'i rakenduste kujundamine."
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

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Katastroofiabi ja Microsoft Azure loodud kõrge kättesaadavus

##<a name="introduction"></a>Sissejuhatus

See artikkel keskendub kõrge kättesaadavus töötavad Azure rakendused. Kõrge-saadavus üldise strateegia sisaldab ka avariitaastet osa. Tõrgete ja katastroofide pilveteenuses kavandamise nõuab ebaõnnestumisi kiiresti tuvastada. Seejärel rakendate strateegia, mis vastab teie hälbe rakenduse seisakuaja. Lisaks tuleb arvesse võtta andmete kahju rakenduse saab luba ei põhjusta kahjulike business tagajärgi, nagu see on taastatud.

Enamik ettevõtteid teatavad, et nad on valmis ajutine ja suurte ebaõnnestumist. Enne kui saate vastata enda jaoks, ei ettevõtte Harjuta tõrkeid? Katsetada taastamise andmebaaside, et teil oleks õige protsesside kohas? Ilmselt ei. On põhjus eduka avariitaastet algab paljude plaanimiseks ja rakendamiseks järgmiste protsesside architecting. Nagu paljude muude mitteseotud nõuetele, nagu turvalisus, saab avariitaastet harva võiksite analüüsi ja see nõuab aega eraldatud. Samuti pole enamik ettevõtteid geograafiliselt jaotatud piirkondade liigsete mahuga eelarvest. Seega isegi ülesanne kriitiliste rakenduste sageli ei kuulu proper tegevuse taastamise kavandamine.

Pilveteenuse platvormid, nt Azure, sisestage geograafiliselt hajutatud piirkondade kogu maailmas. Need platvormid pakuvad ka toetavad kättesaadavus ja Avariijärgne taaste stsenaariumid erinevaid võimalusi. Nüüd iga ülesanne kriitilise pilve rakendus võib olla antud silmas katastroofi Õigekeelsuskontroll süsteemi. Azure'i on paindlikkust ja avariitaastet paljude oma teenuste sisse ehitatud. Peate uuring platvormi funktsioonide hoolikalt ja täiendada rakenduse strateegiad.

Selles artiklis antakse ülevaade peaks tegema katastroofi-tõendada on Azure juurutamise vajalikud arhitektuur juhiseid. Seejärel saate rakendada suurema äriprotsessi järjepidevuse. Äriplaan järjepidevus on juhised jätkuva negatiivset tingimustel. See võib olla tehnoloogiaga, nt downed teenuse või loodusõnnetuse, nt torm või elektrikatkestus tõrke. Rakenduse paindlikkust katastroofideks on suurem katastroof taastamise protsess alamhulka, nagu on kirjeldatud selles dokumendis NIST: [Juhtumiga Plaanimisjuhend infotehnoloogia süsteemide](https://www.fismacenter.com/sp800-34.pdf).

Järgmistes jaotistes erinevaid tasemeid tõrkeid, tehnikate ja arhitektuurides, mis toetavad nende meetodite abil saate määratleda. See teave aitab teie katastroofi taastamise protsesside ja toimingute tagamiseks oma katastroofi taastamise strateegia töötab õigesti ja tõhusalt.

##<a name="characteristics-of-resilient-cloud-applications"></a>Olles pilv rakendusi omadused

Ka architected rakendus peab võimalus tõrkeid taktikaline tasemel vastu ja see saab ka luba strategic kogu süsteemi tõrkeid tasemel piirkond. Järgmistes jaotistes määratleda kogu dokumendis olles pilveteenustega jälgib kirjeldamiseks viidatud terminid.

###<a name="high-availability"></a>Kõrge-saadavus

Tugevalt saadaval pilve rakendus rakendab strateegiad katkestuste sõltuvused, nt hallatavate pilve platvormi pakutavaid teenuseid vastu võtta. Seda moodust võimaldab pilve platvormi võimalused ebaõnnestumisi, vaatamata rakenduse jätkata, on oodatud otstarbekas ja mitteseotud süsteemne omadused. Seda käsitletakse põhjalikumat kanali 9 video sarja [Failsafe: juhised olles Cloud struktuur](https://channel9.msdn.com/Series/FailSafe).

Kui rakendate rakendus, peate arvesse võtma võimalus katkestuste tõenäosuse. Lisaks uurida, kuidas mõni katkestuste on rakenduse business seisukohalt enne sügav rakendamise strateegiad. Ilma arvesse business mõju ja tõenäosus pihta risk tingimus, rakendamist, mis võib olla kulukas ja potentsiaalselt mittevajalike.

Kaaluge auto analoogia jaoks kõrge kättesaadavus. Isegi kvaliteediga osad ja ülemuse matemaatika ei takista aeg-ajalt tõrkeid. Näiteks kui auto saab puhkenud, auto endiselt töötab, kuid see töötab halvenenud funktsioonidega. Kui teil on kavas selle võimalikud toimumiskorra, saate üks need õhukesed ääristatud vaba rehvid seni, kuni jõuate parandamine pood. Kuigi varurehv ei luba kiiresti kiirust, te saate ikka olema vabad auto kuni asendate rehvi. Samuti pilveteenuses, mis läheb kaotsi plaanid saate takistada suhteliselt väike probleem kogu rakenduse langetamine. See kehtib ka siis, kui pilveteenusesse tuleb käitada halvenenud funktsioonidega.

Seal on mõned peamised omadused tugevalt saadaval pilveteenustega: tõrketaluvust kättesaadavus ja skaleeritavus. Kuigi need omadused on omavahel seotud, on oluline mõista iga ja kuidas need aitavad lahenduse üldine kättesaadavust.

###<a name="availability"></a>Kättesaadavus

Saadaval rakenduse leiab selle aluseks oleva taristu ja sõltuvad teenused. Saadaolevate rakenduste eemaldamine ühe punkti tõrgete koondamise ja olles kujunduse kaudu. Kui saate laiendada kättesaadavus Azure silmas pidada, on oluline mõista tõhus kättesaadavuse platvormi mõistet. Tõhus kättesaadavus leiab teenuse taseme lepinguid (SLA) iga sõltuvad teenuse ja nende kumulatiivne mõju kogu süsteemi-saadavus.

Süsteemi-saadavus on ajaakna, mis on süsteemi saab kasutada protsendi mõõt. Näiteks kättesaadavus SLA vähemalt kaks eksemplaride Azure'i veebis või töötaja rolli on 99,95% (kontorist väljas 100%). Ei saa mõõta jõudluse ega funktsionaalsus teenuste rollide töötab. Tõhus kättesaadavus oma pilveteenuses ka mõjutatud erinevate SLAs sõltuvad teenused. Veel jooksvat osade süsteemis, rohkem hooldust, võiksite võtta rakendamiseks saate elastselt vastavad kättesaadavus oma lõppkasutajatele.

Võtke arvesse järgmist SLAd Azure'i teenus, mis kasutab Azure teenused: Arvuta, Azure'i SQL-andmebaasi ja Azure Storage.

|Azure'i teenus|SLA   |Võimalikud minutit tööseisakute/kuu (30 päeva)|
|:------------|:-----|:----------------------------------------:|
|Arvuta      |99,95%|21.6 minutit                              |
|SQL-andmebaas |99,99%|4.3 minutit                              |
|Salvestusruumi      |99,90%|43.2 minutit                              |

Plaanite peab kõigi teenuste minna potentsiaalselt korda. Lihtsustatud selles näites on minutit kuus, mis võiks olla alla koguarvu Airlines. 30-päevast kuud on 43 200 minutit. Airlines on.25 protsenti 30-päevast kuud (43 200 minutit) minutite arv. See annab teile tegelik saadavus 99.75 protsenti pilveteenusesse.

Siiski, kasutades kättesaadavuse st saate parandada see. Näiteks kui koostate rakenduse jätkata, kui SQL-andmebaasi pole saadaval, saate eemaldada mis võrrandi. See võib tähendab, et rakendus töötab vähendatud võimalusi, nii on ka ettevõtte nõuetele silmas pidada. Azure'i SLAs täieliku loendi leiate teemast [Taseme lepingute](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Skaleeritavus.

Skaleeritavus mõjutab otseselt kättesaadavus. Rakendus, mis on suurem koormuse nurjub pole enam saadaval. Skaleeritav rakendused on suurenemine ühtsete tulemustega Windowsi lubatud aja täita. Kui scalable, on see skaala horisontaalselt või vertikaalselt haldamiseks tõusu laadi säilitades ühtsete jõudlust. Üldiselt horisontaalne mastaapimine lisab veel masinad sama suur (protsessor, mälu ja läbilaskevõime), samal ajal vertikaalne skaleerimise suureneb olemasolevatest masinad suurust. Azure, peate vertikaalne skaleerimise suvandite valimise erineva masina suurusega Arvuta jaoks. Seadme suuruse muutmine nõuab siiski uuesti juurutamine. Seetõttu on kõige paindlikke lahendusi mõeldud horisontaalne mastaapimine. See kehtib eriti Arvuta, kuna suurendamiseks saate hõlpsalt töötavate veebis või töötaja rolli eksemplaride arvu. Need täiendavad eksemplarid toime suurema liikluse Azure'i veebiportaali, PowerShelli skriptide või kood. Seda otsust kasvu jälgida teatud mõõdikute põhjal. Selle stsenaariumi korral kasutaja jõudluse ega mõõdikute ei tee märgatav drop koormuse. Tavaliselt web ja töötaja rollid laoseisu mis tahes väliselt. See võimaldab paindlik koormusetasakaalustuseks ja graatsiline käsitsemise eksemplari loendab muudatustest. Horisontaalne mastaapimine töötab ka ka teenuseid, näiteks Azure Storage, mis ei paku vertikaalne skaleerimist astmeline suvandid.

Pilveteenuse juurutuste tuleks käsitleda skaala-üksuste kogum. See võimaldab taotlust elastne rakenduses teenindamine lõppkasutajad läbilaskevõime vajadustele. Skaala üksused on lihtsam visualiseerida veebi ja rakenduse serveri tasemel. Selle eest Azure'i juba kodakondsuseta Arvuta sõlmed veebi ja töötaja rollide kaudu. Lisada rohkem arvutada skaala-üksuste juurutamise ei põhjusta rakenduse olekus halduse küljel efektid, kuna Arvuta skaala-üksused on. Salvestusruumi skaala ühikutega vastutab sektsiooni andmete (liigendatud või struktureerimata andmed). Salvestusruumi skaala-üksused, näiteks Azure'i tabeli partition, Azure'i bloobimälu nõu ja Azure SQL-andmebaas. Isegi mitme Azure Storage konto kasutamist mõjutab rakenduse skaleeritavus otseselt. Väga paindlik pilveteenuses lisada mitu salvestusruumi skaala-üksuste peate kujundama. Näiteks kui rakendus kasutab relatsiooniandmete, partition andmed üle mitme SQL andmebaase. See võimaldab salvestusruumi mudeli elastne Arvuta skaala ühik kursis. Samuti võimaldab Azure Storage andmete eraldamine skeeme, mis nõuavad tahtlikult kujundusi Arvuta kiht läbilaskevõime vajadustele. Scalable pilveteenustega kujundamise head tavad loendi leiate teemast [Head tavad suuremahuliste kujundus teenuste Azure'i pilveteenustega](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

###<a name="fault-tolerance"></a>Tõrketaluvust

Rakenduste peaks Oletame, et iga sõltuvad cloud võimalus saate ja läheb mingil hetkel aega. Vea salliv rakenduse tuvastab ja manöövrid ümber nurjunud elemente, jätkamiseks ja tagastada õigeid tulemeid teatud aja jooksul. Vea salliv system võtab siirdamiseks veatingimusi tööle uuesti poliitika. Veel raske vead, saate rakenduse probleemide tuvastamine ja ei õnnestu üle alternatiivne riistvara või erakorralised lepingute kuni selle on lahendatud. Usaldusväärne rakenduse saate õigesti haldamine ühte või mitmesse rikkumine ja jätkake töökorras. Vea salliv rakendusi saate kasutada ühe või mitme kujunduse strateegiaid, nagu koondamise, kopeerimine või halvenenud funktsioone.

##<a name="disaster-recovery"></a>Katastroofiabi

Pilveteenuse juurutamise enam võib funktsioon süsteemset katkestuste sõltuvad teenuste või aluseks infrastruktuuri tõttu. Tingimustel, käivitab järjepidevuse äriplaan katastroofi taastamise protsess. Selle protsessi tähendab tavaliselt toimingute töötajatele nii automatiseeritud toiminguid selleks, et saadaval piirkonnas rakendus uuesti. Selleks, et uues rakenduse kasutajad, andmete ja teenuste üleviimine. See hõlmab ka varukoopia media või poolelioleva kopeerimine.

Kaaluge eelmise analoogia, mida võrrelda kõrge kättesaadavus võimalus taastamine puhkenud vaba abil. Seevastu avariitaastet hõlmab samme pärast auto krahh, kui auto ei ole enam töökorras. Sel juhul parim lahendus on muuta auto, helistades reisimise teenuse või sõbra tõhusalt leidmiseks. Selle stsenaariumi korral seal tõenäoliselt saab olla pikem viivitus saada tagasi teel. Olemas on ka rohkem keerukuse parandamine ja naasevad algse sõiduki. Samal viisil avariitaastet teise regioon on keerukas toiming, mis tähendab tavaliselt mõned tööseisakute ning andmed võivad kaotsi minna. Paremini mõista ja hinnata katastroofi taastamine strateegiad, on oluline määratleda väljendite: taastamise aja eesmärk (RTO) ja taastamise punkti eesmärk (RPO).

###<a name="recovery-time-objective"></a>Taastamise aeg eesmärk

Funktsiooni RTO on maksimaalne aja eraldatud taastamine rakenduse funktsionaalsust. See on business vajaduste järgi ja see on seotud rakenduse tähtsus. Kriitiline äri rakenduste puhul madal RTO.

###<a name="recovery-point-objective"></a>Taastamine punkt eesmärk

Funktsiooni RPO on aktsepteeritav ajaakna kaotanud andmeid tõttu taastamise protsess. Näiteks kui soovitud RPO on üks tund, peab täielikult varundamine või kopeerida andmeid iga tund. Kui te avab rakendus alternatiivse piirkonnas, varundatud andmete võivad puududa kuni tund andmed. Artiklid, nt kriitiliste rakenduste suunata väiksem RPO.

##<a name="checklist"></a>Kontroll-loend

Vaatame summarize põhilistest punktidest, mida selles artiklis (ja selle teemakohast artiklit [kõrge-saadavus](./resiliency-high-availability-azure-applications.md) ja [avariitaastet](./resiliency-disaster-recovery-azure-applications.md) Azure rakenduste) kaetud. Kontroll-loendi üksuste kaaluge oma tegevuse taastamise kavandamine ja teeb kokkuvõtte. Need on põhitõed, mis on kasulik klientidele, kes soovivad saada raske eduka lahenduse rakendamise kohta.

1. Läbi iga rakenduse riskianalüüs, kuna iga võib olla erinevad nõuded. Mõned rakendused on rohkem kui teised ja oleksid õigustatud arhitekt neid tõrkejärgseks lisatasu.
1. Selle teabe abil saate määratleda RTO ja RPO iga rakenduse jaoks.
1. Kujundage rikkumise, alustades rakenduse arhitektuur.
1. Rakendada kõrge-saadavus, head tavad ajal maksumus, keerukuse ja risk.
1. Rakendada katastroofi taastamise ja protsesside.
  * Kaaluge mooduli taseme kuni täieliku cloud katkestuste ulatuvad ebaõnnestumist.
  * Luua varukoopia kõik viide ja kandeandmete strateegiad.
  * Valige mitme saidi katastroof taastamine arhitektuur.
1. Määratleda teatud omanik katastroofi taastamise protsessid, automatiseerimise ja testimine. Omanik peaks hallata ja oma kogu protsessi.
1. Dokumendi protsesside nii, et need on hõlpsasti korratavad. Kuigi on üks omanik, mitu inimest peaks oskama mõista ja järgida protsesside hädaolukorras.
1. Koolitada töötajate rakendada.
1. Tavaline katastroofi simulatsioonid kasutada nii koolitus ja kinnitamise protsess.

##<a name="summary"></a>Kokkuvõte

Kui riistvara või rakenduste ei Azure'is, on erinevas tõrke ilmnemisel operatsioonisüsteemides kohapealse tehnika ja neid hallata strateegiad. Selle üheks põhjuseks on pilve lahendusi tavaliselt rohkem sõltuvused infrastruktuuri, mis on jaotatud üle Azure piirkond ja hallata eraldi teenused. Peab tegelema kõrge-saadavus meetoditega osaline ebaõnnestumist. Haldamiseks raskem tõrgete, võib-olla tõttu katastroofi sündmus, kasutage katastroofi taastamine strateegiad.

Azure'i tuvastab ja teinud palju tõrkeid, kuid on mitut tüüpi tõrkeid, mis nõuavad rakendusele vastav strateegiad. Peate aktiivselt ettevalmistamine ja ebaõnnestumisi rakenduste ja teenuste andmete haldamine.

Kui teie taotlus kättesaadavus ja katastroofi taastamise kava loomise kaaluge business rakenduse rikkumisest tulenevad tagajärjed. Määratlemise protsessid, poliitika ja toimingute kriitiliste süsteemide taastamiseks pärast katastroofiline sündmus võtab aega, kavandamise ja kohustuse. Ja kui loote lepingud, mida ei saa lõpetada seal. Peate regulaarselt analüüsida, testige ja pidevaks täiustamiseks rakenduse oma haldusalas, ärivajaduste ja saadaolevate tehnoloogiad lepingud. Azure'i pakub uute võimaluste ja tõstatab uued väljakutsed luua töökindlaid rakendusi, et ebaõnnestumist.

##<a name="additional-resources"></a>Lisaressursid

[Microsoft Azure loodud kõrge kättesaadavus](resiliency-high-availability-azure-applications.md)

[Avariitaastet rakenduste Microsoft Azure'i sisse ehitatud](resiliency-disaster-recovery-azure-applications.md)

[Azure'i paindlikkust tehnilise abi saada?](resiliency-technical-guidance.md)

[Ülevaade: Cloud business järjepidevus ja andmebaasi Õnnetusejärgne taastamine SQL-andmebaas](../sql-database/sql-database-business-continuity.md)

[Kõrge kättesaadavus ja Avariijärgne taaste SQL Server Azure'i Virtuaalmasinates](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[Failsafe: Juhised olles cloud struktuur](https://channel9.msdn.com/Series/FailSafe)

[Suuremahuliste teenust Azure pilveteenused kujunduse head tavad](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Järgmised sammud

Selles artiklis on mitmeid artikleid avariitaastet ja kõrge-saadavus Azure rakenduste osa. Järgmise artiklis toodud selle sarja on [loodud Microsoft Azure kõrge kättesaadavus](resiliency-high-availability-azure-applications.md).
