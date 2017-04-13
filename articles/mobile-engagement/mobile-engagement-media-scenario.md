<properties 
    pageTitle="Azure'i Mobile kaasamine rakendamist meediumi rakenduse"
    description="Meediumi rakenduse stsenaarium rakendada Azure Mobile kaasamine" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-media-app"></a>Rakendada mobiilsideseadmete kaasamine meediumi rakendusega

## <a name="overview"></a>Ülevaade

John on Mobiiliversiooni projektijuht suur meediumi ettevõtte jaoks. Ta hiljuti uue rakenduse, mis on väga kõrge allalaadimine loendus. Ta on tabas oma eesmärgid allalaadimiseks, kuid endiselt tema naasmiseks klõpsake Investment(ROI) kasutaja kohta ei vasta tema nõuetele. 

John on juba tuvastatud, miks tema ROI on liiga nõrk. Kasutajad sageli lõpetama oma rakenduse pärast ainult 2 nädalat ja enamik neist kunagi tulevad tagasi. Ta soovib suurendada oma rakenduse säilitamine.

Pärast mõned esialgsed testimine, ta on õppinud, kui ta tegeleb oma kasutajate Tõuketeatiste, kus nad tavaliselt oma rakendusega edasi. Kasutajad, mis olid passiivsed sageli tulemuseks ka rakenduse sõltuvalt ta neile saadetakse teatised. John otsustab investeerida kaasamine programmi oma rakenduse, mis kasutab täpsemalt suunamise Tõuketeatiste mingi.

John on viimati lugeda [Azure Mobile kaasamine - alustusjuhend koos heade tavade](mobile-engagement-getting-started-best-practices.md) ja on otsustanud rakendada juhend.

##<a name="objectives-and-kpis"></a>Eesmärgid ja KPI-d

Peamised Johni rakenduse koosolek. Reklaamid luuakse Business nagu kasutajate tarbimine oma meediumi. Kasutaja kohta manustatud sisu suurendades John suurendab tema tulude. Kõik leppida ühe peamine eesmärk: 25%-ga müügi reklaamid suurendamiseks. Nad loovad ettevõtte tulemuslikkuse võtmenäitajate (KPI-d) mõõt ja ketas eesmärk

* Arvu reklaamid klõpsamisel kasutaja kohta.
* Kui palju artiklilehtede külastatud (iga kasutaja / ühe seansi / nädalas / kuus...)
* Mis on lemmik kategooriaid

Peamised Johni koosoleku põhjal ta on määratletud oma äri KPI-d. Ta järgib 1 osa [Azure Mobile kaasamine - alustusjuhend koos heade tavade](mobile-engagement-getting-started-best-practices.md)kohta. 

Järgmisena loob ta järgmised lingid KPI tagamaks, et eesmärke on jõudnud.

* Säilituspoliitika jälgimine üle järgmised intervallide: iga päev, nädala, kahe nädala ja kuu.
* Aktiivsete kasutajate arv
* Rakenduse reiting rakenduse talletab

Soovitused IT-meeskonna põhjal, tehniline järgmised KPI-d on lisatud vastavad järgmistele küsimustele:

* Mis on minu kasutaja tee (mis leht on külastatud, kui palju aega kasutajate kulutada)
* Arvu jookseb või vead ilmnes seansi kohta?
* Millised OS versioonid töötavad minu kasutajad?
* Mis on keskmise suurusega Kuva minu kasutajate jaoks?
* Millist liiki Interneti-ühendused kas minu kasutajatel on?

Iga KPI ta jaotab vajalikud andmed ja ta salvestab selle oma playbook õiges kohas.

## <a name="engagement-program-and-integration"></a>Kaasamine programm ja integreerimine

Nüüd kui John on lõpule jõudnud, määratledes oma KPI-d, alustab ta oma kaasamine etapi määratledes 4 kaasamine programmid ja nende eesmärgid:    ![][1]

Seejärel John läheb süvitsi iga programmi Tõuketeatiste täpsustades. Tõuketeatised teatis on määratletud viis elemendid:

1. Eesmärk: mis on teatise eesmärk
2. Kuidas jõutakse eesmärk
3. Target (sihtkoht): kes saavad teatise?
4. Sisu: Mis on sõnastus ja vorming teatis (sisse App/Out, rakendus)
5. Millal: mis on parim hetk tõuketeatised teatise saatmiseks

    ![][2]

Lisateabe saamiseks vaadake [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

2 osa raamatus vastavalt John kasutab target jaotise määratleda, mis andmeid ta on koguda ja kirjutab oma sildi kavandamine koos selle meeskonnatöö lahendus kasutusele võtta. Pärast rakendamise ja kasutajale testimise nädal, Juhan lõpuks käivitada oma programmid.

##<a name="program-results"></a>Programmi tulemused

4 kuud hiljem vaatab läbi John esinemiste programmide. Tere tulemast programm ja nädala programm on eesmärkide tema. Ainult ühe seansi kasutaja arv väheneb, kasutatakse funktsioonide rakenduse ja arv nädalas ühendused on kahekordseks.

**Passiivsed programmi** aitab mõista kasutaja tendentsid John. Tundub, et 15% passiivsed kasutajad naaske rakendusse. Aga enamik neist ei peatada aktiivne rohkem kui 1 kuu. John näeb võimalikud optimeerimine see jada täiendavad teatised ja laiendamine oma sisu valikuid.

**Tutvumine programmi** ei tööta ka. See suurendab rist müügi, kuid ei piisa oma eesmärkide saavutamiseks. John tuvastab pole piisavalt andmeid oluline suunamise ja paku asjakohase sisu. Ta lõpetab see programm ja keskendub saatmine "Juhtkiri Tõuketeatiste" koos Azure Mobile allikaid. Oma ajakirjanikud juba CMS lahenduse saatmiseks tõuketeatisi ja nad ei soovi muuta.

John otsustab saavutamiseks API, mis on HTTP REST API, mis võimaldab hallata REACHi kampaaniat ilma kasutajaliides AZME Web kasutada. Seda moodust John koguda andmeid ta peab ja luba kasutada CMS lahenduse tema poolt.

Selleks, et see funktsioon töötab õigesti, Juhan küsib see meeskonnatöö valvsuse järgmistes küsimustes:

1. **Operatsioonisüsteemidega** : need kõik on oma reeglid võimaluse haldamiseks seonduvale Tõuketeatiste, et John otsustab alati loendi ja kontrollib, kui selle API-de hakkama.
N.: Android push süsteem võimaldab suur pilt, mis ei ole iOS-i puhul.

2. **Aja jooksul**: John soovib API, mis ajavahemiku ja lõpeb määratud seostada. Ta soovib säilitada kasutajate mis tahes häiriva teatis pommitamine.

3. **Kategooria**: turundus meeskonnatöö valmistab malli igat tüüpi teavitamine. John küsib see meeskonnatöö kategooria sees API määramiseks.

Pärast mõned katsed John on täidetud. Tänu selle API ajakirjanikud saate ikka saata Tõuketeatiste CMS ja Azure Mobile kaasamine kogub kõik käitumist andmed nende jaoks

Pärast nende 4 esimest kuud, tulemused kajastavad hea üldise jõudluse ja annab confidence John ja oma tahvli ROI kohta kasutaja suureneb 15% ja mobiilsideseadmete müüginäitajad moodustavad 17,5% kogukäibest, 7.5% kasvu ainult neli kuud.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
