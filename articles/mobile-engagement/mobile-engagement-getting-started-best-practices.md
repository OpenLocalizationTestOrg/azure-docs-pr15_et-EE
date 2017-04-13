<properties 
    pageTitle="Azure'i mobiilsideseadmete kaasamine alustamine juhend head tavad"
    description="Azure'i Mobile kaasamine ja head tavad aitavad alustamine juhend hankimine" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure'i mobiilsideseadmete kaasamine - saada juhend head tavad

## <a name="overview"></a>Ülevaade

**Mobiilne ekraan on väga täis ruumi:** 2013 uuring leitud Keskmine mobiilsideseadme oli 27 installitud rakendused. Kasutajate kulunud tavaliselt 30 tundi kuus oma rakendused. Enamik seekord kulus social networking ja mängimine (umbes 20 tundi). Android market oli 2014 1,5 miljonit rakenduste kasutajate seast. Apple'i poest sisaldas ligikaudu 1,2 miljonit rakendused. Endiselt suureneb mobiilirakenduse kasutamine, nagu arendajate võistlema see kasvab market. 

Mobiilse tavakasutaja on installida ja desinstallida rakendused kõrge sagedusega sõltuvalt muutmise huvides ja -rakenduse kogemusi. Selleks, et ka edu rakenduse muutub oluline on teada oma rakenduse installimine rohkem kui lihtsalt mitu kasutajat. On oluline teada, kuidas kasulik rakendus on ja kui selle kasutus trend muutub. Järgmised muutuvad oluline:

- Kasutajate on alguses leidmiseks rakenduse ebahuvitav või aegunud? 
- Mitu kasutajat lõpetanud rakendust üldse? 
- On-rakenduseostud trendid ülespoole või allapoole?
- Kasutajad ei suuda lõpuleviimiseks töövoogudes tõttu rakenduse või selle puudumine probleeme? 
- Saanud hoiate oma rakenduse kasulik ja asjakohased lükates värske sisu oma kasutaja alusel? 
- Soovite selle värske sisu olema sama kõikide kasutajate jaoks või kasutaja segmente rakenduse käitumise põhjal värskendustest? 
 
Vastused küsimustele sarnane need aitaks pikendada elu ja tulu rakenduste. Need aitavad teil määrata ja säilitada oma kasutaja base. 

Meediumi seotud osa kõrgeim säilituspoliitika kasutajate hulgas on tavaliselt rakendused. Üheks põhjuseks on pidevalt teenuseid värske sisu kasutajatele. Kasulik Tõuketeatiste suunatud kasutajasegmenti vastu võib mõjutada kõrge rakenduse säilitus. 

Azure'i Mobile kaasamine programmi eesmärk on aitavad pikendada elu ja säilitamine rakenduse esitada meetodi koguda ja analüüsida üksikasjalikku teavet rakenduse kasutamise kohta. See aitab teil liigitada teie kasutaja base vastavalt käitumine ja loomine aktiivse kampaaniat tõuketeatised ja -app meilisõnumite kohaletoimetamiseks määratletud kasutajate segmente. Tootluse võtmenäitajad (KPI) mõõtmiseks, kuidas aktiivsete kasutajate on erinevate aspektide rakenduse. Azure'i Mobile kaasamine pakub võimalust, peate need KPI-d määratlemiseks. See aitab suurendada infrastruktuuri, peate oma mobiilirakenduse abil kaasamiseks soovitud investeeringutasuvuse (ROI). 

Parimal viisil Azure Mobile kaasamine, peate alustada hästi kujundatud kaasamine leping. Teie lepingu aitavad teil tuvastada, peate saama segmendi oma kasutaja base Varundustöö andmed. See saab käitumise põhjal ja -rakenduse kogemusi. Selleks, et teie leping edukaks, on parim selgelt määratleda KPI-d, mis kuvatakse mõõtma eesmärkide rakenduse. Eemalda mõõdikud määratletud, mille saate manustada hõlpsalt vajalikud loogika rakenduse peene tekstuuriga andmekogumise, mille abil saate analüüsida ja hinnata oma KPI-d. See teema on parim praktiline KPI-d, mida kasutate oma kaasamine kava määratlemine. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Samm 1: Määratleda pakkumise mudeli vastavalt teie KPI-d


