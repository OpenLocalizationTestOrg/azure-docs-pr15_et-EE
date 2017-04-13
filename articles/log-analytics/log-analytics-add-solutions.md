<properties
    pageTitle="Lisada Log Analytics lahenduste lahendusegaleriist | Microsoft Azure'i"
    description="Log Analytics lahenduste on kogumi loogika, visualiseerimine ja andmeid tuua reeglite, mis pakuvad mõõdikute kallutatava ümber kindla probleemi lahendamiseks ala."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Lisage Log Analytics lahenduste lahendusegaleriist

Log Analytics lahendused on **loogika**, **visualiseerimine** ja **andmeid tuua reeglid** mõõdikute kallutatava ümber kindla probleemi lahendamiseks ala varustavate kogum. Selles artiklis loendite lahenduste toetatud Log Analytics ja kirjeldatakse, kuidas lisada ja eemaldada lahendusegalerii kaudu.

Lahenduste põhjalikult teadmisi lubamiseks tehke järgmist.

- aitab uurida ja kiiremini funktsionaalseid probleemide lahendamiseks
- koguda ja oleksid erinevat tüüpi masina andmed
- aitavad teil olema aktiivne, nagu võimsuse planeerimine, paikade olek aruandlus- ja turvalisuse audit.


>[AZURE.NOTE] Log Analytics sisaldab Log otsingufunktsioone, seetõttu ei pea te installida lahenduse lubamiseks. Siiski saate avada visualiseeringud, Soovitatavad otsingud ja muude võimalustega, lisades lahenduste lahenduse galeriist.

Kui olete lisanud lahenduse, andmete kogumise serverite oma infrastruktuuri ja OMS teenuse saadetud. Funktsiooni OMS töötlemise teenuse kulub tavaliselt paar minutit kuni tund. Pärast teenuse töötleb andmeid, saate seda vaadata OMS.

Kui see pole enam vaja, saate hõlpsasti lahenduse eemaldada. Kui eemaldate lahenduse, saadetakse selle andmete ei OMS, mis kasutavad iga päev meiliteavitus andmehulga saab vähendada, kui on olemas.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Lahendused ei toeta Microsoft Agenti jälgimine

Sel ajal, saate serverid, mis on ühendatud Microsoft Agenti jälgimine abil OMS enamik lahendusi, mis on saadaval, sh:

- Active Directory hindamine
- Teatiste haldamine (ilma SCOM teatised)
- Ründevaratõrje
- Muutuste jälitus
- Turvalisus
- SQL-i hindamine
- Süsteemi värskendused

Järgmisi lahendusi on siiski *pole* toetatud Microsoft Agenti jälgimine ja nõuavad System Center toimingute Manager (SCOM) agent.

- (Sh SCOM teatised) teatiste haldus
- Võimsuse haldamine
- Konfiguratsiooni hindamine

Vaadake [Ühenduse Toiminguhalduri Log Analytics](log-analytics-om-agents.md) Log Analytics SCOM agent ühenduse loomise kohta lisateavet.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Lahendusegalerii abil lahenduse lisamiseks

1. Klõpsake lehel ülevaade OMS **Lahendusegaleriisse** paani.    
    ![lahendusegalerii](./media/log-analytics-add-solutions/sol-gallery.png)
2. Klõpsake lehel OMS lahendusegaleriisse teavet iga võimalik lahendus. Klõpsake lahendusele, mille soovite lisada OMS nime.
3. Lahendusele, mille valisite lehel kuvatakse lahenduse üksikasjalikku teavet. Klõpsake nuppu **Lisa**.
4. Uue paani lahendusele, mille olete lisanud, kuvatakse lehe OMS ja te saate kasutuselevõtmine pärast OMS teenuse töötleb andmete ülevaade.

## <a name="to-configure-solutions"></a>Lahenduste konfigureerimine
1. Peate mõne lahenduste konfigureerimine. Näiteks peate konfigureerimine automatiseerimine ja Azure saidi taastamine varukoopia, enne kui saate neid kasutada.
2. Mis tahes nende lahenduste, klõpsake selle paani lehel ülevaade.  
    ![lahendus konfigureerimine](./media/log-analytics-add-solutions/configure-additional.png)
