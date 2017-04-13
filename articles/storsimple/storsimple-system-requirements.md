<properties
   pageTitle="StorSimple süsteeminõuded | Microsoft Azure'i"
   description="Kirjeldatakse tarkvara, võrgunduse, ja kõrge-saadavus nõuded ja head tavad: Microsoft Azure StorSimple lahenduse."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>StorSimple tarkvara, kõrge-saadavus ja võrgu nõuded

## <a name="overview"></a>Ülevaade

Tere tulemast Microsoft Azure'i StorSimple. Selles artiklis kirjeldatakse olulisi süsteeminõuded ja seadme StorSimple ja juurdepääsu seadme salvestusruumi klientidele parimaid tavasid. Soovitame teil üle vaadata teavet hoolikalt enne juurutada oma StorSimple süsteem ja seejärel viidata tagasi vastavalt vajadusele ajal kasutuselevõtu- ja järgmise.

Süsteeminõuded on järgmised.

- **Tarkvaranõuded salvestusruumi kliendid** - kirjeldatakse toetatud opsüsteemid ja nende opsüsteemide lisanõuded.
- **Nõuded StorSimple seadme networking** - teave tuleb tulemüüri lubama iSCSI, cloud või halduse liikluse avatud pordid.
- **Kõrge-saadavus nõuded StorSimple** - kirjeldatakse kõrge-saadavus nõuded ja head tavad StorSimple seadme ja hosti arvuti. 


## <a name="software-requirements-for-storage-clients"></a>Tarkvaranõuded salvestusruumi kliendid

Järgmised tarkvaranõuded on juurdepääs StorSimple seadme salvestusruumi klientides.

| Toetatud opsüsteemid | Nõutav versioon | Täiendavad nõuded ja märkmed |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012 2012R2 |StorSimple iSCSI maht on toetatud ainult järgmiste Windows ketta puhul kasutada:<ul><li>Lihtne helitugevuse lihtsa kettal</li><li>Lihtne ja peegelpilt helitugevuse dünaamiline kettal</li></ul>Windows Server 2012 õhuke ettevalmistamise ja ODX funktsioonid on toetatud, kui kasutate StorSimple iSCSI helitugevust.<br><br>StorSimple saate luua õhukesed ettevalmistatud ja täielikult ettevalmistatud maht. See ei saa luua osaliselt ettevalmistatud draividel.<br><br>Ümbervormindamist õhukesed ettevalmistatud helitugevuse võib võtta kaua aega. Soovitame helitugevuse kustutamine ja seejärel luua uue eksemplari asemel ümbervormindamist. Kui näete endiselt eelistab siiski vormindada maht:<ul><li>Käivitage järgmine käsk enne selle vormindada vältimiseks ruumi taastamise viivitused: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Kui vorming on lõpule jõudnud, kasutada uuesti lubamiseks ruumi taastamise järgmine käsk:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Rakendage Windows Server 2012 [KB 2878635](https://support.microsoft.com/kb/2870270) kirjeldatud Windows Server arvutiga.</li></ul></li></ul></ul> Kui olete konfigureerinud SharePointi StorSimple hetktõmmise juhataja või StorSimple adapterit, avage [tarkvaranõuded valikuline komponendid](#software-requirements-for-optional-components).|
| VMWare ESX | 5,5 ja 6.0 | Toetatud VMWare vSphere iSCSI klient. Funktsioon VAAI-plokk on toetatud VMware vSphere StorSimple seadmetes.
| Linux RHEL/CentOS | 5, 6 ja 7 | Tugi Linuxi iSCSI kliendid Ava-iSCSI algataja versioonide 5, 6 ja 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] Praegu ei toetata IBM AIX StorSimple.

## <a name="software-requirements-for-optional-components"></a>Valikuline komponendid tarkvaranõuded

Järgmised tarkvaranõuded on valikuline StorSimple komponendid (StorSimple hetktõmmise juhataja ja StorSimple adapterit SharePointi).