KPI-d õigesti määratlemine võib olla keeruline tööülesande lõpuleviimiseks. Erinevate ettevõtetele mõeldud rakendused on oma üksikasjad ja eesmärgid. See võib tavaliselt ajage oma lähenemine. Selleks, et vältida, tuleks liigitada eesmärgid ja KPI-d, kolme kategooriasse: **äri**, **kaasamine**ja **tehniline**. See on, mida me kutsume **pakkumise mudel**.

Hea plaan on üldiselt KPI-d, mille põhjal hinnatakse iga järgmise kategooria pakkumise mudeli edu eesmärke.


#### <a name="business-kpis"></a>Business KPI-d

Lihtsaim osa koostamiseks tuleks Business KPI-d. Ilmselt juba määratletud need vormis teie mobiilirakenduse planeerimisel. Neid KPI-d üldiselt aitavad tulu mõõt ja ROI saate rakenduse. Järgmisest loendist leiate mõned näide äri KPI-d, mis võib aidata teil samal ajal oma mõõdikud määratlemine:

- Meediumi Business KPI-d
    - Reklaamid klõpsamisel arv
    - Arv ning lehe kasutaja kohta
    - Praeguse tellimuste arv
- Mängimine Business KPI-d
    - Arvu-rakenduseostud.
    - Keskmine tulu kasutaja kohta (ARPU)
    - Ühe seansi veedetud aeg
    - Päeva esitatud ja praeguse mängu tase
- Pood Business KPI-d
    - Päeva app kasutada
    - Keskmine tulu kasutaja kohta (ARPU)
    - Keskmine summa ostukorvis maksma siirdudes
    - Enamik vaadetes ja ostetud tootekategooria
- Panga ja kindlustusseltsi äri KPI-d
    - Kontode arv
    - Funktsioonid, mis on aktiveeritud
    - Külastatud pakkumine
    - Teatiste klõpsamisel või aktiveerida      



#### <a name="engagement-kpis"></a>Kaasamine KPI-d

Kaasamine KPI-d on jõudluse näitaja mõõta kasutajate kaasamine. Selles jaotises trende tuvastada rakenduse säilitamine. Siin on mõned näide mõõdikud seda tüüpi KPI-d.

- Aktiivsed kasutajad viimase 7 päeva jooksul
- Passiivsed kasutajate arv viimase 7 päeva
- Kasutajad, kes on kasutanud rakenduse 30 päeva jooksul arv  

Mõned selge välise mõjutada näidikud selle ala. Näiteks võite kaaluda mobiilsideseadmes olevat kasutaja igal ajal. See võib või ei pruugi tõene. Mängimine rakendus võib tavaliselt on suurem kasutus pühade ajal, kui mängija võib esitamise ajal veel pole tööl või koolis.   

Määratletud KPI-d selle kategooria peaks aitama teil mõõta oma rakenduse ja klientide vaheline seose.



#### <a name="technical-kpis"></a>Tehniline KPI-d

Tootluse võtmenäitajate selle kategooria, mis aitavad teil kindlaks teha, kui teie rakendus on käituvad õigesti, hangumise või krahh. Nende näitajate saate mõõta oma rakenduse seisundi ja kasutatavuse probleemid, mis võivad takistada kasutajate rakenduse abil määrata. Selle kategooria kogutud teavet võivad sisaldada ka jõudluse teavet, mis on olulised turundus meeskondadel. Andmed võivad ka olema seda tõrkeotsingu jaoks ja toetavate teatamata vigade tuvastamiseks. 
 
