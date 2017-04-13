<properties 
    pageTitle="Diagnostika otsingu abil | Microsoft Azure'i" 
    description="Otsing ja üksikuid sündmusi, taotlusi, filtreerimine ja logige jälgi." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/09/2016" 
    ms.author="awills"/>
 
# <a name="using-diagnostic-search-in-application-insights"></a>Rakenduse ülevaated diagnostika otsingu kasutamine

Diagnostika otsing on [Rakenduse ülevaated] [ start] saate otsida ja analüüsida üksikute telemeetria üksused, nt lehe vaated, erandid või web taotlused. Ja saate vaadata log jälgi ja sündmusi, mida teil on kodeeritud.

## <a name="where-do-you-see-diagnostic-search"></a>Kui näete diagnostika otsing?


### <a name="in-the-azure-portal"></a>Azure'i portaalis

Saate avada diagnostika otsingu otseselt.

![Avatud diagnostika otsing](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)


Avaneb ka mõned diagrammid ja ruudustiku üksuste klõpsamisel. Sel juhul filtrites eelnevalt seatud keskenduda valitud üksuse tüüp. 

Kui teie taotlus on veebiteenuse, kuvatakse ülevaade tera helitugevuse taotluste diagrammile. Klõpsake seda ja saate üksikasjalikumat diagrammile koos näitab, mitu taotlusi on tehtud iga URL-i kirje. Klõpsake ükskõik millisele reale ja saate üksikuid taotluste selle URL-i.

![Avatud diagnostika otsing](./media/app-insights-diagnostic-search/07-open-from-filters.png)


Diagnostika otsingu põhiosa on loend telemeetria üksuste - server nõuab, lehel vaateid, kohandatud sündmused, mis teil on kodeeritud ja jne. Loendi ülaosas on kokkuvõte skeem, mis näitab loendab sündmuste aja jooksul.

Sündmuste tavaliselt ilmu diagnostika otsingu enne, kui need kuvatakse argumendil Exploreris. Kuigi tera värskendab ise intervalliga, kui võite klõpsata nuppu Värskenda ootate teatud sündmuse jaoks.


### <a name="in-visual-studio"></a>Visual Studio

Avage aken otsing Visual Studios.

![](./media/app-insights-diagnostic-search/32.png)

Aknas Otsing on samad funktsioonid, mis veebiportaali.

![](./media/app-insights-diagnostic-search/34.png)


## <a name="sampling"></a>Valimite

Kui teie rakendus loob palju telemeetria (ja kasutate ASP.net-i SDK versioon 2.0.0-beta3 või uuem versioon), kohandatava valimite mooduli vähendab automaatselt maht, mis saadetakse portaali saatmisega tüüpilised murdosa sündmused. Siiski sündmused, mis on seotud sama kutse valitud või märkimata rühmana, et seotud sündmused seas saate liikuda. 

[Vaadake, kuidas valimite](app-insights-sampling.md).


## <a name="inspect-individual-items"></a>Üksikute üksuste uurimine

Valige mis tahes telemeetria üksus võtmeväljade kuvamiseks ja seostuvad üksused. Kui soovite näha kõiki välju, klõpsake nuppu "...". 


![Töö uus üksus, muutke välju ja seejärel klõpsake nuppu OK.](./media/app-insights-diagnostic-search/10-detail.png)

Kõigi väljade otsimiseks saate kasutada lihtsat stringide (ilma metamärkide). Saadaolevad väljad sõltuvad telemeetria tüüp.

## <a name="create-work-item"></a>Töö üksuse loomine

Mis tahes telemeetria üksusest üksikasjadega saate luua Visual Studio meeskonnatöö teenuste viga. 

![Töö uus üksus, muutke välju ja seejärel klõpsake nuppu OK.](./media/app-insights-diagnostic-search/42.png)

Esimest korda selle tegemiseks, palutakse lingi oma meeskonnatöö teenuste konto ja projekti konfigureerimine.

![Sisestage oma meeskonnatöö teenuste serveri URL-i projekti nime, ja klõpsake nuppu Autoriseerin](./media/app-insights-diagnostic-search/41.png)

(Samuti saate konfiguratsiooni tera sätted > tööüksusi.)

## <a name="filter-event-types"></a>Filtreerimine sündmuste tüübid

