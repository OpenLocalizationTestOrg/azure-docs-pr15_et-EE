<properties 
   pageTitle="Microsoft jälgimine toote võrdlus | Microsoft Azure'i"
   description="Microsoft toimingute komplekti (OMS) on Microsofti pilveteenuste vastavalt selle lahendus, mis aitab hallata ja kaitsta oma kohapealse ja pilveteenuse taristu.  Selles artiklis tuvastab kaasatud OMS erinevad teenused ja nende üksikasjalik sisu lingid."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>Microsoft jälgimisega seotud toote võrdlus

Selles artiklis antakse System Center toimingute Manager (SCOM) ja Log Analytics toimingud halduse komplekti (OMS) võrdlus arhitektuuri, kuidas need ressursid jälgida ja kuidas nad teevad kogutud andmete analüüs.  See on teile olulise mõista nende erinevusi ja suhteline tugevuste.  

## <a name="basic-architecture"></a>Tavaline arhitektuur
### <a name="system-center-operations-manager"></a>System Center Toiminguhalduri

Kõik SCOM komponentide installimist oma data Centeri kaudu.  Windows ja Linux masinad, mida haldab SCOM [agentide on installitud](http://technet.microsoft.com/library/hh551142.aspx) .  Agentide ühenduse [Management serverid](https://technet.microsoft.com/library/hh301922.aspx) SCOM andmebaasi ja andmete ladu suhelda.  Agentide toetuvad domeeni autentimise halduse serveritega ühenduse loomiseks.  Usaldatud domeeni selliste serdi autentida või ühenduse [Lüüs Server](https://technet.microsoft.com/library/hh212823.aspx).

SCOM nõuab kahte SQL andmebaase, üks andmeid ja muu andmebaas toetama, aruandlus ja andmete analüüsimiseks.  [Aruandlusteenuste Server](https://technet.microsoft.com/library/hh298611.aspx) töötab SQL-i aruandlusteenuste aruande andmete põhjal andmebaas. 

SCOM saate jälgida abil halduse tuvastamine toodete näiteks [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html) [Azure'i](https://www.microsoft.com/download/details.aspx?id=38414)ning [Office 365](https://www.microsoft.com/download/details.aspx?id=43708)pilve ressursse.  Halduse komplektid kasutada ühe või mitme kohaliku agentide puhverserverite avastanud cloud ressursside ja käivitatud töövoogude jõudluse ja-saadavus.  Puhverserveri agentide kasutatakse ka [kuvari võrguseadmete](https://technet.microsoft.com/library/hh212935.aspx) ja muud välised ressursid.

Toimingud konsooli on Windowsi rakendus, mis ühendab üks halduse serverid ning võimaldab vaadata ja kogutud andmete analüüsimine ja konfigureerimiseks SCOM keskkonnas administraator.  Veebipõhise konsooli võib olla mis tahes IIS-i serveris majutatud ja pakub Andmeanalüüs brauseri kaudu.

![SCOM arhitektuur](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics

Enamik OMS komponendid on Azure pilves, nii et saate juurutada ja hallata vaevaga ja haldamiseks.  Log Analytics kogutud kõik andmed on salvestatud OMS hoidla.

Log Analytics saab koguda andmeid kolme allikatest:

- Füüsilise ja virtuaalmasinates Windows ja [Microsoft jälgimise Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) või Linux ja [Toimingute haldamise komplekti Agent Linuxi jaoks](https://technet.microsoft.com/library/mt622052.aspx).  Need masinad saab kohapealse või virtuaalmasinates Azure'i või teise pilve.
- Azure Storage konto [Azure diagnostika](../cloud-services/cloud-services-dotnet-diagnostics.md) kogutud Azure töötaja rolli, web rolli või virtuaalse masina järgi.
- [Ühenduse SCOM halduse rühma](https://technet.microsoft.com/library/mt484104.aspx).  Selle konfiguratsiooni puhul agentide SCOM management serverid, mis annavad andmed SCOM andmebaasiga, kus see siis saadetakse OMS andmesalve suhelda.
Administraatorid kogutud andmete analüüsimine ja konfigureerimine Log Analytics OMS portaal, mis majutab Azure ja mis tahes brauseris avada.  Neile andmetele juurdepääsuks mobiilirakendused on saadaval, standard platvormid.

![Kasutusanalüüsi arhitektuur](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>SCOM ja Log Analytics integreerimine

Kui SCOM andmeallikana Log Analytics abil saate kasutada nii toodete jälgimise keskkonna hübriid funktsioone.  Saate konfigureerida olemasoleva SCOM agentide tegevuse konsooli haldab OMS, jätkab halduse paketid pidamine SCOM kaudu.  
Ühendatud SCOM haldus jaotises andmete saadetaks Log Kasutusanalüüsi neli meetoditega:

- Sündmuste ja tulemustega seotud andmete kogumise agent ja SCOM toimetatud.  Halduse serverites SCOM siis oma andmed Log Analytics.
- Mõned sündmusi, näiteks IIS-i logid ja turvalisuse sündmused edasi toimetada otse Log Analytics agendi.
- Mõned lahendused on esitamisel agent täiendavat tarkvara või nõuda täiendavate andmete kogumine olema installitud tarkvara.  Andmed tavaliselt saadetakse otse Log Kasutusanalüüsi.
- Mõned lahendused koguda andmeid otse SCOM management serverid, mis ei ole pärit agent.  Näiteks [teatiste haldamise lahendus](https://technet.microsoft.com/library/mt484092.aspx) kogub teatiste SCOM pärast seda, kui need on loodud.

## <a name="monitoring-logic"></a>Loogika jälgimine
SCOM ja Log Analytics töötamine kogutud agentide sarnaseid andmeid, kuid on olulised erinevused, kuidas need määratleda ja rakendada oma loogika andmete kogumise ja kuidas need nad koguda andmeid analüüsida.

### <a name="operations-manager"></a>Suurus
Jälgimine loogika SCOM rakendatakse [halduse paketid](https://technet.microsoft.com/library/hh457558.aspx) , mis sisaldavad loogika otsimise komponendid jälgimiseks, mõõte seisundi ja nende komponentide kogumiseks andmete analüüsimiseks.  Jälgida andmeid võivad olla lihtsad, sisaldades kogumiseks mõni sündmus või jõudluse kokkuvõtete või see võib kasutada keerukate loogika skripti rakendada.  Halduse paketid, mis sisaldavad täielik jälgimine on saadaval erinevaid [Microsoft ja muude tootjate rakenduste](http://go.microsoft.com/fwlink/?LinkId=82105) lisaks riist- ja võrgu seadmed.  Saate [Autor oma halduse paketid](http://aka.ms/mpauthor) jaoks kohandatud rakendused.

Juhtimise sisaldavad mitut töövood, et iga sooritab mõned erinevate järelevalve funktsiooni, näiteks proovide jõudluse vastuolus, teenuse oleku kontrollimine või skripti.  Iga töövoog käivitatakse sõltumatult ja määratleb oma tulemusi, nt milliseid andmebaas selle kirjutab ja kas see loob teatise. 

Saate alistada, nagu nad käivitada, sagedus lävi, kui nad peavad tõrge töövoo üksikasjad ja nad luua teatise tõsidust.  Saate sisestada ka lisafunktsioone, lisades oma töövood.

![Alistab](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Halduse paketid on installitud Toiminguhalduri andmebaasi ja agentide management serverid jaotatud automaatselt.  Iga agent automaatselt alla halduse tuvastamine ja laadimine töövoogude oluline neil on arvutisse installitud rakendused.  Agendi poolt kogutud andmete toimetatakse tagasi management serveri SCOM andmebaasi ja andmete ladu paigutamiseks.  Toimingud konsooli võimaldab teil vaadata ja kohandatud vaateid, armatuurlaudade ja aruannete kaasatud halduspakett toiminguhalduri kaudu andmeid analüüsida.

Järgmisel joonisel on kujutatud jaotumine halduse paketid.

![Juhtimise pack meilivoo](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Sündmuste ja jõudluse saidikogumi
Log Analytics kogub sündmuste ja jõudluse hinnale agent Systemsi allikatest, nt Windowsi sündmuselogi, IIS-i logid ja logi abil.  Saate määratleda kriteeriumid, mille andmete kogumine Log Kasutusanalüüsi portaali kaudu ja seejärel looge Logi päringud kogutud andmete analüüsimiseks.  Standardse kriteeriumid on määratletud kui loote oma OMS tööruumi ja saate määratleda teatud kindlate taotluste kohta. 

![Log Analytics sündmuselogide määratlemine](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Kuigi SCOM on palju üksikasjalik töövooge, mis tavaliselt määratlevad kindlate kriteeriumide andmete ja toiming, mida tuleb teha vastuseks, Log Analytics on üldisema kriteeriumid andmete kogumine.  Log päringuid ja lahenduste paku veel suunatud kriteeriumide analüüsida ja kui see on kogutud pilveteenuses kindlate andmete põhjal.

#### <a name="solutions"></a>Lahendused
Lahenduste täiendavad loogika ette andmete kogumine ja analüüs.  Saate valida lahendusi lahenduse galeriist OMS tellimusse lisada.

![Lahendusegalerii](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Lahenduste käivitamine peamiselt analüüsi sündmuste ja jõudluse hinnale kogutud OMS hoidla pilveteenuses.  Samuti võib kindlaks määrata täiendavate andmete kogumise, mis võib analüüsida või esitatud lahendus OMS armatuurlaua täiendavad kasutajaliides, mille Log päringuid. 

Näiteks [muutuste jälitamise lahenduse](https://technet.microsoft.com/library/mt484099.aspx) tuvastab konfiguratsioonimuudatuste operatsioonisüsteemides agent ja kirjutab sündmuste võib analüüsida mitme graafilise vaatega, et Summeeri OMS hoidlasse tuvastatud muudatused.  Saab süvitsi summeeritud log päringuid kogutud lahendus üksikasjalikud andmed kuvatakse vaate kaudu.


Kuigi saate valida oma tellimusele lisada lahendusi, pole praegu võimalus luua oma lahendusi.  Saate valida sündmuste ja jõudluse hinnale koguda ja kohandatud vastavalt oma Logi päringud vaadete loomine.

Log Analytics jälgimisega seotud loogika on kokku võetud järgmisel joonisel.

![Log Kasutusanalüüsi lahenduse kulgemist](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Seisundi jälgimine
### <a name="operations-manager"></a>Suurus
SCOM saate mudel rakenduse erinevate osade ja pakkuda reaalajas seisund iga.  See võimaldab teil mitte ainult tuvastatud Kuva tõrked ja jõudluse aja jooksul, vaid ka valideerimiseks tegelik rakenduse või süsteemi seisundi ja iga osa, mis tahes ajal.  Kuna seda mõistab aja jooksul, mis on saadaval, peab mootori seisund SCOM toetab ka teenuse taseme lepinguid (SLA) mis analüüsimiseks ja aruandluseks kättesaadavus rakenduse aja jooksul.

Näiteks alltoodud vaade kuvatakse reaalajas seisundi SQL-i andmebaasi mootorite SCOM jälgida.  Iga andmebaaside ühe andmebaasi mootorite seisundi kuvatakse poole vaate allosas.

![Maakond kuvamine](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Seisund Exploreri ühe andmebaasi mootorite on näidatud allpool kuvari, mis määratletakse oma üldist tervist.  Nende kuvari on määratletud halduspaketi SQL-i ja käivitada kogu SQL-i andmebaasi mootorite avastanud SCOM vastu.

![Seisund explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Mitme operatsioonisüsteemides komponentide saab kombineerida mõõta seisundi jaotatud rakendus.  See võib olla abiks eriti ärirakenduste, mis sisaldavad mitut jaotatud komponendid.  Saate luua mudeli, et iga komponendi selle rollup kättesaadavus rakenduse sisse.

Active Directory on kujutatud ühe halduspaketi sisaldava mudeli selle jaotatud komponentide analüüsimiseks.  Alloleval joonisel näidis kuvatakse seisundi üldine keskkonna ja seos domeenikontrollerid metsad ja domeenid.  Kõigi nende komponentide sisaldab komponentide ja mitme kuvari sarnaneb eeltoodud näites SQL-i.

![SCOM diagrammivaate](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
OMS ei sisalda levinud mootor mudeli rakendustele või mõõta oma reaalajas seisund.  Tulekul võib hinnata üldise kogutud andmete põhjal teenuste seisundi ja need installida kohandatud loogika agent reaalajas analüüsi läbiviimiseks.  Kuna lahenduste käivitada hoidlasse OMS juurdepääsuga pilves, nad võivad pakkuda sageli põhjalikult analüüsida, kui on tavaliselt läbi halduse paketid. 

Näiteks [AD hindamise ja SQL-i hindamise lahenduste](https://technet.microsoft.com/library/mt484102.aspx) kogutud andmete analüüsimine ja sisestage reiting keskkonna erinevaid aspekte.  See sisaldab soovitused täiustused, mida saab teha kättesaadavus ja keskkonna jõudluse parandamiseks.

![AD Assessment lahendus](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Andmeanalüüs
SCOM ja Log Kasutusanalüüsi pakuvad erinevaid funktsioone kogutud andmete analüüsimiseks.  SCOM on vaadete ja armatuurlaudade toimingud konsooli mitmesuguseid vorminguid ja aruannete esitamise andmed tabelina andmebaas tehtud andmete analüüsimiseks.  Log Analytics pakub päringukeel täielik Logi ja kasutajaliidese OMS hoidla andmete analüüsimiseks.  Kui SCOM kasutatakse andmeallikana Log Analytics, hoidla sisaldab SCOM kogutud nii Log analüütilisi tööriistu saab kasutada nii andmete analüüsimiseks.

### <a name="operations-manager"></a>Toiminguhalduri

#### <a name="views"></a>Vaated
Toimingud konsooli vaated võimaldavad teil kuvada erinevate andmetüüpidega kogutud SCOM muus vormingus, tavaliselt tabeli sündmusi, teatiste ja riigi andmed, ja joone graafikud jõudlusandmeid.  Vaadete minimaalsete analüüsi või andmete konsolideerimine, kuid võimaldab teatud kriteeriumide alusel filtreerida. 

![Vaated](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Tavaliselt annab halduse paketid toetavad rakenduse või süsteem, mis jälgib see mitme vaate.  See võib sisaldada olekus vaated erinevad objektid, millega halduspakett toiminguhalduri leiab, teatiste vaadete tuvastatud probleemide ja jõudluse vaated hinnale.

Vaadete sobivad eriti keskkonnas, sh avatud teatiste hetkeseisu ja jälgida süsteemid ja objektide seisundioleku analüüsimine.  Saate minna allapoole üksikasjalikud sündmuse või jõudlusandmeid toetamine kindla teatise, diagnoosimine selle põhjus. Samuti saate vaadata jõudlus ja seisund eri osade taotluse hinnake oma praegune seisund.

#### <a name="dashboards"></a>Armatuurlauad
Armatuurlaudade toimingud konsooli peamiselt töötada samade andmete vaated, kuid on kohandatav ja võivad hõlmata rikkalikumat visualiseeringud.  Standardse armatuurlauad on saadaval, et saate hõlpsasti kohandada teie enda.  Samuti saate PowerShelli vidina, mida saab kuvada PowerShelli päringu tagastatud andmeid.

![Armatuurlaua](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Arendajad on võimalus lisada kohandatud komponendid armatuurlaudade kaasatakse nende halduse paketid.  Need võivad tuleb tugevalt eraldi kindla rakendus, nt armatuurlaua SQL-i halduse Pack allpool näidatud.  Armatuurlaua saate kasutada ka mallina kohandatud versioonid.

![SQL-i armatuurlaud](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Aruanded
Aruannete SCOM analüüsida andmed tabelina andmebaas.  Ta saab printida ja ajastatud automatiseeritud kohaletoimetamise muudes failivormingutes koos PDF-i, CSV ja Word.  Aruannete töötada nii, et need sobivad eelkõige pikaajaline trendide analüüsi andmete andmebaas.

Tavaliselt annab halduse paketid mõne kindla rakenduse kohandatud aruandeid.  Samuti võite märkida ruudu teegist üldise aruandeid, mida kohandades saate oma rakendustes või erakorralised analüüsi tegemine.

Järgmine on nähtaval Active Directory halduspaketi kogutud andmete Näidisaruande jõudlust.

![Aruanne](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
Log Analytics on [päringukeele](https://technet.microsoft.com/library/mt484120.aspx) , mille abil saate teha analüüsi üle mitme rakendustest pole vaja luua kohandatud vaate või aruande andmete.  Kuna OMS rakendatakse pilves, päringute ja andmete analüüsi ei ole kehtivad piirangud riistvara ja saate kiiresti teha päringuid, sh miljonid kirjed. 

Päringute Kasutusanalüüsi Log on ka muid funktsioone aluseks.  Saate päringu salvestamine, eksportimine Excelisse tulemuste või on see automaatselt käivitada regulaarse intervalliga ja teatise luua, kui selle tulemused vastavad teatud kriteeriumide.  

![Logi päringu meilivoo](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Allpool on kujutatud Log Analytics päringu.  Selles näites kõik sündmused, kus "alustatud" nimi on tagastatud ja Rühmitusalus sündmuse ID-d.  Kasutaja pakub päringu ja Log Kasutusanalüüsi dünaamiliselt genereerib kasutajaliideses analüüsima.  Valides loendist mõni üksuse üksikasjalik sündmuse andmeid tagastada.

![Log päring](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Lisaks sihtotstarbelise analüüsi, saate päringuid Log Analytics edaspidiseks kasutamiseks salvestada ja lisatakse ka [OMS armatuurlaud](http://technet.microsoft.com/library/mt484090.aspx) , nagu on näidatud järgmises näites.

![OMS armatuurlaud](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Järgmised sammud

- Juurutada [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics)kasutajaks registreerumist.  