Siin on mõned näited tehnilise KPI-d.

- Count ja töödeldud või töötlemata erandi teave 
- Ajatemplit viimase krahhi
- Viimase nupu klõpsamisel või külastatud viimasel lehel
- Rakenduse mälukasutus
- Rakenduse paneeli määr
- Opsüsteemi versioon, mis töötab rakendus
- Rakenduse versioon

Määratleda nende KPI-mõõt rakenduse jõudluse parandamiseks ja Pinpointi võimalikud vead. Selle näidikud aitavad vähendada aega, tuleb pakkuda oma klientidele Paranda. Need samuti aidata määrata kasutajasegmenti, kes on esinenud teatud probleemid. Selle kasutaja osadeks abil saate luua kampaaniat teatiste saadaval parandusi ja võimalike reklaamid taastamiseks klientide rahulolu. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Playbook ülesanne 1: Teie KPI Armatuurlaua loomine

Teie turunduse strateegia määratlemisel pean oma KPI-d vaate iga teie peamine eesmärgi. Nad peaksid olema selgelt määratletud andmepunktide, mis võimaldab teil jälgida oma rakenduse ja lõppkasutaja toimimist olulist teavet koguda.

Koostada KPI armatuurlaud, mis sisaldab soovitud teavet all

1.  Millised on selle rakenduse KPI-d?
2.  Milliseid andmeid andmepunktide saavad kasutada tähistada iga KPI-d?
3.  Kus asub andmed minu rakenduse (st Kuva, sätted, süsteemi...)?
4.  Valitud KPI saab esitada mõne kaasamine järjestuse?

**KPI Builder** töölehel saate kasutada meie [Meediumi Playbook malli] [ Media Playbook link] näited ja juhised.



## <a name="step-2-your-engagement-program"></a>Samm 2: Kaasamine programmi


Hea mobiilsideseadmete kaasamine programmi tuleks kaaluda rakenduse oluline osa. Täiesti siia suur Tere tulemast programm, mis käivitab kasutaja rakenduse kasutamine esimest päeva jooksul. See võib olla väga positiivne mõju kaasamine ja säilitamine rakenduse. Uuringud on näidanud, et enamik kasutajaid lõpetate rakenduse paar esimest päeva pärast installimist. Soovite püüdma vastavad või ületavad klientide oodata juhtimise intressi ennetähtaegselt, samal ajal, kui kasutaja on endiselt suunatud rakenduse. Veenduge, et esitada võtmeväärtuse ja rakenduse eelised oma klientidele. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Tõuketeatised on parim lähenemine ennetähtaegse kokkulepete mobiilsideseadme kasutajatega. Hoolikalt tuleb kui tõuketeatised kasutajate tükeldada. Kuna pärast kasutaja tunneb, nagu nad saavad rämpsposti või ebahuvitav teatised, saate seda on raske mõjutavad. Mõne klõpsuga, kustutada kasutaja rakenduse kunagi tagastamiseks. Kasutaja peaks saama väga isikupärastatud Rakendusesisese väärtus üldise rämpsposti asemel.

Kui kasutajad on aktiivselt, siis kaasamine programmi aitab juhtida muude aspektide rakendus.

Näiteks võib setup turunduskampaania, mis taotleb oma hindamise rakenduse aktiivsed kasutajad. Kuna selle kasutaja lõigu viimane aktiivne ja on enamiku kogemusi teiega rakendus, ootate tal anda kõige täpsem hinnang. Kui teil on suur rakenduse reiting, aitab see ketas üles ka oma uus klient omandamise kulude vähendamine rakenduse orgaaniline allalaadimine.



#### <a name="engagement-sequence"></a>Kaasamine järjestuse


Globaalne kaasamine programmi sisaldab erinevate kaasamine järjestuse. Iga jada eesmärk on mitu eesmärkide saavutamiseks.


###### <a name="life-push-sequence"></a>Kestus tõuketeatised järjestust