Avage filtri tera ja valige sündmuste tüübid, mida soovite näha. (Kui hiljem, mida soovite taastada filtrid, kellega te avasite tera, klõpsake nuppu Lähtesta.)


![Valige käsk Filtreeri ja valige telemeetria tüübid](./media/app-insights-diagnostic-search/02-filter-req.png)


Sündmuse tüübid on:

* **Jälita** - diagnostikalogid, sh TrackTrace, log4Net, NLog ja System.Diagnostic.Trace kõned.
* **Taotleda** - HTTP päringuid, mis on saadud serveri rakenduse, lehtede, skriptide, pildid, laadi faile ja andmeid. Need sündmused kasutatakse koosolekukutsete ja kutsele vastamise ülevaade Diagrammide loomine.
* Lehe view aruannete loomiseks kasutatud **Leheküljevaade** – telemeetria web kliendi poolt saadetud. 
* **Kohandatud sündmuse** – kui lisasite kõned TrackEvent() Selleks, et [jälgida kasutust][track], saate neid otsida siin.
* **Erandi** - server ja need, mis TrackException() abil logite sisse tabamatu erandid.

## <a name="filter-on-property-values"></a>Atribuudi väärtuste filtreerimine

Saate filtreerida sündmuste kohta oma atribuute. Saadaolevad atribuudid sõltuvad teie valitud sündmuste tüübid. 

Näiteks valige välja taotlusi kindla vastuse koodi.

![Laiendage atribuudi ja väärtuse valimist](./media/app-insights-diagnostic-search/03-response500.png)

Ei sisalda väärtusi, teatud atribuudi valimise on sama mõju, valides kõik väärtused; See aktiveerib selle atribuudi filtreerimine välja.


### <a name="narrow-your-search"></a>Otsingu piiritlemine

Pange tähele, et loendab paremas servas filtri väärtused kuvada mitu juhud seal on filtreeritud praeguse määramine. 

Selles näites on selgeks, mis on `Reports/Employees` taotleda 500 tõrgete enamik tulemusi:

![Laiendage atribuudi ja väärtuse valimist](./media/app-insights-diagnostic-search/04-failingReq.png)

Samuti kui soovite vt ka muid sündmusi, mis olid juhtub sel ajal, märkige ruut **kaasa sündmuste määratlemata atribuutidega**.

## <a name="remove-bot-and-web-test-traffic"></a>Robot ja web test liikluse eemaldamine

Filtri **reaal- või sünteetilisest liikluse** ja märkige ruut **Real**.

Samuti saate filtreerida **Allikas sünteetiliste liiklust**.

## <a name="inspect-individual-occurrences"></a>Üksikute sündmuste uurimine

Lisada, et taotluse nime filtri seadmine ja seejärel saab kontrollida üksikute esinemiskordade sündmusega.

![Valige väärtus](./media/app-insights-diagnostic-search/05-reqDetails.png)

Taotluse sündmuste, Kuva üksikasjad ilmnes taotluse töötlemise erandid.

Klõpsake erandi näha selle üksikasjad, sh virnas jälitus.

![Klõpsake erandi](./media/app-insights-diagnostic-search/06-callStack.png)

## <a name="find-events-with-the-same-property"></a>Sündmuste sama atribuudiga otsimine

Kõigi üksuste sama väärtusega atribuut leidmiseks tehke järgmist.

![Paremklõpsake atribuut](./media/app-insights-diagnostic-search/12-samevalue.png)

## <a name="search-by-metric-value"></a>Argumendil väärtuse järgi otsimine

Saada kutsed vastuse pidevalt > 5s.  Korda on esindatud puugid: 10 000 puugid = 1ms.

!["Vastuse aeg":(threshold TO *)](./media/app-insights-diagnostic-search/11-responsetime.png)



## <a name="search-the-data"></a>Andmete otsimine

Saate otsida terminite ühtki atribuudi väärtust. See on eriti kasulik, kui olete loonud [kohandatud sündmused] [ track] atribuudi väärtustega. 

Võite aja vahemiku otsingud lühemaks vahemikus on kiirem. 

![Avatud diagnostika otsing](./media/app-insights-diagnostic-search/appinsights-311search.png)