| Komponent | Host platvorm | Täiendavad nõuded ja märkmed |
| --------------------------- | ---------------- | ------------- |
| StorSimple hetktõmmise haldur | Windows Server 2008R2 SP1, 2012 2012R2 | Kasuta StorSimple hetktõmmise Manager Windows Server on vaja varundamine ja taastamine peegelpilt dünaamiline ketast, ja mis tahes rakenduse ühtsete varukoopiad.<br> StorSimple hetktõmmise halduri toetatakse ainult Windows Server 2008 R2 SP1 (64-bitine), Windows 2012 R2 ja Windows Server 2012.<ul><li>Kui kasutate aken Server 2012, tuleb teil installida .NET 3.5 – 4.5 enne installimist StorSimple hetktõmmise haldur.</li><li>Kui kasutate Windows Server 2008 R2 SP1, tuleb teil installida Windows Management Framework 3.0 enne installimist StorSimple hetktõmmise haldur.</li></ul> |
| SharePointi StorSimple adapterit | Windows Server 2008R2 SP1, 2012 2012R2 |<ul><li>SharePointi StorSimple adapterit on toetatud ainult SharePoint 2010 ja SharePoint 2013.</li><li>RBS nõuab SQL Server Enterprise'i versiooni 2008 R2 või 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Võrgunduse StorSimple seadme nõuded

Seadme StorSimple on lukustatud. Pordid tuleb avada tulemüüri iSCSI, pilveteenuste ja haldus liikluse lubamiseks. Järgmises tabelis on loetletud tuleb avada tulemüüri portide. Selles tabelis *sisse* - või *sissetuleva meili* viitab, millest kliendi sissetulevad taotlused juurdepääs seadme suunas. *Välja* või *väljaminevaid* viitab StorSimple seadme saadab andmete väliselt, lisaks juurutamise suunas: näiteks Väljamineva meili Interneti-ühendus.

| Pordi nr<sup>1,2</sup> | Sisse või välja | Pordi ulatus | Nõutav | Märkmete |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Välja |  WAN | Ei |<ul><li>Väljamineva pordi kasutatakse värskenduste toomiseks Interneti-ühendus.</li><li>Väljamineva veebiteenuse puhverserveri on kasutaja konfigureeritav.</li><li>Süsteemi värskenduste lubamiseks selle pordi peab avatakse ka selle domeenikontrolleri fikseeritud IP-d.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Välja | WAN | Jah |<ul><li>Väljamineva pordi kasutatakse andmete pilveteenuses.</li><li>Väljamineva veebiteenuse puhverserveri on kasutaja konfigureeritav.</li><li>Süsteemi värskenduste lubamiseks selle pordi peab avatakse ka selle domeenikontrolleri fikseeritud IP-d.</li><li>Selle pordi kasutatakse ka mõlema funktsiooni domeenikontrollerid Prügikoristus.</li></ul>|
|UDP 53 (DNS) | Välja | WAN | Mõnel juhul; Märkmete kuvamine |Selle pordi on nõutav ainult juhul, kui teil on Interneti-põhise DNS-serveri kasutamine. |
| UDP 123 (NTP) | Välja | WAN | Mõnel juhul; Märkmete kuvamine |Selle pordi on vaja ainult siis, kui kasutate Interneti-põhiste NTP-serverist. |
| TCP 9354 | Välja | WAN | Jah |Väljamineva pordi kasutatakse StorSimple seadme StorSimple halduri teenuse suhelda. |
| 3260 (iSCSI) | Rakenduses | KOHTVÕRGU | Ei | Selle pordi kasutatakse pääsevad andmetele juurde iSCSI.|
| 5985 | Rakenduses | KOHTVÕRGU | Ei | Sissetulev port kasutatakse StorSimple hetktõmmise halduri StorSimple seadmega suhtlemiseks.<br>Selle pordi kasutatakse ka kaugühenduse teel ühenduse loomisel Windows PowerShelli StorSimple http kaudu. |
| 5986 | Rakenduses | KOHTVÕRGU | Ei | Selle pordi kasutatakse kaugühenduse teel ühenduse loomisel Windows PowerShelli StorSimple https. |

<sup>1</sup> sissetuleva pordid pole vaja avada avaliku Interneti.