Kestus tõuketeatised jada on erinevad sõltuvalt kasutaja kaasamine rakendusega elutsükli. Teatud kasutaja võib uue, passiivsed või väga aktiivne. Mõne kaasamine elutsükli eri kasutajad võib kasu oma värske näpunäited või linke sisu dokumentatsiooni. 

Näiteks uue kasutaja võib vaja abi saada rakendusse rakendusse või kasu uue kasutaja huvi sarnaneb järgmisega esimest korda, kui nad käivitage rakendus...

*"Tore, et olete rongis! Ärge unustage sisse logida, 1 kuu tasuta hankida!"*


###### <a name="behavioral-push-sequence"></a>Käitumist tõuketeatised järjestust

Käitumist tõuketeatised jada eesmärk on suurendada põhineb kasutaja käitumine kogutud rakenduse kasutamine.  

Näiteks võib kasu fantaasia Jalgpalli rakenduse väga aktiivne kasutaja on tegelenud tõuketeatised järgmine teatis...

*"Juhan te on raske jalgpallifänn! Meie NFL osa sisse logida ja tasuta juurdepääsu SuperBowl võidavad!"*


###### <a name="alerting-push-sequence"></a>Tõuketeatised jada teavitamine

Kasutajate mõistate asjakohaste uudiste huvide värskendustest. Mõne teatiste tõuketeatised jada täiustab kaasamine huvide põhinevad teatised kasutaja on selgelt näidanud saatmisega. See võib olla konkreetsete, kui kasutaja valib oma huvide rakenduses. See võiks kindlaks, peidetult vastavalt kogutud interaktiivselt rakenduse andmeid.

Näiteks regulaarselt osta rakenduse pood kasutaja teatud kaubamärgi kohvi, mis on jäädvustatud äri KPI-d. Järgmine teatis võib selle kasutaja kaasamine rakendusega parandada.
 
*"Tere Wes, üks teie kohvi kuidas toimub müük 25% mai 2015 esimene nädal. Täname klient ja veenduge, et kalendriüksusi saate teadsid."*

###### <a name="rentention-push-sequence"></a>Rentention tõuketeatised järjestust

Järjestusel eesmärk on säilitamise abil korduvate tõuketeatised teatis kampaaniat ketas tavaline tavaks kaasamine rakenduse kasutajad. See aitab suurendada rakenduse säilituspoliitika, kui kasutaja on selle kasutusviisid. 

Järgmine kord nädalas põhineb kasutaja lemmik meeskonnad tõuketeatised teatis võib kuvada näiteks spordialad seotud rakenduse kasutaja:

*"Võimalus võidavad 200 punkti, avage hääletus Kas New York Yankees võidavad oma mängu sel nädalal vastu Toronto Blue pasknäärid!"*


#### <a name="the-3w-approach"></a>3W lähenemine

Juhtida erinevate tõuketeatised järjestuse aitab teil suhelda lõppkasutajad. Siiski peate endiselt kasutada 3W lähenemine isikupärastamiseks teie teatised. 3W lähenemine peaks käsitlema, kes, mida ja kui iga teatis jaoks. Kui te selgelt vasta kolmele küsimusele saate teatised peaksid olema õigesti suunatud kaasamine.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Kes: Kasutaja segmendi, mis saadetakse sõnumid

Teatiste lükkamine kasutajatele tuleks kaaluda väga tundlik side kanali. Veenduge, et teie eesmärk on saada kasutajasegmenti teatised liidetakse ka seda kasutajasegmenti huvides. Valesti suunatud teatis tõenäoliselt väga avalda negatiivset mõju kasutajale. Mis on desinstallitud rakenduse rämpsposti võivad need lugeda. 

Kasutage kombinatsiooni tehniline ja käitumist teatud kriteeriumide määratlemisel ei saa teateid ja kasutajale segmente. Lihtne näide määratlemine kasutajasegmenti võib olla umbes järgmine tekst:

