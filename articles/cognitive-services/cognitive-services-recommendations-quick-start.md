<properties
    pageTitle="Lühijuhend: seadme õ soovitused API | Microsoft Azure'i"
    description="Azure'i masina Õppekeskuse soovitused - Lühijuhend"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Kognitiivne teenuste soovitused API Lühijuhend

Dokumendis kirjeldatakse, kuidas endal teie teenuse või rakenduse kasutamiseks [Soovitused API](http://go.microsoft.com/fwlink/?LinkId=759710).
Soovitused API ja muude kognitiivse teenuste kohta rohkem üksikasju leiate [siit](http://go.microsoft.com/fwlink/?LinkId=759709). Sellest juhendist kogu võite leida [Soovitused API viide](http://go.microsoft.com/fwlink/?LinkId=759348) mugav.


<a name="Overview"></a>
## <a name="general-overview"></a>Ülevaade

See dokument on samm-sammult juhendi. Meie eesmärk on sõelub koolitada mudeli ja osutage ressursse, mis võimaldab teil peab olema mudeli kaudu tootmiskeskkonnast tarvilikud toimingud.

Selle ülesande võtab aega umbes 30 minutit.

[Soovitused API](http://go.microsoft.com/fwlink/?LinkId=759710)kasutamiseks peate tegema järgmist:

1. Andmemudeli loomine – mudeli on teie andmeid, kataloogi andmed ja mudel soovitus.
1. Andmete importimine kataloogi - kataloogiga sisaldab metaandmete teavet üksused.
1. Andmete importimine kasutus - kasutusandmete saab ühel viisil 2 (või mõlemad) üles laadida:
  -  Kasutus andmeid sisaldava faili üleslaadimisel.
  -  Saatmine andmeid tuua sündmused.
  Tavaliselt saadate kasutus faili, et luua mudelit, mis algse soovitus (bootstrap) ja seda kasutada kuni süsteemi kogutakse piisavalt andmeid tuua andmevorming abil.
1. Luua soovitus mudel – see on asünkroonne toiming, kus soovitus süsteemi võtab kasutamine andmete ja loob mudeli soovitus. See toiming võib võtta mitu minutit või mitu tundi suurusest andmed ja konfiguratsiooni parameetrid koostamine. Kui käivitamise koostamine, saate Koosta ID-ga. Selle abil saate kontrollida, kui Koosta protsess on lõppenud enne alustamist kasutamine soovitusi.
1. Soovitused - Get kindla üksuse või üksuste loendi jaoks kasutada.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Alustagem!

Käivitate soovitused mudeli loomine. Seejärel Juhendame Teid kohta, kuidas kasutada power soovituste pood saidil mudeli tulemusi.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Ülesanne 1 - allkirjastamiseks üles API soovitused ####

Selles ülesandes tuleb soovitused API teenuse kasutajaks ja soovitused andmemudeli loomine.

1. Avage **Azure portaali** [http://portal.azure.com/](http://portal.azure.com/) ja logige sisse oma Azure kontoga.

1.  Valige + **Uus**.

1. Valige suvand **ärianalüüsi** .

1. Valige **Kognitiivse teenuste API -de** toode.
See toode võimaldab teil alustada tellimuse kognitiivse teenustega API-de (näo, teksti Analytics, arvuti nägemine jne). Täna me keskenduda soovitused API-ga.

1. Kognitiivne Services API sihtleht, sisestage **konto nimi** tellimuse soovitusi. (Jaoks instace: "MyRecommendations"). See nimi ei tohi olla tühikuid ei.

1. Valige **soovitused** **API tüüp**.

1. **Hinnakirjad taseme**, saate valida leping. Võite valida **tasuta** taseme 10 000 tehingud kuus. See on tasuta lepingut, nii et see on hea võimalus alustada üritab süsteem. Kui lähete tootmise, soovitame teil arvesse võtta teie taotlus helitugevust ja muuta lepingu tüübist. (Märkus: saate määrata ainult ühe taseme tasuta tellimuse korraga)

1. Valige **Ressursirühm**või looge uus, kui teil pole veel üks.

1. Saate muuta muude elementide dialoogiboksis loomine. Meil peaks meelde, et ressursi pakkuja täna on toetatud ainult USA andmekeskuste kaudu.

1. Kui olete teinud kõik valikud, klõpsake nuppu **Loo**.

1. Oodake mõni minut ressursi kasutusele võtta.
Kui see on juurutatud, saate minna **klahvid** jaotise **sätted** tera kui teile antakse esmaseid ja teiseseid kasutada API võti.  Kopeerima primaarvõtme, peate selle esimese mudeli loomisel.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Ülesanne 2: Kas teil on oma andmeid? ####

Soovitused API teada teie kataloogi ja tehinguid hea toode soovituste andmiseks. Mida tähendab, et peate selle kanali hea andmetega oma toodete (me kutsume seda faili **kataloogi** ) ja kogumi tehingud piisavalt suur, et leida huvitav mustrite tarbimine (me kutsume selle **kasutamine**) kohta.

1. Tavaliselt soovite päringu andmebaasi tehinguid nende teabe puhul.
Ainult juhul, kui teil pole need käepärast, oleme teile Microsoft Store tehingu andmete põhjal andnud mõningaid näidisandmeid.

 Andmeid saate alla laadida [siin](http://aka.ms/RecoSampleData). Kopeerimine ja kausta teie kohalikus arvutis olevat MsStoreData.Zip lahti.

 > **Märkus:** Proovi kood, mida te alla laadida ja käivitada ülesanne 3 on juba manustatud sees – nii, et see toiming on valikuline näidisandmeid.  See tähendab, et see toiming võimaldab teil suurema andmekogumi alla laadida ja võimaldab teil sisendeid üheks soovitused API paremini mõista.

1.  Nüüd Heitkem pilk kataloogi fail. Liikuge asukohta, kuhu kopeerisite andmed.
 Avage fail kataloogi **Notepadis**.

 Märkate, et kataloogi fail on üsna lihtne. See on järgmises vormingus`<itemid>,<item name>,<product category>`

 >  AAA-04294, OfficeLangPack 2013 32/64 E34 Online DwnLd Office <br>
 > AAA-04303, OfficeLangPack 2013 32/64, ET Online DwnLd Office  <br>
 > C9F-00168, KRUSELL Kiruna pöörata tiitellehe Nokia Lumia 635 - Camel, tarvikud

 Meil peaks meelde, et kataloogi faili saab märksa, näiteks saate lisada metaandmete (me kutsume need *funktsioonid üksuse*) toodete kohta. Peaksite nägema [kataloogi vorming](http://go.microsoft.com/fwlink/?LinkID=760716) jaotis API viite Lisateavet kataloogi vorminguga.

1. Vaatame tehke sama kasutuse andmetega. Märkate, et kasutus kuupäev on vormingu `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015/08/04T11:02:52, ostu 0003BFFDD4C2148C, 297833400, 2015/08/04T11:02:50, ostu 0003BFFDD4C2118D, 297833300, 2015/08/04T11:02:40, ostu 00030000D16C4237, 297833300, 2015/08/04T11:02:37, ostu 0003BFFDD4C20B63, 297833400, 2015/08/04T11:02:12, ostu 00037FFEC8567FB8, 297833400, 2015/08/04T11:02:04 ostmine

Pange tähele, et esimesed kolm elemendid on kohustuslikud. Sündmuse tüüp pole kohustuslik. Saate vaadata [kasutus vorming](http://go.microsoft.com/fwlink/?LinkID=760712) selle teema kohta lisateavet.

 > **Kui palju andmeid on vaja?**
 <p>
Hästi, on tõesti sõltub ise kasutusandmete. Süsteemi õpib, kui kasutajad osta erinevatele üksustele. Mõned järgud nagu FBT, on oluline teada, millised üksused on ostetud sama tehingud. (Me kutsume selles *osaleda juhud*). Hea rusikareegel on, et enamike üksuste jaoks tuleb 20 tehingud või rohkem, et kui oleksite kataloogis 10 000 üksust, soovitame, et teil on vähemalt 20 korda numbrit tehingute või umbes 200 000 tehingud.
See on taas rusikareegel. Peate eksperimenteerida oma andmed.
</p>

<a name="Ex1Task3"></a>
####Ülesanne 3 - soovitused mudel####

Nüüd, kui olete konto ja teil on andmeid, loome esimese mudelisse.

Selles ülesandes on valimi rakenduse abil luua oma esimene mudel.

1. Kõigepealt peaks olema teadlik [Soovitused API viide](http://go.microsoft.com/fwlink/?LinkId=759348).

1. [Valimi rakenduste](http://go.microsoft.com/fwlink/?LinkID=759344) allalaadimiseks kohalikus kaustas.

1. Avage Visual Studios **RecommendationsSample.sln** lahenduse, mis asub kaustas **C#** .

1. Avage fail **SampleApp.cs** . Märkus, et faili juhiseid järgmist:
 + Andmemudeli loomine
 + Kataloogi faili lisamine
 + Kasutus-faili lisamine
 + Mudeli ehitada käivitamine
 + Üksuste paari põhjal soovituse hankimine
<p></p>

1. Ülesanne 1 võtme **AccountKey** välja väärtus asendada.

1. Samm lahenduse kaudu ja te näete, kuidas saab loodud mudeli.

1. Proovige asendades lihtsalt allalaaditud uue andmemudeli loomine Microsoft Store või raamatu soovitusi failid kataloogi ja kasutamist. Peate ka mudeli nimi ja üksused, mille te soovitusi taotlemine muutmiseks.

1. Mudeli loomisel pange tähele **mudeli ID** , kui teil on soovitusi tootmiskeskkonnas taotlemisel.

>  Lugege lisateavet koostada tüüpide ja kvaliteedi hindamiseks järgud [siin](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Lihtsate mudelisse valmistamisel! ###

Nüüd, kui teil mõista, kuidas andmemudeli loomine ja tarbimine soovitusi, järgmise sammuna tuleb panna selle veebisaidi mobiilirakenduse valmistamisel või teie CRM või ERP süsteemi integreerida.
Ilmselt iga need rakendused oleks erinev. Kuna soovitused API on nõutud edastamine veebiteenusele, peaks oskama helistada mis tahes erinevates keskkondades hõlpsalt.

**Oluline:**  Kui soovite kuvada soovitusi avaliku klientarvutist (näiteks pood saidi), peaksite looma soovituste andmiseks puhverserverit. See on oluline, et teie API võti (potentsiaalselt ebausaldusväärsete) väliste üksuste on avatud.

Siin on mõned ideed kohad, kus saate kasutada soovitusi:

### <a name="product-page"></a>Toote leht

**Üksuse soovitused**
<p>Kui mudel oli väljaõppe ostu andmete põhjal, võimaldab see teie klient, saate *teada, mis võivad huvi pakkuda inimesed, mille olete ostnud kirje allikas tooted*.</p>
<p>Kui mudel oli koolitada, klõpsake andmete põhjal, võimaldab see oma kliendi avastada *tooted, mis võivad külastada inimesed, mis on külastatud kirje allikas*. Seda tüüpi mudeli võib tagastada sarnaseid üksusi.</p>

**Sageli ostnud koos soovitused**
<p>Sageli ostnud koos koostamine A võiks koolitada, siis võite saada selliste üksuste komplektides, on tõenäoline, et osta koos selle üksuse.</p>

### <a name="check-out-page"></a>Vaadake lehel

**Üksuse soovitused**
<p>Soovitused, mis on mudeli võib võtta Sisestage üksuste loendit. Nii saate võib edastada kõigi üksuste ostukorvi sisendina, et saada soovitusi.
Sel juhul mudel annab soovitused, mis on antud kõigi üksuste ostukorvi.
</p>

### <a name="landing-page"></a>Sihtleht

**Kasutaja soovitused**
<p>
Soovitused, mis on mudeli saavad võtta sisestada kasutaja ID-d.  Ajalugu tehingud, et kasutaja kasutab määratud kasutajale isikupärastatud soovituste andmiseks.
</p>

Tutvuge [Saada üksuse soovitused dokumentatsiooni](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Mis saab edasi?
Kui olete teinud see palju õnne palju! Lisateavet selle kohta lisateavet leiate täieliku [Soovitused API viide](http://go.microsoft.com/fwlink/?LinkId=759348) kui teil on küsimusi, ärge kõhelge meilemlapi@microsoft.com