<sup>2</sup> kui mitu pordid lüüsi konfigureerimine, suunatud väljaminev liiklus tellimuse määratakse pordi marsruutimise sortimisjärjestus [Port marsruutimine](#routing-metric), kirjeldatud allpool.

<sup>3</sup> selle domeenikontrolleri fikseeritud IP-d StorSimple seadmes tuleb marsruuditavaid ja võimalus Interneti-ühenduse. Fikseeritud IP-aadressid, mida kasutatakse teenindamine seadmele värskendusi. Kui seadme kontrollerid ei saa ühendust Interneti kaudu fikseeritud IP-d, ei saa värskendada StorSimple seadme.

> [AZURE.IMPORTANT] Tagama, et tulemüüri muutmine või dekrüptida mis tahes SSL-i liikluse StorSimple seadme ja Azure vahel.

### <a name="url-patterns-for-firewall-rules"></a>URL-i mustrid tulemüüri reeglid

Võrgu administraatorid saavad sageli konfigureerida täpsemad tulemüüri reeglite alusel filtreerimiseks sissetulev URL-i mustrite ja väljaminev liiklus. Seadme StorSimple ja StorSimple halduri teenuse sõltuvad muude Microsoft rakendusi nagu Azure'i teenus siini, Azure Active Directory juurdepääsu reguleerimine, salvestusruumi kontod ja Microsoft Update'i serverites. Nende rakenduste seostatud URL-i mustreid saab konfigureerida tulemüüri reeglid. See on oluline mõista, et need rakendused seostatud URL-i mustreid saate muuta. See omakorda nõuab võrguadministraatori poole, et jälgida ja värskendage oma StorSimple nimega ja vajadusel tulemüüri reeglid.

Soovitame teil seada tulemüüri reeglite väljamineva liikluse, põhineb StorSimple fikseeritud IP-aadressi ohtralt enamikel juhtudel. Siiski saate seada täpsemad tulemüüri reeglid, mida on vaja luua turvalist keskkonnas allolevat teavet.

> [AZURE.NOTE] Kasutatav seade (allikas) IP-d alati olema seatud lubatud võrgu liidesed. Sihtkoha IP-d peaks olema seatud [Azure andmekeskuse IP-vahemikke](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).

#### <a name="url-patterns-for-azure-portal"></a>URL-i mustrite Azure'i portaal
| URL-i mustri                                                      | Komponent/funktsioonid                                           | Seadme IP-d                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple halduri teenuse<br>Teenuse juurdepääsu juhtimine<br>Azure'i teenus siini| Pilveteenuse lubatud võrgu liidesed        |
|`https://*.backup.windowsazure.com`|Seadme registreerimine| Ainult andmed 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Serdi tühistamise |Pilveteenuse lubatud võrgu liidesed |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure'i salvestusruumi kontod ja jälgimine | Pilveteenuse lubatud võrgu liidesed        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update serverid<br>                             | Selle domeenikontrolleri fikseeritud IP-d ainult               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN-ID |Selle domeenikontrolleri fikseeritud IP-d ainult   |
| `https://*.partners.extranet.microsoft.com/*`                    | Tugiteenuste pakett                                                  | Pilveteenuse lubatud võrgu liidesed        |

#### <a name="url-patterns-for-azure-government-portal"></a>URL-i mustrid Azure'i valitsuse portaal
| URL-i mustri                                                      | Komponent/funktsioonid                                           | Seadme IP-d                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | StorSimple halduri teenuse<br>Teenuse juurdepääsu juhtimine<br>Azure'i teenus siini| Pilveteenuse lubatud võrgu liidesed        |
|`https://*.backup.windowsazure.us`|Seadme registreerimine| Ainult andmed 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Serdi tühistamise |Pilveteenuse lubatud võrgu liidesed |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure'i salvestusruumi kontod ja jälgimine | Pilveteenuse lubatud võrgu liidesed        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update serverid<br>                             | Selle domeenikontrolleri fikseeritud IP-d ainult               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN-ID |Selle domeenikontrolleri fikseeritud IP-d ainult   |
| `https://*.partners.extranet.microsoft.com/*`                    | Tugiteenuste pakett                                                  | Pilveteenuse lubatud võrgu liidesed        |

### <a name="routing-metric"></a>Marsruutimise meetermõõdustik

Marsruutimise mõõdiku on seostatud liidesed ja lüüsi, mis marsruutida määratud võrgu andmed. Marsruutimise meetermõõdustik kasutatakse marsruutimise protokoll arvutamiseks parim tee antud sihtkohta, mitme tee on olemas sihtkohaga suurust. Väiksem marsruutimise meetermõõdustik, seda suurem eelistused.

StorSimple kontekstis, kui mitme võrgu liidesed ja lüüside on konfigureeritud kanali liikluse, marsruutimise mõõdikute rakendatakse määratlemiseks suhteline järjestuses, milles saada liidesed kasutada. Kasutaja ei saa muuta marsruutimise mõõdikute. Saate siiski kasutada funktsiooni `Get-HcsRoutingTable` cmdlet-käsu StorSimple seadme marsruutimise tabelist (ja mõõdikute) välja printida. Lisateavet [Tõrkeotsingu StorSimple](storsimple-troubleshoot-deployment.md)juurutuse Get-HcsRoutingTable cmdlet-käsk.

Marsruutimise argumendil algoritmide erinevad sõltuvalt seadme StorSimple töötavate tarkvara versiooni.

**Väljalasete enne Update 1**

See hõlmab tarkvara versioonides Update 1-GA, 0,1, nt 0,2 või 0,3 väljaanne. Tellimuse marsruutimise mõõdikute põhjal on järgmine:

   *Viimase konfigureeritud 10 New York võrgu liidese > muu 10 New York võrgu liidese > viimase konfigureeritud 1 New York võrgu liidese > muu 1 New York võrgu liidese*


**Väljalasete Update 1 ja Update 2 enne alustamist**

See hõlmab näiteks 1, 1.1 või 1.2 Tarkvara versioonid. Tellimuse marsruutimise mõõdikute põhjal on otsustanud järgmiselt:

   *ANDMETE 0 > viimase konfigureeritud 10 New York võrgu liidese > muu 10 New York võrgu liidese > viimase konfigureeritud 1 New York võrgu liidese > muu 1 New York võrgu liidese*

   Update 1, marsruutimise mõõdiku andmete 0 on muudetud madalam; Seetõttu kõik pilve-liikluse marsruuditakse läbi 0. Märkige üles see, kui StorSimple seadmes on rohkem kui üks cloud lubatud võrgu liidese.


**Väljalasete alates Update 2**

Värskenduse 2 on mitmeid võrgunduse seotud parandusi ja marsruutimise mõõdikute on muutunud. Käitumise selgitab järgmiselt.

- Võrgu liidesed on määratud lülitame väärtuste kogumist.   

- Kaaluge näiteks tabel määratud erinevate võrgu liidesed kui need on pilv lubatud väärtused või pilveteenuses – keelatud, kuid konfigureeritud lüüsi allpool näidatud. Pange tähele, väärtused, mis on määratud siin on näide väärtused ainult.


  	| Võrgu liidese | Pilveteenuse lubatud | Pilveteenuse-keelatud Gateway |
  	|-----|---------------|---------------------------|
  	| Andmete 0  | 1            | -                        |
  	| Andmete 1  | 2            | 20                       |
  	| Andmete 2  | 3            | 30                       |
  	| Andmete 3  | 4            | 40                       |
  	| Andmete 4  | 5            | 50                       |
  	| Andmete 5  | 6            | 60                       |


- Järjestuses, milles suunatakse cloud liiklust läbi võrgu liidesed on:

    *Andmete 0 > andmete 1 > kuupäev 2 > andmete 3 > 4 andmed > andmete 5*

    Seda saab selgitada järgmises näites.

    Kaaluge StorSimple seade on kaks cloud lubatud võrgu liidesed, andmete 0 ja andmete 5. Andmete 1 – 4 andmed on pilv-keelatud, kuid on konfigureeritud lüüsi. Järjestus, kus liiklus suunatakse kasutatav seade on järgmised:

    *Andmete 0 (1) > andmete 5 (6) > andmete 1 (20) > andmete 2 (30) > andmete 3 (40) > andmete 4 (50)*

    *Kui arvud on sulgudes näitab vastav marsruutimise mõõdikute.*

    Andmete 0 nurjumisel cloud liikluse saada vahendusel andmete 5. Lüüs on konfigureeritud kõik võrgus, kui nurjuda nii andmete 0 ja andmete 5, et pilveteenuses liikluse läbida andmed 1.


- Pilveteenuse lubatud võrgu liidese nurjumisel, siis on 3 korduskatsed ühenduse liides 30 teise viivitusega. Kui kõik korduskatsed ei õnnestu, suunatakse järgmise saadaval cloud lubatud kasutajaliideses määratud nii liiklust. Kui kõik cloud lubatud võrgu liidesed fail, nurjub seadme üle muude kontrolleril (sel juhul pole taaskäivitamist).

- Kui VIP tõrke jaoks liidest võrgu iSCSI lubatud, saab 3 korduskatsed viivitusega kaks sekundit all. Selline käitumine on jäänud samaks varasemate versioonidega. Kui kõik iSCSI võrgu liidesed ei õnnestu, siis selle domeenikontrolleri Tõrkesiirde toimub (koos taaskäivitamist).


- Teatise tõstetakse ka StorSimple seadme VIP tõrke korral. Lisateabe saamiseks minge [teatiste kiirülevaade](storsimple-manage-alerts.md).

- Korduskatsed, osas iSCSI on ülimuslik pilve.

    Näide: A StorSimple seade on lubatud kaks võrgu liidesed, andmete 0 ja andmed 1. Andmete 0 on cloud lubatud andmed 1 on nii pilve ja iSCSI lubatud. Pole muud võrgu liidesed selle seadme on lubatud pilve või iSCSI.

    Viimase iSCSI võrgu liidese korral andmed 1 nurjub, andnud selle tulemuseks kontrolleril Tõrkesiirde andmete 1 muude kontrolleril.


### <a name="networking-best-practices"></a>Võrgunduse head tavad

Lisaks ülaltoodud võrgu nõuetele optimaalse jõudluse StorSimple lahenduse, tuleb järgida järgmised head tavad:

- Veenduge, et seadme StorSimple on spetsiaalne 40 Mbps läbilaskevõime (või rohkem) saadaval igal ajal. Ei saa ühiskasutusse anda selle läbilaskevõime (või QoS poliitikate abil tagada eraldatud) muude rakendustega.

- Veenduge, et võrguühendus Interneti-ühendus on alati saadaval. Juhuslik või usaldusväärne Interneti-ühendused ja seadmed, sh pole Interneti-ühendust üldse, tulemuseks konfiguratsiooni ei toetata.

- Teostavad iSCSI ja pilveteenuse liikluse, on spetsiaalne võrgu liidesed iSCSI ja pilvepõhise juurdepääsu oma seadmes. Lisateavet leiate teemast [muuta võrgu liidesed](storsimple-modify-device-config.md#modify-network-interfaces) StorSimple seadmes.

- Kasutage oma võrgu liidesed Link koondamine Control Protocol (LACP) konfigureerimine. Selle konfiguratsiooni ei toetata.


## <a name="high-availability-requirements-for-storsimple"></a>Kõrge-saadavus StorSimple nõuded

Riistvara platvorm, mis on kaasatud StorSimple lahendus on kättesaadavus ja usaldusväärsus funktsioone, mis annavad aluse väga kättesaadav, tõrketaluvusega salvestusruumi taristu oma andmekeskuses. Kuid on nõuded ja head tavad, mis peavad vastama tagada teie StorSimple lahendus olemasolu. Enne juurutamist StorSimple, hoolikalt üle järgmised nõuded ja head tavad host ühendatud arvutites ja StorSimple seadme.

Jälgimine ja haldamine StorSimple seadme riistvara kohta lisateabe saamiseks minge [kasutamine StorSimple halduri teenuse jälgida riistvara ja olek](storsimple-monitor-hardware-status.md) ja [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Kõrge-saadavus nõuded ja protseduurid StorSimple seadme

Vaadake hoolikalt, et tagada kõrge kättesaadavus StorSimple seadme järgmine teave.

#### <a name="pcms"></a>PCMs

StorSimple seadmed on järgmised liigsed, kuum kiirvahetuse power ja jahutus moodulid (PCMs). Iga PCM on piisavalt teenuse osutamiseks kogu raami. Kõrge kättesaadavuse tagamiseks peab olema installitud nii PCMs.

- Oma PCMs ühenduse eri power allikad esitada kättesaadavus power allika nurjumisel.
- Kui soovitud PCM nurjub, taotluse asendamist kohe.
- Eemaldage nurjunud PCM ainult siis, kui teil on asendamine ja selle installida.
- Mõlemad PCMs samaaegselt eemaldada. PCM moodul sisaldab varuaku mooduli. Mõlema funktsiooni PCMs eemaldamise tulemuseks suletakse ilma aku kaitse ja seadme olek ei salvestata. Asuvat kohta lisateabe saamiseks minge [Säilita varuaku mooduli](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Selle domeenikontrolleri moodulid

StorSimple seadmed on järgmised liigsed, kuum kiirvahetuse kontrolleril moodulid. Selle domeenikontrolleri moodulid töötama aktiivne/passiivne viisil. Mis tahes ajal, üks kontrolleril moodul on aktiivne ja osutab teenust, samal ajal teiste kontrolleril mooduli on passiivne. Passiivne kontrolleril moodul on sisse lülitatud ja tööle hakkab, kui aktiivne kontrolleril mooduli nurjus või on eemaldatud. Iga on piisavalt võimsus teenuse osutamiseks kogu raami. Kõrge kättesaadavuse tagamiseks peab olema installitud nii kontrolleril moodulid.

- Veenduge, et igal ajal on installitud nii kontrolleril moodulid.

- Selle domeenikontrolleri mooduli nurjumisel taotluse asendamist kohe.

- Eemaldage nurjunud kontrolleril mooduli ainult siis, kui teil on asendamine ja selle installida. Eemaldamine mooduli jaoks pikemaks mõjutab õhuvoolu ja seega jahutamiseks süsteem.

- Veenduge, et mõlema kontrolleril mooduli võrguühendus on täpselt ühesugused, ja ühendatud võrgu liidesed on on identne võrgukonfiguratsioon.

- Kui selle domeenikontrolleri mooduli nurjub või vajab väljavahetamine, veenduge, et muude kontrolleril mooduli on aktiivne olekus enne asendades nurjunud kontrolleril mooduli. Veenduge, et mõne selle domeenikontrolleri on aktiivne, minge [tuvastamine aktiivne kontrolleril seadmes](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Eemaldage mõlemad kontrolleril moodulid samal ajal. Kui selle domeenikontrolleri Tõrkesiirde on pooleli, valige selle domeenikontrolleri mooduli sulgeda või eemaldamine raami.

- Pärast selle domeenikontrolleri Tõrkesiirde, oodake vähemalt viis minutit enne eemaldamist kas kontrolleril mooduli.

#### <a name="network-interfaces"></a>Võrgu liidesed

StorSimple seadme kontrolleril moodulid iga on neli 1 Gigabit ja kaks 10 Gigabitine Ethernet võrgu liidesed.

- Veenduge, et mõlema kontrolleril mooduli võrguühendus on täpselt ühesugused, ja võrgu liidesed kontrolleril mooduli liideste ühendatakse on on identne võrgukonfiguratsioon.

- Võimaluse korral Juurutage võrguühenduste erinevad lülitid võrgu seadme tõrke korral teenuse kättesaadavuse tagamiseks.

- Kui ühendatav ainult või viimase ülejäänud iSCSI lubatud kasutajaliidese (koos määratud IP-d), keelata kasutajaliideses esmalt ja seejärel eemaldage kaabel on. Kui esmalt kasutajaliideses on ühendatud, siis põhjustavad aktiivne kontrolleril nurjumise üle passiivne kontrolleril. Kui passiivne kontrolleril on ka selle vastavate liideste lahti ühendada, siis nii kontrolörid on taaskäivitage mitu korda enne lahendada ühe kontrolleril.

- Ühenduse vähemalt kaks andmete liideste võrgu iga domeenikontrolleri moodulist.

- Kui teil on lubatud kaks 10 New York liideste juurutamine need erinevad lülitid üle.

- Kasutage võimalusel MPIO serverites tagamaks, et serverid saate luba link, võrgu või kasutajaliidese tõrge.

Võrgunduse kohta lisateabe seadme kõrge-saadavus ja jõudluse, avage [seadme StorSimple 8100 installimine](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) või [seadme StorSimple 8600 installida](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD ja HDD-d

StorSimple seadmed on järgmised ühtlase olekus ketast (SSD) ja kõvakettal kettadraivide (HDD-d), mis on kaitstud, kasutades peegeldatud tühikuid. Peegeldatud tühikute kasutamine tagab, et seade on võimalus luba ühe või mitme SSD või HDD-d.

- Veenduge, et kõik SSD ja HDD moodulid on installitud.

- Kui soovitud SSD või HDD nurjub, taotluse asendamist kohe.

- Kui soovitud SSD või HDD ei või asendamine nõuab, veenduge, et eemaldada ainult SSD või HDD, mis nõuab asendamine.

- Eemalda rohkem kui üks SSD ega HDD süsteemist igal ajal.
2 või enam ketast teatud tüüpi (HDD, SSD) ei tööta või lühikese aja jooksul järjestikust tõrge võib kaasa tuua süsteemi rikkeid ja võimalikud andmekao. Sellisel juhul abi saamiseks [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) .

- Jooksul väljavahetamine, jälgida **Riistvara oleku** **haldamise** lehe draivid SSD ja HDD-d. Roheline märge olek näitab, et selle ketast terve või OK on punane hüüumärk punktis näitab nurjunud SSD või HDD.

- Soovitame konfigureerida cloud hetktõmmiseid kõik mahus, mis teil vaja kaitsta süsteemi tõrke korral.

#### <a name="ebod-enclosure"></a>EBOD ruumi

StorSimple seadme mudelist 8600 hõlmab lisaks esmane ruumi ruumi laiendatud kogum, ketast (EBOD). Mõni EBOD sisaldab EBOD kontrollerid ja kõvakettal kettadraivide (HDD-d), mis on kaitstud, kasutades peegeldatud tühikuid. Peegeldatud tühikute kasutamine tagab, et seade on võimalus luba rikkumine ühe või mitme HDD-d. Esmane ruumi kaudu liigsete SAS kaabel on ühendatud EBOD ruumi.

- Veenduge, et mõlema EBOD ruumi kontrolleril moodulid nii SAS kaabel ja kõik kõvakettal kettadraivide on arvutisse installitud.

- EBOD ruum kontrolleril mooduli nurjumisel taotluse asendamist kohe.

- EBOD ruum kontrolleril mooduli nurjumisel veenduge, et muud kontrolleril mooduli on aktiivne enne asendate nurjunud mooduli. Veenduge, et mõne selle domeenikontrolleri on aktiivne, minge [tuvastamine aktiivne kontrolleril seadmes](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- EBOD kontrolleril mooduli asendamine ajal pidevalt jälgida komponendi StorSimple halduri teenuse olekut **hooldustööd**kasutades > **riistvara olek**.

- Kui on SAS kaabel ei või nõuab asendamine (Microsoft Support tuleb kaasata selline kindlaksmääramine.), veenduge, et eemaldada ainult SAS kaabel, mis nõuab asendamine.

- Samaaegselt eemaldage nii SAS kaabel süsteemist igal ajal.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Kõrge-saadavus arvutite host soovitused

Hoolikalt üle hosts StorSimple seadmega ühendatud kõrge kättesaadavuse tagamiseks järgmistest headest tavadest.

- [Kaks sõlme faili server kobar konfiguratsioone]ja StorSimple konfigureerimine[1]. Ühe punktide tõrge ja hoone koondamise host küljel eemaldades kogu lahendus muutub tugevalt saadaval.

- Selle hulka kuuluvad salvestusruumi Tõrkesiirde ajal jagab saadaval koos Windows Server 2012 (SMB 3.0) kõrge-saadavus kasutamine pidevalt saadaval (CA). Lisateavet konfigureerimise faili serveri kogumite ja pidevalt saadaval aktsiad Windows Server 2012, vaadake selle [video demo](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Järgmised sammud

- [Lugege StorSimple süsteemi limiitide kohta](storsimple-limits.md).
- [Saate teada, kuidas oma StorSimple lahenduse juurutamine](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