"Kõik kasutajad, kes käivitatud soovitud mobiilirakenduse jaoks esimene kord 3 päeva tagasi ja külastatud sisselogimislehele kaks korda ilma tegelikku lõpuleviimist sisselogimist".
 
See lause aitab tuvastada, peate toetama teatud stsenaarium koguda andmeid.


###### <a name="what-the-message-that-you-will-send"></a>Mida: Sõnum, mille saadate

**Tooni**

Kasutage oma kokkulepete toon, mis sobib teie segmenditud kasutajate jaoks. See on kindlasti hea viis ühenduse loomiseks oma lõppkasutajatega ja esiletõstmine rakenduse kasutaja intressi. 

**Ümbersuunamine**

Tõuketeatised teatise saab kasutada rohkem kui rakenduse avamine. Kui teavitussõnum pakub kontekstis nagu leviedastuse uudised või toote edendamine, see teatis võib sügav link otse rakendusest õige sisu. Toetamiseks, peate looma URL-i skeem lasta rakenduse ümbersuunamise haldamine. See on oluline samm, mida ei saa unustada, kui töötab teie kaasamine järjestuse.

Ümbersuunamise saab hallata ka muud süsteemid. Näiteks toimingu URL-i abil on võimalik ümber suunata lõppkasutajad palju muudest süsteemidest, sh järgmist:

- Veebisait
- E-posti postkasti juba häälestamine
- On SMS ruut
- Sissehelistamise teenuse
- Otse, et rakenduste poe rakendus hindamine. 

See pakub palju võimalusi tegeleda lõppkasutajad ja parandada esinemiste automaatse reeglite loomine.


**Vormingu ja sisu**

Tüüpide ja tõuketeatised teatise vorme.

1. **Teadaannete** : võimaldab reklaami sõnumeid saata kasutajatele erinevatel (välja rakenduse rakenduse või iga kord).
2. **Küsitluste** : lubatud, saate teabe kogumine lõppkasutajad, esitades neile küsimustele. Nende vastused on saadaval siis kriteeriumid target lõppkasutajatele loomisel.
3. **Andmete sunnib** : võimaldab saata kahendarvuks või base64 andmefaili rakendust värskendada. Rakenduse kasutajate kogemuse rakenduse isikupärastamiseks saadetakse andmete tõuketeatised sisalduvat teavet. Rakenduse peab olema loodud andmete toetamiseks andmete tõuketeatised.
4. **Paanide (ainult Windows Phone)** : lubatud, saate selle Microsofti Push teatise teenuse (MPNS) abil saate saata kohalikke Push teate, mis sisaldab XML-andmete (alates SDK versioon 0.9.0 toetatud. Lõplik last paanide ei tohi ületada 32 KB.). Sõnumis kuvatakse otse oma tahvli paani.
5. **WebView** : veebivaate on hüpikakende sisaldavate veebisisu. Hüpikaken on tõuketeatised teatamise lõppkasutaja klõpsamisel. Veebivaate võimaldab teil on mitu suhtlemine lõppkasutaja.
 
>[AZURE.NOTE] Veenduge, et saadate nimega Tõuketeatiste sisu vastab vastav platvormi (iOS, Android, Windows) juhised rakenduste arendamise ja Tõuketeatiste saatmine.

 


###### <a name="when-the-timing-of-your-campaign"></a>Millal: Kampaaniat ajastuse

Kui on valida parima aja aktiveerimine kampaaniat käivitamise tõuketeatised? See peaks olema käsitsi või automaatselt? Peaks olema korduv? Õige aeg või sagedus on vaja kaasata kasutajate parimaid tulemusi. Iga kaasamine järjestuse ja stsenaariumi, peate määrama Millal saab saata Tõuketeatiste sobivaima aja. Siin on mõned võimalikud näited:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Kui saadate iga päev palju teatised, tuleb võtta raske arvesse võtta, et teie kasutajad võivad näha suhtluse rämpspostiks. 