3. Seejärel konfigureerimine lahendus vajalikku teavet ja klõpsake siis nuppu **Salvesta**.  
    ![lahendus konfigureerimine](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Lahendusegalerii abil lahenduse eemaldamine

1. Klõpsake lehe Ülevaade OMS, klõpsake paani **sätted** .
2. Klõpsake lehe sätted vahekaardil lahenduste klõpsake käsku **Eemalda** lahenduse, mille soovite eemaldada.
3. Kinnituse dialoogiboksis nuppu **Jah** lahenduse eemaldada.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Andmete kogumine üksikasjad OMS funktsioone ja lahendusi

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas andmeid kogutakse OMS funktsioone ja lahendusi. Otsekoheseks agentide ja SCOM agentide on põhiosas sama, kuid otsekoheseks agent sisaldab lisafunktsioone Lubage see OMS tööruumi loomiseks ja marsruutimine läbi puhverserveri. Kui kasutate SCOM agent, see suunatud OMS agent OMS suhelda. SCOM agentide selles tabelis on OMS agentide, mis on ühendatud SCOM. Leiate teavet [Ühenduse Toiminguhalduri Log Analytics](log-analytics-om-agents.md) OMS SCOM keskkonna olemasoleva ühenduse loomise kohta.

>[AZURE.NOTE] Kasutatav tüüp määrab, kuidas andmeid saadetakse OMS, on järgmised tingimused:

- Kas kasutate otsekoheseks agent või OMS SCOM manustatud agent.
- Kui nõutakse SCOM, SCOM agent lahendus alati saadetakse andmed OMS SCOM halduse rühm, kasutades. Lisaks, kui SCOM on vaja, ainult SCOM agent kasutatakse lahendus.
- Kui SCOM nõutakse ja tabel näitab, et SCOM agent saadetakse andmed OMS haldus jaotises abil, siis SCOM agent andmed alati saadetakse OMS halduse rühmade kasutamine. Otsest agentide mööduda jaotise haldus ja saata oma andmed otse OMS.
- Kui SCOM agent andmeid ei saadeta abil halduse rühma, siis andmed on saadetud otse OMS – mööda jaotise haldus.


|andmetüüp| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|---|
|AD hindamine|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 päeva|
|AD kopeerimise olek|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 päeva|
|Teatiste (Nagios)|Linux|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|saabumisel|
|Teatiste (Zabbix)|Linux|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|15 minutit|
|Teatiste (Toiminguhalduri)|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 minutit|
|Ründevaratõrje|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)| tunni|
|Võimsuse haldamine|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)| tunni|
|Muutuste jälitus|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)| tunni|
|Muutuste jälitus|Linux|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|tunni|
|Konfiguratsiooni Assessment (pärand Advisor)|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)| kaks korda päevas|
|ETW|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minutit.|
|IIS-i logid|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minutit.|
|Võtme võlvid|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuti|
|Võrgus taotluse lüüsid|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuti|
|Võrgu turberühmad|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuti|
|Office 365|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|Klõpsake teatis|
|Jõudluse hinnale|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|ajastatud nimega vähemalt 10 sekundit|
|Jõudluse hinnale|Linux|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|ajastatud nimega vähemalt 10 sekundit|
|Teenuse struktuuri|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minutit.|
|SQL-i hindamine|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 päeva|
|SurfaceHub|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|saabumisel|
|Logi|Linux|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|Azure'i salvestusruumist: 10 minuti; agendi: saabumisel|
|Süsteemi värskendused|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)| vähemalt 2 korda päevas ja 15 minutit pärast värskenduse installimist|
|Windowsi turbe sündmuselogide|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)| Azure Storage: 10 min; agent: saabumisel|
|Windowsi tulemüüri logid|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)| saabumisel|
|Windowsi sündmuste logid|Windows|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)| Azure Storage: 1 min; agent: saabumisel|
|Kaabel andmed|Windowsi (2012 R2 / 8.1 või uuem versioon)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Jah](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)| iga 1 minuti|

## <a name="log-analytics-preview-solutions-and-features"></a>Logige Analytics eelvaade lahenduste ja funktsioonid

Teenuse ja devops tavade järgimine oleme võimalus partneri klientidega arendada funktsioone ja lahendusi.

Privaatne eelvaate anname klientidele juurdepääsu rühma ennetähtaegse rakendamise funktsiooni või lahenduse saada tagasisidet ja täiustamiseks. Selle kiiret rakendamist on minimaalne funktsioone ja funktsionaalseid võimalusi.

Meie eesmärk on proovida asju kiiresti, et leida, mis töötab ja mis ei tööta. Me itereerima selle protsessi kuni privaatne eelvaade klientide tagasiside teatab meile, kas olete valmis avalikku eelvaade.

Avaliku eelvaate, teeme funktsiooni või lahenduse olemas kõik kasutajad rohkem tagasiside saamine ja kinnitage meie skaleerimist ja tõhusust. Selles etapis:

