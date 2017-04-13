<properties 
    pageTitle="Azure'i Mobile kaasamine rakendamine rakenduse mängimine"
    description="Rakenduse stsenaarium rakendada Azure Mobile kaasamine mängimine" 
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

#<a name="implement-mobile-engagement-with-gaming-app"></a>Rakendada mobiilsideseadmete kaasamine mängimine rakendusega

## <a name="overview"></a>Ülevaade

Mängimine käivitamine on käivitanud uue Kalastamine vastavalt rollimäng play/strateegia mängu rakenduse. Mängu on tööks 6 kuud. See aeg on suur edu, ja see on miljoneid allalaaditavad failid ja säilitamine on väga kõrge võrreldes teiste käivitamise mängu rakendused. Koosolekul kvartali ülevaade huvirühmade nõus neile vajalikule suurendamiseks keskmiselt tulude kasutaja kohta (ARPU). Premium-mängu paketid on saadaval pakkumisi. Nende mängu pakendis lubada kasutajatel uuendada ilme ja nende õngenöör ja värvi või tegeleb mängu jõudlust. Kuid paketi müük on liiga nõrk. Nii need esmalt otsustada klientide programmikasutuskogemuse Kasutusanalüüsi tööriista analüüsimiseks ja seejärel välja töötada ka kaasamine programmi suurendamiseks müük abil täpsemalt osadeks.

Vastavalt [Azure Mobile kaasamine - alustusjuhend koos heade tavade](mobile-engagement-getting-started-best-practices.md) kohta nad on strateegia koostamine.

##<a name="objectives-and-kpis"></a>Eesmärgid ja KPI-d

Peamised mängu kokku. Kõik leppida ühe peamine eesmärk – premium paketi müügi suurendamiseks 15%-ga. Nad loovad ettevõtte tulemuslikkuse võtmenäitajate (KPI-d) mõõt ja ketas eesmärk

* Mis tasemel mängu on need paketid osta?
* Mis on tulu kasutaja kohta, ühe seansi, nädalas ja kuus?
* Milliseid lemmik osta?

[Alustamisjuhendi](mobile-engagement-getting-started-best-practices.md) osa 1 selgitatakse, kuidas määratleda ja KPI-d. 

Business KPI-de nüüd määratletud, Mobile müügijuht loob uue kasutaja trendide ja säilituspoliitikate määratlemiseks kaasamine KPI-d.

* Säilituspoliitika jälgimine ja üle järgmised intervallide kasutamine: iga päev, iga 2 päeva, iga nädal, iga kuu ja iga 3 kuud
* Loendab aktiivsele kasutajale
* Rakenduse reiting poes

Soovitused IT-meeskonna põhjal, tehniline järgmised KPI-d on lisatud vastavad järgmistele küsimustele:

* Mis on minu kasutaja tee (mis leht on külastatud, kui palju aega kasutajate kulutada see)
* Arvu jookseb või vead ilmnes seansi kohta
* Millised OS versioonid töötavad minu kasutajad?
* Mis on keskmise suurusega Kuva minu kasutajate jaoks?
* Millist liiki Interneti-ühendus kas minu kasutajatel on?

Iga KPI Mobile müügijuht täpsustatakse andmeid peab ta ja kus see asub tema playbook.

## <a name="engagement-program-and-integration"></a>Kaasamine programm ja integreerimine

Enne koostamise täpsemalt kaasamine programmis, projekti eest vastutav Mobile projektijuht peaks olema mõistmiseta, kuidas ja toodete tarbib kasutajad.

Kolm kuud pärast Mobile projektijuht kogutud piisavalt andmeid oma Rakendusesisese tõuketeatised teatis müük täiustamiseks. Ta saab teada, et:

* Esimese ostu juhtub tavaliselt tasemel 14. 90% juhul, on uued legendaarne relvad $ 3.
* 80% juhtudel, ostude kasutajatele, kellel on tehtud ostu, jätkake toote ja teha veel.
* Kasutajad, kellel on möödunud tase 20, käivitage üle nädala $10 kulutama.
* Kasutajatele endiselt osta premium pakettide tasemel 16, 24 ja 32.

Tänu analüüsi Mobile projektijuht otsustab luua teatud tõuketeatised teatis järjestuse rakenduse müügi suurendamiseks. Ta loob kolme tõuketeatised järjestuse, mida ta nõuab: Tere tulemast programmi, müügi programmide ja passiivsed programmi. Lisateabe saamiseks vaadake [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