Azure'i Mobile kaasamine pakub suhtluse kui rämpsposti vältimiseks kaks võimalust. Esmalt veenduge, et saate suunata sama kasutajate peene struktuuriga osadeks abil. Lisaks sisaldab Azure Mobile kaasamine "kvoot" funktsiooni. Selle funktsiooni saate piirata teatised saadetakse kampaaniat. Näiteks säte on vaikimisi kvoot 5 nädalas tagab, et kasutaja lisada osa lõigu turunduskampaania kasutaja saab kuni 5 teatisi, et nädal.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Playbook ülesanne 2: Looge oma kaasamine programmi

Vähe aega oma eesmärkide kokkuvõtmist ja määratlemine kampaaniat, eeldate esitamiseks konkreetsete toimingute abil. Veenduge, et rakendate 3W lähenemine teatised oma kampaanias. 

Töölehe **Kaasamine programmi** kasutamine [Meediumi Playbook malli] [ Media Playbook link] näited ja juhised.


## <a name="step-3-app-integration"></a>Samm 3: Rakenduse integreerimine


#### <a name="create-a-tag-plan"></a>Sildi plaani loomine

Azure'i Mobile kaasamine integreerida rakenduse peate plaani sildi loomiseks. Sildi leping ongi projekti. See määratleb turundustegevuse tehnilised andmed, töö voo rakendus ja rakenduse mõõta KPI-d kogutud andmete reaal sildi seos. See näitab, milliseid analytics saab vaadata portaalis. Samuti aitab teil määratleda kasutaja segmente ja saatke oma lõppkasutajad tegeleda aktiivse Tõuketeatiste. Kui teil on määratletud, sildi leping lisamise integreerida rakenduse kood on lihtne Azure Mobile kaasamine SDK abil.

Sildi leping peaks sildistamine kõik rakenduses. See peaks sisaldama ainult sildi andmeid, mis on osa teie mobiilne strateegia. See on tõenäoliselt mitmesuguse rakenduste vahel. [Meediumi Playbook malli] [ Media Playbook link] antud alusel Azure Mobile kaasamine aitab koostada sildi leping antud meetod. Kasutada oma sildi leping loomisse juhendina **Sildi kavandamine** töölehel.

Sildi jaotise määratlemisel töölehel on väga konkreetne. See on väga oluline, et vältida segadust. Täpsem teave saadetakse iga oodatud stsenaariumi, mis igal sildil. Kaasata, kus igal sildil on manustatud tegevuse nime. See kõik tuleks **teavitused** osa töölehe. Sildi kavandamine töölehel peaks olema peamine viide testi kinnitamiseks. 

Jaotises **andmete kogumise** tüübid, nimed, väärtused ja lisa-teave /-väärtuse paarideks nõutav iga sildi, millele on manustatud rakenduse otsimine peaks oma arendusmeeskonnale.

Soovitame läbivaatamise sildi leping koos kõigi meeskondade projektiga seotud. Tehke vajalikud parandused ja veenduge, et kõik on selge, turundus- ja meeskondade.

**Lause töö** töölehe saab kasutada kõigi projektiga seotud juhendit.


#### <a name="data-types"></a>Andmetüübid

Need on levinud tüüpi andmete toetavad Azure Mobile allikaid.

###### <a name="devices-and-users"></a>Seadmed ja kasutajad

Azure'i Mobile kaasamine tuvastab kasutajate luues iga seadme jaoks kordumatut tunnust. See identifikaator nimetatakse identifikaator (või deviceid). See on loodud nii, et kõik töötavad sama seadmes rakendused ühiskasutusse sama seadme identifikaator.

###### <a name="sessions-and-activities"></a>Seansid ja tegevused

Seanss on rakenduse juhitud kasutaja ühe eksemplari. Seansi ulatub hetkest kasutaja alustab rakendus, kuni see peatub.