Otsige termineid, mitte kasutatavat võrdluslaadi. Termineid on tähtedest ja numbritest koosnev stringid, sealhulgas mõned kirjavahemärgid nagu "." ja "_". Näiteks:

Termini|*on suurem kui*|kuid need ei vasta
---|---|---
HomeController.About|kohta<br/>Avaleht|h\*kohta<br/>Avaleht\*
IsLocal|kohaliku<br/>on<br/>\*kohaliku|saared\*<br/>islocal<br/>i\*l\*
Uue viivitus|w d|uue<br/>viivitus<br/>n\* ja d\*


Siit leiate otsingu avaldiste abil saate:

Valimi päring | Efekti 
---|---
aeglane|Otsige üles kõik sündmused, mille väljad sisaldavad termin kuupäevavahemikku "aeglane"
andmebaasi?|Vastab database01, databaseAB...<br/>? pole lubatud otsingutermin alguses.
andmebaasi * |Vastab andmebaasi, database01, databaseNNNN<br/> * pole lubatud otsingutermin alguses
Apple'i ja banaan|Otsige sündmused, mis sisaldavad nii. Kasutage kapitali "ja" ei "ja".
Apple'i OR banaan<br/>Apple'i banaan|Otsige sündmusi, mis sisaldavad kas termin. Kasutage "Või" ei "või". < /br/ > lühike vorm.
Apple ei banaan<br/>õuna-banaan|Otsige sündmused, mis sisaldavad ühe termin, kuid mitte teiste.<br/>Lühike vorm.
rakenduse * ja banaan-(grape pear)|Loogika tehtemärgid ja kahvel.
"Meetermõõdustik": 0 kuni 500<br/>"Meetermõõdustik": 500 * | Otsige sündmused, mis sisaldavad nimega mõõtühikute väärtus vahemikus.


## <a name="save-your-search"></a>Otsingu salvestamine

Kui olete määranud kõik filtrid, mida soovite, saate salvestada selle otsingu lisamine lemmikute hulka. Kui töötate organisatsioonikonto, saate valida, kas selle ühiskasutusse teised meeskonna liikmed.

![Klõpsake nuppu lemmik, määrake nimi ja klõpsake nuppu Salvesta](./media/app-insights-diagnostic-search/08-favorite-save.png)


Otsingu uuesti, **minge ülevaade tera** näha ja avage lemmikud:

![Lemmikud paan](./media/app-insights-diagnostic-search/09-favorite-get.png)

Kui salvestasite koos suhteline ajavahemiku, uuesti avada tera on värskeimad andmed. Kui salvestasite koos absoluutne ajavahemiku, näete samad andmed iga kord.


## <a name="send-more-telemetry-to-application-insights"></a>Saada lisateavet telemeetria rakenduse ülevaated

Lisaks out-of-box telemeetria rakenduse ülevaateid SDK saadetud, saate teha järgmist.

* Jäädvustada jälgi Logi oma lemmik logimine [.NET] raamistiku[ netlogs] või [Java][javalogs]. See tähendab, et saate otsida jälitusandmete log ja need oleksid lehe vaateid, erandid ja muid sündmusi. 
* [Koodi kirjutamine] [ track] saata kohandatud sündmused, lehe vaadete ja erandid. 

[Siit saate teada, kuidas saata logid ja kohandatud telemeetria rakenduse ülevaated][trace].


## <a name="questions"></a>K & v

### <a name="limits"></a>Kui palju andmeid on alles?

Kuni 500 sündmuste sekundis iga rakendusest. Sündmused on alles seitse päeva.

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Kuidas vaadata postituse andmete minu server kutsed?

Me ei Logi postituse andmed automaatselt, kuid saate kasutada [TrackTrace või Logi kõned][trace]. Viige postituse andmed sõnumi parameeter. Ei saa filtreerida sõnumi saate atribuudid, kuid mahupiirangu on pikem.

## <a name="add"></a>Järgmised sammud

* [Logide ja kohandatud telemeetria saata rakenduse ülevaated][trace]
* [Kättesaadavus ja tundlikkuse kontrollib häälestamine][availability]
* [Tõrkeotsing][qna]



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[javalogs]: app-insights-java-trace-logs.md
[netlogs]: app-insights-asp-net-trace-logs.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
[trace]: app-insights-search-diagnostic-logs.md
[track]: app-insights-api-custom-events-metrics.md

 