- Eelvaade funktsioonid kuvatakse vahekaardil sätted ja kasutaja saab lubada
- Eelvaate lahenduste saab lisada Galerii kaudu või avaldatud skripti abil

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Mida peaksin teadma eelvaade funktsioone ja lahendusi kohta?

Oleme oodatud on uued funktsioonid ja lahenduste ja me meeldib saate arendada neid koos.

Eelvaade funktsioone ja lahendusi pole kõigi jaoks õige küll, et enne liituda privaatse eelvaade või lubada avaliku eelvaate veenduge, et töötate OK midagi, mida alles arendatakse.

Kui eelvaade funktsiooni teid portaali kaudu kuvatakse hoiatus, mis tuletab teile meelde, et see funktsioon on eelvaade.

#### <a name="for-both-private-and-public-preview"></a>*Era* - ja *avaliku* eelvaade

Avalike ja privaatvõ eelvaated kehtib järgmine:

- Asjad, mida ei pruugi alati õigesti töötada.
  - Probleemid vahemikus on mõnevõrra pahameelt kaudu, et midagi ei tööta üldse
- On võimalik mõjutab operatsioonisüsteemides oma eelvaade / keskkonnas
  - Teeme, et vältida negatiivne asju, mis juhtub süsteemidele kasutate OMS, kuid mõnikord ootamatuid asju esineda
- Andmete kaotsimineku / võivad rikutud
- Võib-olla palume teil diagnostikalogid või muude probleemide tõrkeotsinguks andmete kogumine
- Funktsioon või lahenduse võib eemaldada (ajutiselt või jäädavalt)
  - Meie õppetunnid põhjal oleme otsustada vabasta funktsiooni või lahenduse eelvaates
- Eelvaated ei pruugi või võib-olla ei on testitud kõik konfiguratsioone ja me võib piirata.
  - Operatsioonisüsteemid, mida saab kasutada (nt funktsioon võib rakenduvad ainult Linux preview's)
  - Agent (MMA, SCOM), mida saab kasutada tüüp (nt funktsioon ei pruugi töötada SCOM preview's)  
- Teenuse taseme leping ei hõlma eelvaade lahenduste ja funktsioonid
- Eelvaate funktsioonide kasutamist, kehtib kasutamise eest
- Funktsioonide ja võimaluste kohta, et peate funktsiooni / lahendus on kasulik võib olla puudu või lõpetamata
- Funktsioonid / lahendused ei pruugi olla saadaval kõikides piirkondades
- Funktsioonid / lahendused ei pruugi olla lokaliseeritud
- Funktsioone ja lahendusi võib olla piiratud arvu kliendid või seadmed, saate seda kasutada
- Võib-olla peate kasutama skriptide konfigureerimise ja lahenduse/funktsiooni lubamine
- Kasutajaliidese (UI) on lõpetamata ja võib muutuda päev
- Avaliku eelvaated ei pruugi vastav oma tootmise / kriitilised süsteemid

#### <a name="for-private-preview"></a>*Privaatne* eelvaade

Lisaks ülaltoodud, on teatud privaatne eelvaated.

- Meil teile pakkuda meile tagasisidet teie töötamist nii, et saame funktsiooni/lahendus paremini oodata
- Me teiega tagasiside küsitlused, telefonikõned või e-posti kasutamine
- Asjad, mida alati ei tööta õigesti
- Me võib olla nõutav varjamine lepingu (NDA) osalemiseks või võivad sisaldada konfidentsiaalse sisu
  - Enne ajaveebindus, tweeting või muul viisil suhtlemine muude tootjate kontrollige selle programmi halduriga eelvaate eest vastutav mõista piirangud avaldamine
- Ei tööta esitamisel / kriitilised süsteemid


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Kuidas saan juurdepääsu privaatne eelvaade funktsioone ja lahendusi?

Kutsume kliendid privaatne eelvaadete mitmel erineval viisil olenevalt eelvaate kaudu.

- Igakuine kliendi küsitlusele vastamine ja annab meile õigus jälgida teiega parandab tõenäosust kutsutud privaatne eelvaade.
- Oma Microsofti konto meeskonna nimetada saate.
- Soovi korral saate põhjal postitatud Twitteri [msopsmgmt](https://twitter.com/msopsmgmt) üksikasjad
- Soovi korral saate andmed jagatud ühenduse sündmustest – vaadake meile koosolekud ups, konverentsid ja online kogukondades.


## <a name="next-steps"></a>Järgmised sammud

- [Otsingu logid](log-analytics-log-searches.md) üksikasjaliku teabe kogutud lahendusi.