Tegevus on loogiline rühmitamine teha rakenduse võib seansi ajal kogumi. See on tavaliselt kindla Kuva rakenduse, kuid see võib olla midagi määratletud loogika rakenduse. Vähemalt peaks sildistamine iga Kuva või tegevuse oma rakenduse. See võimaldab teil mõista kasutaja-tee.


###### <a name="events"></a>Sündmused

Sündmuste kasutatakse interaktiivselt rakenduse teatada. Neid saab kiirsõnumite toimingud, nt ühiskasutuse sisu või video käivitamine. Sündmuste sildistamine annab teile andmete kogumine, kus näidatakse, kuidas kasutajad suhtlevad rakendus. 

###### <a name="jobs"></a>Tööde haldamine

Töö kasutatakse aruande toiminguid, mis kestus. Mõned näited on järgmised.

- API kõnede teostamine
- Reklaamid Kuva aeg
- Tausta tööülesannete kestus 
- Ostu protsessi kestus
- Video vaatamine


###### <a name="errors"></a>Tõrked

Tõrgete kasutatakse teatada probleemidest tuvastatud rakenduse abil. Näiteks vale kasutajatoimingute või API kõne ebaõnnestumist.

###### <a name="application-information"></a>Rakenduse teave

Teenuserakenduse teabe (rakenduse teave) kasutatakse sildistamiseks kasutaja kogemusi rakendusega seotud andmed. See loob kasutaja suhtluse rakenduse. 

Antud rakendus – teave Key, Azure Mobile kaasamine ainult jälgib, Viimane väärtus (ajalugu). Rakenduse-teave näitab rakenduse või teie lõppkasutajad olekut. Näiteks olek sisse logida või kasutaja lemmik rühma.

###### <a name="crash-data"></a>Andmete ootamatult sulguda

Andmete automaatselt kogutud Mobile kaasamine SDK aruannete rakenduse tõrked ei käsitleja oli rakenduse krahhi. Näiteks töötlemata erandi, mis ilmneb.


###### <a name="extra-data"></a>Andmete eest

Sündmuste, tõrkeid, tegevused ja -tööde haldamine tõsta parameetritega. See on eriti-teave arendaja võivad pakkuda rakenduse teatud andmetena. See on oluline, et määratleda kohandatud osadeks. 

Näiteks väärtuse "artikkel"-siltide võimaldab teil lõigu lõppkasutajatele, kes vaadata kindla artiklis põhjal. Siiski, mis ei pruugi olla piisavalt. See võib olla parem, kui ka sama silti "artikkel" lisa-teave, näiteks "news_category" tegevuse. See oleks kasulik määrata dünaamiliselt lemmik kategooriates kasutaja jaoks. 

Lisa-teave on teatatud paaris /-väärtuse. Selle rakenduse näites oleks "news_category" eest-info kategooria väärtus. Näiteks "sports", "Standard" või "poliitika".





#### <a name="tag-and-sdk-integration"></a>Sildi ja SDK integreerimine 

Üksikasjalike juhiste saamiseks integreerimine Azure Mobile kaasamine SDK rakenduse, järgige dokumentatsiooni [Kaasamine SDK integreerimine](mobile-engagement-windows-store-integrate-engagement.md) Azure veebisaidil. Valige oma suunata platvorm lingid lehe ülaosas.

Soovitame luua projektide kahte rakenduste ehitatud Azure Mobile allikaid. Üks arengu ja testi lavastus ja teine tootmise lavastus. Teie IT-meeskond saab siis edendada testi lavastus tootmisele kui kasutajate sobivuse testimine õnnestus.



#### <a name="user-acceptance-testing-uat"></a>Kasutajale sobivuse testimine (UAT)

Kasutaja testimise (UAT) hõlmab kindlasti, et kõik toimib ootuspäraselt. Töövoogudes lõpule viia ning koguda kõiki vajalikke andmeid sildi teie lepingu puhul:
 
