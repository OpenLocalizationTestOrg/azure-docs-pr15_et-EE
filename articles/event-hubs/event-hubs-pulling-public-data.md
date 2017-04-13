<properties
    pageTitle="Avalikest andmetest tõmbamine üheks Azure'i sündmuse jaoturi | Microsoft Azure'i"
    description="Ülevaade sündmuse jaoturiga importimine web näidis"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Avalikest andmetest üheks Azure'i sündmuse jaoturi tõmbamine

Teil on tüüpilised Internet, asjade stsenaariumid, saate programmi tõuketeatised andmete Azure, kas mõni Azure sündmuse jaoturi või mõne asjade jaoturi seadmete. Mõlemad need jaoturi on talletamiseks, analüüsimine ja visualiseerimine Microsoft Azure kättesaadavaks tehtud hulgaliselt sissepääsud Azure'i sisse. Mõlemad nõuda vajutada andmete neile vormindatud JSON ja turvatud teatud viisil. Kuvatakse tabeliveergude järgmise. Mida teha, kui soovite andmeid tuua avalik või privaatne allikatest, kus andmed on esitatud veebiteenuse või kanali mingi, kuid teil on võimalus muuta seda, kuidas andmed on avaldatud? Kaaluge ilm või liikluse või börsidiagrammi hinnapakkumised - ei saa öelda NOAA, või WSDOT või NASDAQ oma sündmuse jaoturi PTT konfigureerimine. Probleemi lahendamiseks oleme kirjutada ja väike pilv näide, mis saate muuta ja juurutada, mis selliseid andmeallikast andmeid tõmmata ja lükake see oma sündmuse jaoturi avatud allikad. Sealt saate teha, mida iganes soovite, teema, et tootja litsentsitingimused. Rakenduse leiate [siit](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Pange tähele, et see proovi kood ainult kuidas tõmmata andmete tüüpiline veebist kanalid ja Azure sündmuse jaoturiga kirjutamist. See funktsioon pole mõeldud olema tootmise rakendus, ja pole katsete on tehtud teha seda sobib kasutamiseks sellise keskkonna - on strictfly DIY, arendaja värskendustest näide ainult. Lisaks selles valimi olemasolu ei ole samaväärne tuleks **pull** andmete Azure'i asemel **tõuketeatised** soovitus selle. Tuleks läbi vaadata turvalisuse, jõudluse, funktsioonid ja maksab tegureid enne lahendamise kohta – lõpuni arhitektuur.

## <a name="application-structure"></a>Rakenduse struktuuri

Rakendus on kirjutatud C# ja [valimi kirjeldus](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) sisaldab kogu teavet, mida soovite muuta, luua ja avaldada rakendus. Järgmistes jaotistes on üksikasjalik ülevaade sellest, mida rakendus ei.

Alustame eeldusel, et teil on juurdepääs andmekanaliga. Näiteks soovite tõmmata liiklus andmete olek Washingtoni osakonna transpordiga või ilm andmed NOAA, kohandatud aruandeid kuvamiseks või ühendada muude andmetega oma rakenduse andmeid. Peate ka seadistanud on Azure sündmuse jaoturi ja leida ühenduse string, mis on vaja juurde pääseda.

Lahendus GenericWebToEH sisselülitamisel loeb konfiguratsioonifail (App.config) saada mitmeid asju:

1. URL-i või andmete avaldamine saidi URL, loendit. Ideaalvariandis mitte see sait, mis avaldab JSON, nagu need WSDOT viidatud andmete on [siin](http://www.wsdot.wa.gov/Traffic/api/). 
2. URL-i, kui vajalik mandaat. Paljud Avalikud allikad ei pea identimisteabe või saate panna identimisteabe URL-string. Teiste jaoks on vaja, et teil on pakkuda eraldi. (Pange tähele, et saate ainult määrata ühe mandaadikomplekt selle rakenduse nii, et see toimib ainult siis, kui määrate, et ainult üks URL-i, mitte URL-ide loendit).
3. Ühendusstringi ja sündmuse jaoturi selle sündmuse jaoturi nimeruumi, millele vajutada andmed nimi. Selle teabe leiate Azure'i portaalis.
4. Une teatud millisekundites, intervalli küsitlused avalikest andmetest saidi jaoks. See säte nõuab mõtlema. Kui te küsitlus liiga harva, siis võivad jääda vastamata andmeid; Teisalt, kui te sageli liiga küsitlus, võidakse kuvada palju korduvate andmeid või hullem veel, mida võib olla blokeeritud õelate robot nimega. Kaaluge andmeallika värskendamise sagedust - ilm või andmed võivad värskendada iga 15 minuti järel, aga stock hinnapakkumisi võib-olla iga paari sekundi, sõltuvalt sellest, kust neid. 
5. Lipu rakenduse kindlaks teha, kas andmed on tulevad JSON-või XML-i. Kuna peate push andmed sündmuse jaoturiga, taotlus on mooduli XML-i teisendamiseks JSON enne saatmist.

Pärast konfiguratsiooni faili lugemist, läheb rakenduse tsükkel - andmete vajaduse korral oma sündmuse jaoturi kirjutamist ja seejärel ootamine une intervall enne seda kõik uuesti teisendamine avaliku veebisaidi juurdepääs. Täpsemalt:

  * Avaliku veebisaidi lugemine. Andmete vastuvõtuks valmis-saada kasutatakse Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs RawXMLWithHeaderToJsonReader klassi eksemplari. See loeb allika voo GetData() meetodi ja seejärel jagab selle abil GetXmlFromOriginalText väiksem andmeühikuga (st kirjed). 
  See meetod ei loe XML-i ning samuti regulaarne JSON või JSON massiiv. Seejärel töötlemine käivitatakse MergeToXML konfiguratsiooni App.config (vaikimisi = tühja).
  * Saatmise ja vastuvõtmise andmed on rakendada esitatavaks sisse Program.cs Process() meetod. 
  Pärast GetData() väljundi tulemuste saamine, eraldada meetod enqueues sündmuse jaoturi väärtused.

## <a name="next-steps"></a>Järgmised sammud

Lahendus juurutamiseks klooni või [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) rakenduse allalaadimine App.config faili redigeerimiseks, ehitada ja lõpuks avaldada. Kui rakendus on avaldatud, näete seda töötab Azure klassikaline portaalis jaotises pilveteenustega ja saate muuta mõne konfiguratsioonisätete (nt sündmuse jaoturi target ja une intervall) vahekaarti **konfigureerimine** .

Vaadake veel sündmuse jaoturi näidised [Azure näidised Galerii](https://azure.microsoft.com/documentation/samples/?service=event-hubs) ja [MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)-is.
