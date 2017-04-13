<properties 
   pageTitle="Mis on StorSimple? | Microsoft Azure'i" 
   description="Kirjeldab StorSimple tiering, seade, virtuaalse seadme, teenuste ja mäluhaldus ja tutvustab võtme terminite StorSimple." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000 sarja: hübriid pilve salvestusruumi lahendus

## <a name="overview"></a>Ülevaade

Tere tulemast rakendusse Microsoft Azure'i StorSimple, integreeritud salvestusruumi lahenduse, haldab salvestusruumi tasks kohapealse seadmed ja Microsoft Azure'i salvestusruumi. StorSimple on tõhusa, kulutõhus ja hõlpsasti hallatava salvestusruumi ala võrgu (SAN) lahenduse, mis välistab paljude probleemide ja ettevõtte salvestusruumi ja andmekaitsega seotud. Kasutab kaitstud StorSimple 8000 sarja seade, integreerub pilveteenustega ja pakub Haldusriistad komplekti sujuvalt vaate kõik ettevõtte salvestusruumi, sh salvestusruumi. (Veebisaidil Microsoft Azure'i StorSimple juurutamise kehtib ainult StorSimple 8000 seeria seadmed. Kui kasutate StorSimple 5000/7000 sarja seade, avage [StorSimple abi](http://onlinehelp.storsimple.com/).)

StorSimple kasutab [tiering salvestusruumi](#automatic-storage-tiering) haldamine salvestatud andmeid mitmest eri salvestusruumi meediumi. Praeguse töökomplekt on talletatud kohapealse ühtlase olekus draividel (SSD), kõvaketta draivid (HDD-d) on talletatud andmed, mida kasutatakse harvem ja arhiivimise filtreerima pilveteenusesse. Lisaks StorSimple kasutab korduste eemaldamise ja detailsuse andmed tarbib, et vähendada. Lisateabe saamiseks minge [korduste eemaldamise ja detailsuse](#deduplication-and-compression). Määratlused muude võtme ja StorSimple 8000 sarja dokumentides kasutatud põhimõtet, minge [StorSimple terminoloogia](#storsimple-terminology) käesoleva artikli lõpus.

StorSimple Update 2, kus saate tuvastada üldine nimega *kohalikult kinnitatud* lähteandmeid jääb kohaliku seadmega ja mitte taseme pilveteenusesse. See võimaldab teil käivitamiseks töökoormus, mis on tundlik cloud latentsus, nt SQL-i ja virtuaalse masina töökoormus, draividel kohalikult kinnitatud kasutada pilveteenuses varundamiseks. Kohalik kinnitatud mahud kohta leiate lisateavet teemast [kasutamine StorSimple halduri teenuse haldamiseks maht](storsimple-manage-volumes-u2.md). 

Värskenduse 2 võimaldab teil luua StorSimple virtuaalse seadmete madal latentsused ja suure jõudlusega esitatud Azure premium mälu eeliseid. StorSimple premium virtuaalse seadmete kohta leiate lisateavet teemast [Deploy ja Azure StorSimple virtuaalne seadme haldamine](storsimple-virtual-device-u2.md). Azure premium mälu kohta lisateabe saamiseks külastage [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](../storage/storage-premium-storage.md).

Lisaks mäluhaldus StorSimple andmete kaitse funktsioonid võimaldavad teil luua nõudmisel ajastatud varukoopiaid ja kohalikult või pilveteenuses talletada. Varukoopiate võetakse vormi suureneva pilte, mis tähendab, et ta saab luua ja taastada kiiresti. Pilveteenuse hetktõmmiseid võib olla kriitilise tähtsusega Avariijärgne taaste stsenaariumid, sest need asendamine teisene salvestusruumi süsteemide (nt lint varukoopia) ja võimaldab teil andmeid oma andmekeskuse või alternatiivse saitide vajadusel taastada.

![video ikoon](./media/storsimple-overview/video_icon.png) Vaadake videot: Sissejuhatus Microsoft Azure'i StorSimple jaoks.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>Miks kasutada StorSimple?

Järgmises tabelis kirjeldatakse olulisi eeliseid, mida pakub Microsoft Azure'i StorSimple.

| Funktsioon | Kasu |
|---------|---------|
|Läbipaistva integreerimine | Microsoft Azure'i StorSimple kasutatakse varjatult link andmete hoidlate iSCSI Protocol (protokoll). See tagab pilveteenuses, veebisaidil andmekeskuses, andmete või remote serverites kuvatakse hoida ühes kohas.|
|Vähendatud salvestusruumi kulud|Microsoft Azure'i StorSimple eraldab piisavalt kohalik või salvestusruumi vastavad praegusele nõudmistele ja laiendab salvestusruumi ainult siis, kui see on vajalik. See vähendab veelgi salvestusruumi nõuded ja kulud kõrvaldamise liigsete andmete (korduste eemaldamise) versiooni või tihendamise abil.|
|Lihtsustatud mäluhaldus|Microsoft Azure'i StorSimple pakub süsteemi haldustööriistade, mille abil saate konfigureerida ja hallata talletatud andmed asutusesiseselt, kaugserverisse ja pilveteenuses. Lisaks saate hallata varundamine ja taastamine funktsioonid Microsoft Management Console (MMC) lisandmoodul. StorSimple pakub eraldi, valikuline liides, mille abil saate laiendada StorSimple haldus ja andmete turvamise teenuseid SharePoint serveris talletatud sisu. |
|Täiustatud katastroofiabi ja nõuetele vastavus|Microsoft Azure'i StorSimple ei nõua laiendatud taastamise aeg. Selle asemel see taastab andmeid on vaja. See tähendab, et toiminguid saate jätkata minimaalsete segadusi vältida. Lisaks saate konfigureerida Määrake varukoopia ajakavasid ja andmete säilitamise poliitikat.
|Andmete mobility|Microsoft Azure'i pilveteenuste andmeid pääseb muudelt saitidelt taastamine ja migreerimise eesmärkidel. Lisaks saate StorSimple konfigureerimiseks StorSimple virtuaalne seadmed töötavad Microsoft Azure'i virtuaalmasinates (VMs). VMs saate virtuaalse seadmete juurdepääsu salvestatud andmeid testi või taastamine.|
|Pilve väliselt tugi |Tarkvara StorSimple 8000 seeria update 1 või uuem versioon toetab Amazon S3 RRS, HP ja OpenStack cloud services kui ka Microsoft Azure'i. (Te vajate ikkagi Microsoft Azure storage konto seadme juhtimise otstarbel.) Lisateabe saamiseks minge [mis on uut rakenduses Update 1.2](storsimple-update1-release-notes.md#whats-new-in-update-12).|
|Süsteemi järjepidevuse tagamine | Värskendus 1 või uuem versioon sisaldab uue migreerimise funktsioon, mis võimaldab StorSimple 5000-7000 sarja kasutajad oma andmete siirdamiseks StorSimple 8000 sarja seade.|
|Kättesaadavus valitsuse Azure'i portaalis | StorSimple Update 1 või uuem versioon on Azure Governmenti portaalis. Lisateabe saamiseks vt [Deploy seadme kohapealse StorSimple Government portaalis](storsimple-deployment-walkthrough-gov.md).|
|Andmekaitse ja kättesaadavuseks. | StorSimple 8000 seeria Update 1 või uuem versioon toetab Zone liigsete mälu (ZRS), lisaks kohalikult liigsete salvestusruumi (LRS) ja geograafilise liigne salvestusruumi (GRS). Lugege [artiklit Azure Storage koondamise suvandite](https://azure.microsoft.com/documentation/articles/storage-redundancy/) ZRS üksikasju.|
|Oluliste rakenduste tugi | StorSimple Update 2, kus saate tuvastada kohaliku kinnitatud üldine. See funktsioon tagab, et kriitiliste rakenduste andmed on pole mitmetasandilise pilveteenusesse. Kohalik kinnitatud maht ei kuulu cloud latentsused või ühenduvusprobleemide. Kohalik kinnitatud mahud kohta leiate lisateavet teemast [kasutamine StorSimple halduri teenuse haldamiseks maht](storsimple-manage-volumes-u2.md).|
|Madal latentsus ja suure jõudlusega | StorSimple Update 2 võimaldab teil luua virtuaalse seadmete kasutama suure jõudlusega, madal latentsus funktsioone Azure premium salvestusruumi. StorSimple premium virtuaalse seadmete kohta leiate lisateavet teemast [Deploy ja Azure StorSimple virtuaalne seadme haldamine](storsimple-virtual-device-u2.md).|

![video ikoon](./media/storsimple-overview/video_icon.png) vaadake [Sellest videost](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) leiate ülevaate StorSimple 8000 sarja funktsioonid ja eelised.

## <a name="storsimple-components"></a>StorSimple komponendid

Microsoft Azure'i StorSimple lahendus sisaldab järgmised komponendid.

- **Microsoft Azure'i StorSimple seadme** – kohapealse hübriid salvestusruumi massiiv, mis sisaldab SSDs ja kõvaketast koos liigsete kontrollerid ja automaatne Tõrkesiirde võimalused. Kontrolörid Halda salvestusruumi tiering, hoidke praegu kasutatud (või kuum) andmete kohaliku salvestusruumi (jaotises seadme või kohapealse serverid), liikudes vähem sageli kasutatud andmeid pilve.
- **StorSimple virtuaalse seadme** – tuntud ka kui StorSimple virtuaalse seadme, see on tarkvara versiooni StorSimple seadme, mille tiražeerib arhitektuur ja füüsilise hübriid mäluseadmesse enamik võimalusi. StorSimple virtuaalse seade töötab ka Azure virtuaalse masina ühe sõlme. Premium virtuaalse seadmed, mis Azure premium mälu ära, on saadaval Update 2 ja uuemad versioonid.
- **StorSimple halduri teenuse** – Azure klassikaline portaali, mille abil saate hallata StorSimple seadme või StorSimple virtuaalse seadme ühe web kasutajaliidese kaudu pikendada. Saate luua ja teenuste haldamine, vaadata ja seadmete haldamine, teatiste vaatamiseks, maht, hallata ja vaadata ja hallata varukoopia poliitika ja varukoopia kataloogi StorSimple halduri teenuse.
- **Windows PowerShelli StorSimple** – käsurea liides, mille abil saate hallata StorSimple seade. Windows PowerShelli StorSimple on funktsioonid, mis võimaldab teil StorSimple seadme registreerimine, konfigureerida võrgu liidese oma seadmes, teatud tüüpi värskenduste installimiseks, tõrkeotsing seadme tugi seansi kasutades ja muutke seadme olek. Pääsete juurde Windows PowerShelli StorSimple ühenduse järjestikune konsooli või remoting Windows PowerShelli abil.
- **StorSimple azure PowerShelli cmdletid** – kogumi Windows PowerShelli cmdlet-käsud, mis võimaldavad teil teenusetaseme ja migreerimise automatiseerimine käsurea kaudu. Lisateavet Azure PowerShelli cmdletid StorSimple minge [cmdleti viide](https://msdn.microsoft.com/library/dn920427.aspx).
- **StorSimple hetktõmmise halduri** – MMC lisandmooduli kasutava helitugevuse rühmad ja Windowsi draivi teenus vari rakenduse ühtsete varukoopiate loomiseks. Lisaks saate luua varukoopia ajakavasid ja klooni või taastada mahud StorSimple hetktõmmise halduri. 
- **SharePointi StorSimple adapterit** – tööriista, mis läbipaistev laiendab Microsoft Azure'i StorSimple salvestusruumi ja andmekaitse parkides, SharePoint serveris, tehes StorSimple salvestusruumi vaadatav ja mõistliku SharePointi administreerimiskeskuse portaalis.

Alloleval joonisel pakub Microsoft Azure'i StorSimple arhitektuur ja komponentide ületaseme vaadet.

![StorSimple arhitektuur](./media/storsimple-overview/overview-big-picture.png)

Järgmistes jaotistes kirjeldatakse iga nende komponentide täpsemalt ning selgitavad, kuidas lahendus korraldab andmeid, määrab mälu ja hõlbustab mäluhaldus ja andmekaitse. Viimases osas pakub mõned olulised ja mõistetest seotud StorSimple komponendid ja nende juhtimine.

## <a name="storsimple-device"></a>StorSimple seadme

Microsoft Azure'i StorSimple seade on kohapealse hübriid salvestusruumi massiiv, mis pakub esmased ja iSCSI juurdepääsu sellel talletatud andmed. See haldab salvestusruumi suhtlemine ja aitab tagada turvalisuse ja konfidentsiaalsusega kõik andmed, mis on talletatud Microsoft Azure'i StorSimple lahendus.

StorSimple seadme sisaldab SSD ja kõvaketta draivid HDD-d, samuti klastrite ja automaatne Tõrkesiirde tugi. See sisaldab ühiskasutusega protsessor, jagatud salvestusruumi ja kahe peegelpilt kontrollerid. Iga selle domeenikontrolleri sisaldab järgmist:

- Ühenduse hosti
- Kuni kuus võrgu pordid ühenduse kohtvõrgu (LAN)
- Riistvara jälgimine
- Mittelenduv muutmälu (NVRAM), mis säilitab teabe isegi siis, kui power on katkenud
- Kobar-arvestada serverites tõrkesiirdeklastrist tarkvaravärskendusi hallata nii, et värskendused on minimaalne värskendamine või ei mõjuta teenuse kättesaadavus
- Klaster teenus, mis toimib nagu tagaandmebaas kobar, pakkudes kõrge-saadavus ja minimeerimine negatiivset mõju, mis võivad ilmneda juhul, kui mõni HDD või SSD nurjus või on võrguühenduseta

Mis tahes hetkel aktiivne on ainult üks. Kui aktiivne kontrolleril nurjub, teine kontrolleril aktiveerub automaatselt. 

Lisateabe saamiseks minge [StorSimple riistavara ja olek](storsimple-monitor-hardware-status.md).

## <a name="storsimple-virtual-device"></a>StorSimple virtuaalse seade

StorSimple abil saate luua tiražeerib arhitektuur ja võimalusi füüsilise hübriid mäluseadmesse virtuaalse seadet. Ühe sõlme Azure'i virtuaalarvuti töötab StorSimple virtuaalse seadme (tuntud ka kui StorSimple virtuaalse seadme). (Virtuaalne seadme saab luua ainult arvutis Azure virtuaalne. Te ei saa luua StorSimple seadme või kohapealse serverisse.) 

Virtuaalne seade on järgmised funktsioonid.

- See toimib nagu füüsilise seadme ja pakkuda iSCSI liidese virtuaalmasinates pilveteenuses. 
- Saate luua piiramatu arv virtuaalne seadmed pilves, ja neid sisse ja välja lülitada vastavalt vajadusele. 
- See aitab simuleerida kohapealsete keskkondadega avariitaastet, arendamise ja testi stsenaariumid ja aitavad Üksusetaseme andmetoomisteenused varukoopiate põhjal. 

Värskenduse 2 ja uuemates versioonides StorSimple virtuaalse seade on saadaval kaks mudelit: 8010 seade (varem 1100 mudeli) ja 8020 seade. 8010 seade on maksimaalne maht on 30 TB. 8020 seade, mis kasutab Azure premium mälu ära, on maksimaalne maht on 64 TB. (Rakenduses kohaliku astme, Azure premium mälu salvestab andmed SSD samas kindlad salvestab andmed HDD-d.) Pange tähele, tuleb teil Azure premium salvestusruumi kontot kasutada premium mälu. Premium mälu kohta lisateabe saamiseks külastage [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](../storage/storage-premium-storage.md).

StorSimple virtuaalse seadme kohta lisateabe saamiseks minge [Deploy ja Azure StorSimple virtuaalne seadme haldamine](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>StorSimple halduri teenuse

Microsoft Azure'i StorSimple sisaldab veebipõhise kasutajaliidese (StorSimple halduri teenus), mis võimaldab teil ühes keskses kohas hallata andmekeskuse ja salvestusruum pilveteenuses. StorSimple halduri teenuse abil saate teha järgmisi toiminguid:

- StorSimple seadmete süsteemi sätete konfigureerimine.
- Konfigureerimine ja haldamine turbesätted StorSimple seadmete jaoks.
- Pilveteenuse identimisteabe ja atribuutide konfigureerimine.
- Konfigureerimine ja haldamine serveris maht.
- Helitugevuse rühmade konfigureerimine.
- Varundamise ja taastamise andmed.
- Jõudluse jälgimist.
- Vaadake üle süsteemisätetest ja võimalike probleemide tuvastamine.

StorSimple halduri teenuse abil saate kõigi administreerimisülesannete täitmine, välja arvatud juhul, kui need, mis nõuavad süsteemi alla ajal, nt Alghäälestus ja värskenduste installimine.

Lisateabe saamiseks lugege [kasutamine StorSimple halduri teenuse haldamiseks StorSimple seadme](storsimple-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShelli StorSimple

Windows PowerShelli StorSimple pakub käsurea liides, mille abil saate luua ja teenuse Microsoft Azure'i StorSimple haldamine ja häälestamine ja StorSimple seadmete jälgimine. See on Windows PowerShelli põhise käsurea liides, sisaldab asjakohast cmdlet-käskude haldamise StorSimple seadme. Windows PowerShelli StorSimple sisaldab funktsioone, mis võimaldavad teil.

- Seadme registreerimine.
- Konfigureerida võrgu liidese seadmes.
- Teatud tüüpi värskenduste installimine.
- Tõrkeotsing seadme tugi seansi kasutades.
- Seadme oleku muutmine

Windows PowerShelli StorSimple järjestikune konsooli (arvutis host ühendatud otse) või juurde kaugühenduse teel remoting Windows PowerShelli abil. Pange tähele, et mõned Windows PowerShelli jaoks StorSimple ülesandeid, nagu algsel seadme registreerimine, saab teha ainult järjestikune konsooli. 

Lisateabe saamiseks minge [Windows PowerShelli kasutamine StorSimple seadme haldamiseks](storsimple-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure'i StorSimple PowerShelli cmdlet-käsud

StorSimple Azure PowerShelli cmdlet-käsud on kogumi Windows PowerShelli cmdlet-käsud, mis võimaldavad teil teenusetaseme ja migreerimise automatiseerida käsurea kaudu. Lisateavet Azure PowerShelli cmdletid StorSimple minge [cmdleti viide](https://msdn.microsoft.com/library/dn920427.aspx).

## <a name="storsimple-snapshot-manager"></a>StorSimple hetktõmmise haldur

StorSimple hetktõmmise Manager on Microsoft Management Console (MMC) lisandmoodul, mille abil saate luua ühtse, punkti /-kellaaja varukoopiaid kohalik ja pilveteenuse andmed. Lisandmoodul käivitatakse Windows Server-põhise host. Saate kasutada StorSimple hetktõmmise Manager.

- Konfigureerimine, varundamine ja mahud kustutamine.
- Helitugevuse rühmi, et tagada konfigureerimine varundatud andmete on rakenduse ühtsete.
- Varukoopia poliitikate haldamine, nii, et andmed on varundatakse lülitame ajakava ja määratud asukohas talletatava (kohapeal või pilves).
- Maht- ja üksikuid faile taastada.

Varukoopiate on jäädvustatud nimega salvestamine ainult muudatused toimunud viimase hetktõmmise ja täielik varukoopiate kui palju salvestusruumi vaja hetktõmmiseid. Saate luua varukoopia ajakava või teha kohe varukoopiate vastavalt vajadusele. Lisaks saate StorSimple hetktõmmise halduri luua säilituspoliitikate juhtelemendi mitu pilte ei salvestata. Kui teil on vaja hiljem andmete taastamine varukoopia, StorSimple hetktõmmise halduri abil saate valida kataloogi kohaliku või pilve pilte. 

Katastroof juhul või kui teil on vaja mõnel muul põhjusel andmete taastamine, StorSimple hetktõmmise halduri taastab see sammhaaval seda on vaja. Andmete taastamine ei nõua sulgeda kogu süsteemi ajal taastada faili asendada seadmete või tegevuste teisaldamine mõnele teisele saidile.

Lisateabe saamiseks minge [mis on StorSimple hetktõmmise haldur?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>SharePointi StorSimple adapterit

Microsoft Azure'i StorSimple sisaldab StorSimple adapterit SharePointi, valikuline osa, mis ulatub läbipaistev StorSimple salvestusruum ja selle andmete kaitse funktsioonid parkides, SharePoint Serveri jaoks. Funktsiooni adapterit töötab Remote bloobimälu salvestusruumi (RBS) pakkuja ja SQL serveri RBS funktsiooni võimaldab plekid liikumiseks server Microsoft Azure'i StorSimple süsteemi toetavad. Microsoft Azure'i StorSimple siis salvestab andmed BLOOBIMÄLU kohalikult või pilves, kasutuse põhjal.

SharePointi StorSimple adapterit hallatakse SharePointi administreerimiskeskuse portaali kaudu. Järelikult SharePointi halduse jääb tsentraliseeritud ja kõik salvestusruumi näib SharePointi serveripargis.

Lisateabe saamiseks minge [SharePointi StorSimple adapterit](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Salvestusruumi haldamine tehnoloogiad
 
Lisaks sihtotstarbeline StorSimple seade, virtuaalse seadme ja muud komponendid, kasutab Microsoft Azure'i StorSimple järgmist tarkvara tehnoloogiad andmete kiire juurdepääsu andmiseks ja salvestusruumi vähendamiseks:

- [Automaatse salvestusruumi tiering](#automatic-storage-tiering) 
- [Õhuke ettevalmistamine](#thin-provisioning) 
- [Korduste eemaldamise ja tihendamine](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automaatse salvestusruumi tiering

Microsoft Azure'i StorSimple automaatselt korraldab andmete loogilise astme praeguse kasutuse, vanus ja seoses muude andmete põhjal. Ajal vähem aktiivne ja passiivne andmete migreeritakse automaatselt pilveteenusesse, talletatakse andmeid, mis on aktiivne. Järgmine diagramm näitab seda moodust salvestusruumi.
 
![StorSimple storage astme](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Kiire juurdepääsu lubamiseks StorSimple salvestab väga aktiivne (andmeid kuum) SSD StorSimple seadmes. See talletab andmeid, mida kasutatakse aeg-ajalt (Pange andmete) kohta HDD-d seadmesse või andmekeskuse servereid. Andmevormis viib passiivsed andmeid, varukoopia ja andmete säilitatakse arhiivimine või täitmise eesmärgil pilveteenusesse. 

>[AZURE.NOTE] Update 2 või uuemat versiooni, saate määrata kohaliku kinnitatud maht, sellisel juhul andmed jääb kohaliku seadmega ja pole mitmetasandilise pilveteenusesse. 

StorSimple reguleerib ja korraldab andmed ümber ning salvestusruumi ülesanded nimega mustreid muuta. Näiteks osa teabest võib muutuda vähem aktiivne aja jooksul. Kuna muutub järk-järgult vähem aktiivne, see on migreeritud SSD HDD-d ja seejärel pilveteenusesse. Kui sama andmeid uuesti aktiivseks, see on migreeritud tagasi mäluseade.

Salvestusruumi tiering protsessi toimub järgmiselt:

1. Süsteemiadministraator häälestab Microsoft Azure'i pilveteenuste salvestusruumi konto.
2. Administraator kasutab järjestikune konsooli ja StorSimple halduri teenuse (töötab Azure klassikaline portaalis) seadmes ja faili server, loomise mahud ja andmete konfigureerimiseks. Kohapealse masinad (nt faili serverid) kasutada StorSimple seadme Internet Small Computer System Interface (iSCSI).
3. Esialgu StorSimple salvestab andmete kiire SSD taseme seade.
4. Kui SSD taseme võimsus, StorSimple deduplicates tihendab vanimast andmete plokid ja teisaldab need HDD taseme.
5. Nimega HDD taseme lähenemisel võimsusega StorSimple krüptib vanimast andmete plokid ja saadab turvaliselt Microsoft Azure storage konto HTTPS-i kaudu.
6. Microsoft Azure'i loob mitme koopiad andmete selle andmekeskuse ja remote andmekeskuses, tagada, et andmeid taastada katastroof juhul. 
7. Kui faili server nõuab pilves talletatud andmed, StorSimple tagastab selle sujuvalt ja salvestab koopia SSD taseme StorSimple seadme.

### <a name="thin-provisioning"></a>Õhuke ettevalmistamine

Õhuke ettevalmistamise on virtualization tehnoloogia saadaolevat näib ületada füüsilised ressursid. Asemel broneerimist piisavalt salvestusruumi eelnevalt, StorSimple kasutab õhuke ettevalmistamise eraldada praeguse nõuetele vastavuse lihtsalt piisavalt ruumi. Salvestusruumi elastne laadi hõlbustab see lähenemine, kuna StorSimple saate suurendada või vähendada salvestusruumi muutuvate vajaduste rahuldamiseks. 

>[AZURE.NOTE] Kohalik kinnitatud maht on pole õhukesed ette valmistatud. Mäluruumi eraldada ainult kohalikku helitugevus on täielikult ette valmistatud, kui maht on loodud.

### <a name="deduplication-and-compression"></a>Korduste eemaldamise ja tihendamine

Microsoft Azure'i StorSimple kasutab korduste eemaldamise ja andmete tihendamise täpsemaks vähenda.

Korduste eemaldamise üldine vähendab kõrvaldamise koondamise salvestatud andmehulgas säilitatavate andmetega. Teave muutub, StorSimple ignoreerib samaks andmed ja salvestab ainult soovitud muudatused. Lisaks StorSimple vähendab salvestatud andmeid ja koolitus Mittevajalik teave. 

>[AZURE.NOTE] Andmete kohalikult kinnitatud draividel on deduplicated ega tihendatud. Siiski varukoopiaid kohalikult kinnitatud maht on deduplicated ja tihendatud.

## <a name="storsimple-workload-summary"></a>StorSimple töökoormus Kokkuvõte

Allpool on esitatud tabelina Kokkuvõte toetatud StorSimple töökoormus.

| Stsenaarium                  | Töökoormus                | Toetatud |  Piirangud                                  | Versioon              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Koostöö             | Failide ühiskasutus            | Jah       |                                                | Kõik versioonid         |
| Koostöö             | Jaotatud failide ühiskasutus| Jah       |                                                | Kõik versioonid         |
| Koostöö             | SharePointi              | Jah *      |Toetatud ainult kohalikult kinnitatud mahud      | Värskenduse 2 ja uuemad versioonid   |
| Arhiivimine                  | Failide arhiivimine   | Jah       |                                                | Kõik versioonid         |
| Office'i Klõpskäivituse            | Virtuaalmasinates        | Jah *      |Toetatud ainult kohalikult kinnitatud mahud      | Värskenduse 2 ja uuemad versioonid   |
| Andmebaasi                  | SQL-I                     | Jah *      |Toetatud ainult kohalikult kinnitatud mahud      | Värskenduse 2 ja uuemad versioonid   |
| Video järelevalve        | Video järelevalve      | Jah *       |Toetatud, kui StorSimple seadme eesmärk on jätkuvalt ainult selle töökoormus| Värskenduse 2 ja uuemad versioonid   |
| Varundus                    | Peamine eesmärk varundamine | Jah *      |Toetatud, kui StorSimple seadme eesmärk on jätkuvalt ainult selle töökoormus| Värskenduse 3 ja uuemad versioonid |
| Varundus                    | Teisene sihtrakenduse varundamine | Jah *      |Toetatud, kui StorSimple seadme eesmärk on jätkuvalt ainult selle töökoormus| Värskenduse 3 ja uuemad versioonid |

*Jah & #42; -Rakendatakse lahendus juhised ja piirangud.*

Järgmine töökoormus ei toeta StorSimple 8000 seeria seadmed. Kui juurutatud StorSimple, tulemuseks nende töökoormus konfiguratsiooni ei toetata.

-  Magnetresonantskuva
-  Exchange'i
-  VDI-LISANDMOODUL
-  Oracle'i
-  SAP-I
-  Suur andmed
-  Sisu jaotuse.
-  Buutimine SCSI kaudu

Järgmine loend toetatud StorSimple taristu komponendid. 

| Stsenaarium | Töökoormus      | Toetatud |  Piirangud                                 | Versioon      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Üldine  | Kiire marsruutimiseks | Jah       |                                               |Kõik versioonid |
| Üldine  | DataCore FC   | Jah *       |Toetatud DataCore SANsymphony            | Kõik versioonid |
| Üldine  | DFSR          | Jah *      |Toetatud ainult kohalikult kinnitatud mahud     | Kõik versioonid |
| Üldine  | Indekseerimine      | Jah *       |Astmeline maht, ainult metaandmete indekseerimine on toetatud (andmed).<br>Kohalik kinnitatud maht, täielik indekseerimine on toetatud.| Kõik versioonid |
| Üldine  | Viirusetõrje    | Jah *       |Astmeline maht, toetatakse ainult skannimine avamisel ja Sule.<br> Kohalik kinnitatud maht, täielik skannimine on toetatud.| Kõik versioonid |

*Jah & #42; -Rakendatakse lahendus juhised ja piirangud.*

## <a name="storsimple-terminology"></a>StorSimple terminoloogia 

Enne oma Microsoft Azure'i StorSimple lahenduse juurutamine soovitame, vaadake üle järgmised tingimused ja määratlused.

### <a name="key-terms-and-definitions"></a>Võtme tingimused ja määratlused

| Termini (lühend või lühend) | Kirjeldus |
| ------------------------------ | ---------------- |
| Accessi juhtelement kirje (ACR)    | Microsoft Azure'i StorSimple seadmes, mis määratleb, mis hosts saab ühendada mahuga seotud kirje. Määramine põhineb iSCSI kvalifitseeritud hosts (sisalduvad iga-aastase), millega loote StorSimple seadme nimi (IQN).|
| AES 256                        | 256-bit Advanced Standard (AES) algoritmi jaoks krüptimist, nagu see viib ja sealt pilve. |
| jaotuse ühiku maht (AUS)     | Väikseim kogus kettaruumi, mida saab hoida faili teie Windowsi failisüsteemi. Kui faili maht ei ole isegi mitu kobar suurus, lisaruumi tuleb kasutada hoida faili (kuni järgmise mitu kobar suurus) tulemuseks kaotsi ruumi ja kõvaketta killustatus. <br>Soovitatav AUS Azure StorSimple maht on 64 KB, kuna see töötab ka korduste eemaldamise algoritmide kohta.|
| automatiseeritud salvestamise tiering      | Automaatselt vähem aktiivne andmete teisaldamise SSD HDD-d ja seejärel soovitud taseme pilves, ja seejärel lubada keskse kasutajaliidese kõik salvestusruumi haldamine.|
| varukoopia kataloogi | Varukoopiate, tavaliselt seotud, rakenduse tüüp, mida kasutati kogum. Selle saidikogumi kuvatakse varukoopia kataloogi lehe StorSimple halduri teenuse kasutajaliides.|
| varukoopia kataloogi fail             | Fail, mis sisaldab loendit saadaval hetktõmmiseid praegu talletatud StorSimple hetktõmmise halduri andmebaasi varundada. |
| varukoopia poliitika                   | Valiku mahu, varunduse tüüp ja ajakava, mis võimaldab teil luua varukoopiaid eelmääratletud ajakava.|
| suured binaarsed objektid (plekid)    | Kogu talletatud ühtse tervikuna andmebaasi halduse süsteemi binaarandmeid. Plekid on tavaliselt pildid, heli või muude MMS objektide, kuigi mõnikord kahendarvu täitmiskood on salvestatud on BLOOBIMÄLU.|
| Ülesanne käepigistus Authentication Protocol (CHAP) | Protokoll omavahelistes ühenduse, põhineb omavahelistes, parool või salajane ühiskasutuse, mida kasutatakse. CHAP võib olla nii ühe- või vastastikune. Ühesuunalise CHAP, mis Sihtvaluuta autendib algataja. Vastastikuste CHAP nõuab, et sihtkohas autentimiseks algataja ja et algataja autendib sihtkohas. | 
| klooni                          | Koopia maht. |
|Pilveteenuse nimega mõne teise astme (CaaT)          | Pilvepõhine salvestusruumi integreeritud mõne teise astme salvestusruumi arhitektuur sees nii, et kõik salvestusruumi näib ühe ettevõtte salvestusruum võrgus.|
| pilveteenuse pakkuja (CSP)   | Cloud services arvutuste pakkuja.|
| pilveteenuse hetktõmmis                 | Punkti õigel ajal koopia helitugevuse pilves talletatud andmetega. Pilveteenuse hetktõmmise võrdub hetktõmmise kopeeritud erinev, väljapoole salvestusruumi süsteem. Pilveteenuse hetktõmmiseid on eriti kasulik Avariijärgne taaste stsenaariumid.|
| pilveteenuse salvestusruumi krüptovõtme   | Parooli või klahvi StorSimple seadme saadetud seadme pilveteenusesse krüptitud andmetele juurdepääsuks kasutada.|
| kobar-arvestada värskendamine         | Tarkvaravärskendusi serverites tõrkesiirdeklastrist haldamise nii, et värskendused on minimaalne või teenuse kättesaadavus ei mõjuta.|
| Andmeteed                       | Kogum funktsionaalseid üksusi, mis on seotud andmete töötlemise toiminguid.|
| desaktiveerimine                     | Püsivate toimingu piirid StorSimple seade ja seotud pilveteenuses vahelise ühenduse. Pilveteenuse hetktõmmiseid seadme pärast selle protsessi ja saab kloonitud või kasutada tõrkejärgseks.|
| ketas peegeldus                 | Loogilise ketta maht on raske eraldi koopia draivid reaalajas pidev kättesaadavuse tagamiseks.|
| dünaamiline ketas peegeldus         | Loogilise ketta maht dünaamiline kettale koopia.|
| dünaamiliste ketast                  | Ketas helitugevuse vormingut, mis kasutab loogilise ketta Manager (LDM) talletamiseks ja andmete haldamine üle mitme füüsilise ketast. Dünaamiliste ketast saate laiendada, et anda rohkem vaba ruumi.|
| Laiendatud kogum ketast (EBOD) ruumi | Teisene ruum Microsoft Azure StorSimple seadme, mis sisaldab lisatasu kõvakettale ketast jaoks täiendav salvestusruum.|
| paks ettevalmistamine               | On tavapärase salvestusruumi ettevalmistamise hoidlas, mis on eraldatud ruumi vastavalt vajadustele oodatud (ja on tavaliselt väljaspool vajadus). Vt ka *õhuke ettevalmistamise*.|
| arvuti kõvaketta (HDD)          | Ketas, mis kasutab kohustused Vaagnad andmete talletamiseks.|
| hübriidjuurutuse salvestusruumi           | Salvestusruumi arhitektuur, mis kasutab kohaliku ja väljapoole ressursse, sh salvestusruumi.|
| Internet System Interface (iSCSI) | Mõne salvestusruumi Internet Protocol (protokoll) IP-põhise võrgu standard andmete salvestusruumi seadmed või vahendid.|
| iSCSI-käiviti                 | Tarkvara komponent, mis võimaldab host arvutis, kus töötab Windows ühenduse välise iSCSI-põhise salvestusruumi võrku.|
| iSCSI kvalifitseeritud nimi (IQN)      | Kordumatu nimi, mis tuvastab iSCSI siht- või algataja.|
| iSCSI Target (sihtkoht)                    | Tarkvara osa, mis annab keskse iSCSI ketta alamsüsteemi salvestusruumi ala võrkudes.|
| reaalajas arhiveerimise                  | Salvestusruumi moodust, kus arhiivimine andmed on kättesaadavad kogu aeg (see pole talletatud mujal läbiviidavat lindile, näiteks). Microsoft Azure'i StorSimple kasutab reaalajas arhiivimine.|
|Kohalik kinnitatud helitugevuse | helitugevuse, mis asub seade ja pole kunagi mitmetasandilise pilveteenusesse. |
| kohaliku hetktõmmis                  | Helitugevuse andmeid, mis on talletatud Microsoft Azure StorSimple seadme punkti õigel ajal koopia.|
| Microsoft Azure'i StorSimple      | Võimas lahendus andmekeskuse salvestusruumi seade ja tarkvara, mis võimaldab asutustel kasutada salvestusruumi, nagu oleks andmekeskuse salvestusruumi. StorSimple lihtsustab andmekaitse ja andmehaldus ajal vähendada. Lahendus koondab esmane salvestusruumi, arhiivimine, varundamine ja Avariijärgne taaste (DR) kaudu pilvega. SAN salvestus- ja pilveteenuse andmehaldus äriklassi platvormil kombineerides lubada StorSimple seadmete kiirus, lihtne ja töökindlus kõigi salvestusruumi seotud vajadustele.|
| Power ja jahutus mooduli (PCM)  | Seadme StorSimple koosneb power tarvikute ja jahutusventilaatori, seega nimi Power ja jahutus mooduli riistvara. Esmane ruumi seade on kaks 764W PCMs EBOD ruum on kaks 580W PCMs.|
| esmane ruumi               | Peamine ruum StorSimple seadme rakenduste platvormi kontrollerid sisaldava.|
| taastamise aeg eesmärk (RTO)   | Pärast katastroof täielikult taastub on maksimaalne aeg, mis peaks tühi enne Äriprotsess või süsteem.| 
|järjestikust manustatud SCSI (SAS)       | Teatud tüüpi kõvaketta (HDD).|
| teenuse andmete krüptovõtme     | Võti uue StorSimple seadmesse, mis registreerib StorSimple halduri teenuse kättesaadavaks teha. Otsingukonfiguratsiooni andmete StorSimple halduri teenuse ja seadme vahel on krüptitud avalik võti ja saab siis dekrüptida ainult abil privaatvõti seadmes. Teenuse andmete krüptovõtme võimaldab teenuse hankimiseks selles privaatvõti dekrüptimine jaoks.|
| teenuse registreerimise võti        | Võti, mis aitab StorSimple seadme registreerimine StorSimple halduri teenuse nii, et see kuvatakse Azure klassikaline portaalis täpsemaks haldamise toimingute jaoks.|
| Väike arvuti süsteemi liidese (SCSI) | Füüsilise ühenduse loomisega ja nende vahel andmete läbides kogum.|
| ühtlase ketas (SSD)         | Ketas, mis sisaldab pole liikuvad osad; näiteks mälupulgale.|
| salvestusruumi konto                 | Accessi mandaadikomplekt seotud konto salvestusruumi teenusepakkuja antud pilve.| 
| SharePointi StorSimple adapterit| Microsoft Azure'i StorSimple osa, mis läbipaistev laiendab StorSimple salvestusruumi ja andmekaitse parkides, SharePoint Serveri.|
| StorSimple halduri teenuse      | Azure'i klassikaline portaali, mis võimaldab teil hallata oma kohapealse Azure StorSimple ja virtuaalne seadmed pikendamine.|
| StorSimple hetktõmmise haldur     | Microsoft Management Console (MMC) lisandmoodulit Microsoft Azure'i StorSimple varundus ja taaste toimingute haldamise.|
| varukoopia tegemine                     | Funktsioon, mis võimaldab kasutajal on interaktiivne maht varukoopia tegemiseks. See on alternatiivne võimalus maht vastandina võttes määratletud poliitika on automaatse varundamise käsitsi varukoopia tegemine.|
| õhuke ettevalmistamine               | Optimeerida, millega saadaolevat salvestusruumi kasutatakse salvestusruumi süsteemid tõhusust meetod. Klõpsake õhuke ettevalmistamise talletamist on jaotada mitme kasutaja põhjal igal ajal iga kasutaja nõutud minimaalse ruumi. Vt ka *rasva ettevalmistamise*.|
| Tiering | Loogika rühmitustega praeguse kasutuse, vanus ja muud andmed seoses andmete korraldamine. StorSimple automaatselt korraldab andmete astme.   |
| helitugevuse                          | Draivid kujul esitatud loogilises salvestusruumi alad. StorSimple mahud vastavad mahud host, kaasa arvatud avastanud iSCSI ja StorSimple seadme ühendada.|
 | helitugevuse container                | Rühmitamis ja neile sätted. Kõik mahud StorSimple seadme rühmitatakse helitugevuse ümbriste. Helitugevuse container sätted sisaldavad salvestusruumi kontod, krüptimise sätete Cloud seotud krüptimise võtmed saadetud andmed ja läbilaskevõime tarbitud suhtes, mis hõlmab pilve.|
| helitugevuse rühma                    | StorSimple hetktõmmise halduri helitugevuse rühm on konfigureeritud varukoopia töötlemise hõlbustamiseks mahud kogum.|
| Draivi vari teenus (VSS)| Windows Serveri operatsioonisüsteemi teenus, mis hõlbustab rakenduse järjepidevuse suheldes VSS-rakendusi koordineerimiseks suureneva hetktõmmiseid luua. VSS tagab rakendused on ajutiselt passiivsed, kui on tehtud pilte.|
| Windows PowerShelli StorSimple | Windows PowerShelli põhise käsurea liidesega StorSimple seadme haldamine ja kasutada. Säilitades osa Windows PowerShelli põhilisi funktsioone, on see kasutajaliidese täiendavad sihtotstarbeline cmdlet-käsud, mis on suunatud StorSimple seadme haldamine.|


## <a name="next-steps"></a>Järgmised sammud

[StorSimple](storsimple-security.md)turvalisusest.




 

 