- Teavet sildistamine tuleb vastavalt dokumenteeritud AZME mõisted
- Kogu vajaliku teabe kogutakse (sh täiendavat teavet väärtust rakenduse teave väärtust)
- Nomenklatuuri vastab vastavalt yout sildi kavandamine
- On saadetud pole duplikaatide Sildid

Põhjalikult testida teatise käitumine, mida teil on manustatud rakenduse tüübid

- Teadete, küsitluste, andmete sunnib välja rakenduse ja rakendus
- Teksti/Web vaated
- Märgi värskendamine kategooriad



#### <a name="setup"></a>Häälestamine

Azure'i Mobile kaasamine seadistamine on väga lihtne. Kõik dokumendid, mis on seotud kasutajaliideses on saadaval veebisaidil Azure Mobile kaasamine, [Kuidas liikuda kasutajaliidese](mobile-engagement-user-interface-home.md).

Soovitatav on alustamist häälestades õige rollid ja rolli liikmelisused projekti kasutajate jaoks. See aitab teil õige platvormi kõigi kasutajate juurdepääsu haldamine. Teie rollid võivad olla:

- Administraatorid
- Arendajad
- Vaatajate 

Hiljem:
- Registreerige oma deviceID testida oma seadmes.
- Minge oma konto sätted ja diagrammid ja teatis kohaletoimetamise aja määrata oma ajavööndi ajavööndi häälestamine.
- Avage rakenduse ja registreerida "Rakenduse-info" peate target lõppkasutaja käeulatuses.

Oma esimese tõuketeatised teatis turunduskampaania käivitamise kohta lisateabe saamiseks vaadake [kasutamine ja haldamine sunnib jõuda oma lõppkasutajad alustamiseks](mobile-engagement-how-tos.md).



## <a name="conclusion"></a>Kokkuvõte


Kaasamine programmid on iteratiivne ja peaksite pidevalt parandada teie enda nagu saate proovida mis sobib kõige paremini teie rakendus. 

Esialgu ajal arendamise kogemus strateegiad ei püüdma kogu global suhete strateegia allikaid. Võtta samm-sammult lähenemine oma KPI-d ja kuidas neid kasutada. Strateegia on kordumatu iga rakenduse.

Pärast seda, kui olete loonud kogemusi võite soovi korral lisada oma kaasamine programmid järgmist:

- Jälgimine: Kasutajate viimistlemine ja määratlete ilmselt andmete kogumise allikad. Azure'i mobiilsideseadmete kaasamine saab linkida andmete kogumise allikad. See võimaldab teil jälgida esinemiste iga allikas. See teave on huvitav oma omandamise investeeringu maksimeerimiseks. 

- A ja B testimine: see on oluline osa kaasamine programmi. Iga rakendus on oma üksikasjad. A ja B testimine, saate parandada oma kaasamine programmi.

- Geograafilise asukoha: See on suur võimalus kaubamärkide. See funktsioon tänu jõuate õigesse kohta ja ajal. Soovitame, et kontrollida, kas olete kogunud piisavalt lõppkasutaja käitumine andmed enne kasutamist geograafilise asukoha.

- Andmete tõuketeatised: andmete push on mõni nähtamatu tõuketeatisesertide. Andmete tõuketeatised võimaldab kohandada rakenduse lõppkasutaja käitumise põhjal. Näiteks kui sageli konsulteerib kasutajasegmenti tehnoloogia tooted, rakenduse omanik saata andmed tõuketeatised, mis kuvatakse isikupärastamine tema avalehe tehnoloogia sisuga.



## <a name="next-steps"></a>Järgmised sammud

- [Loo konto Azure Mobile allikaid](mobile-engagement-create.md).
- Külastage [oma Mobile strateegia määratlemine](mobile-engagement-define-your-mobile-engagement-strategy.md) Lisateavet oma Mobile strateegia määratlemine.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
