<properties 
    pageTitle="Java web projekti rakenduse ülevaated tõrkeotsing" 
    description="Tõrkeotsingujuhendi - reaalajas Java rakenduste rakenduse ülevaated jälgimine." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Tõrkeotsing ja k ja v jaoks rakenduse ülevaated Java

Küsimused või probleemid [Visual Studio rakenduse ülevaated Java][java]? Siin on mõned näpunäited.


## <a name="build-errors"></a>Tõrked koostamine

*Eclipse, rakenduse ülevaateid SDK Maven või Gradle kaudu lisamisel, ma saan koostamine või kontrollsumma valideerimise tõrkeid.*

* Kui sõltuvus <version> element on kasutusel mustri metamärkidega (nt (Maven) `<version>[1.0,)</version>` või (Gradle) `version:'1.0.+'`), proovige määrata teatud versiooni hoopis nagu `1.0.2`. Vaadata, ning [vabastage märkmete](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) uusim versioon.

## <a name="no-data"></a>Andmed 

*Ma lisatud rakenduse ülevaated ja parandusfunktsiooni minu rakenduse, kuid ma pole kunagi näinud portaalis andmete.*

* Oota ja klõpsake nuppu Värskenda. Diagrammide värskendamine ise aeg-ajalt, kuid saate ka käsitsi värskendada. Värskendusintervalli sõltub ajavahemiku diagrammi.
* Kontrollige, kas teil on mõni instrumentation võti määratletud ApplicationInsights.xml faili (kaustas ressursid projektis)
* Veenduge, et pole `<DisableTelemetry>true</DisableTelemetry>` sõlm XML-failis.
* Tulemüüri, peate avage TCP-pordid 80 ja 443 dc.services.visualstudio.com väljamineva liikluse jaoks. Siit leiate [tulemüüri erandid täielik loend](app-insights-ip-addresses.md)
* Käivitage Microsoft Azure tahvli, Vaata teenuse olek kaarti. Kui on mõned teatiste viiteid, oodake, kuni ta naasevad OK ja seejärel sulgege ja avage see uuesti oma rakenduse ülevaated rakenduse tera.
* Lülitada sisse logimise IDE konsooli akna, lisades mõne `<SDKLogger />` elemendi juurkausta sõlme ApplicationInsights.xml faili (ressursid kausta projektis) ja kirjete sissejuhatava põhjendusega [tõrkega] kontrolli all.
* Veenduge, et õige ApplicationInsights.xml fail on edukalt laaditud Java SDK, vaadates konsooli väljundi sõnumite "konfiguratsioonifail on edukalt leitud" lause.
* Kui Otsingukonfiguratsiooni fail ei leitud, kontrollige väljundi sõnumite kuvamiseks, kust otsitakse config faili jaoks ja veenduge, et selle ApplicationInsights.xml asub neid Otsi asukohti. Nagu rusikareegel, saate lisada config faili rakenduse ülevaateid SDK purgid lähedal. Näide: Tomcat, tähendab see WEB-INF/teegi kausta.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Saab kasutada andmete kuvamiseks, kuid see on lõpetanud

* [Ajaveebi oleku](http://blogs.msdn.com/b/applicationinsights-status/)kontrollimine
* On klõpsamist kuu meiliteavitus andmepunktide? Avage sätted/kvoodi ja hinnakirjad välja selgitada. Sel juhul saate uuendada oma lepingut või täiendava läbilaskevõime eest maksta. Vt [hinnad värviskeemi](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Ma ei näe ma olen oodanud kõik andmed

* Avage kvootide ja hinnad blade ja kontrollige, kas kasutusel on [valimite](app-insights-sampling.md) . (100% edastamise tähendab, et toiming pole valimite.) Rakenduse ülevaated teenuse saate määrata ainult murdosa telemeetria, mis saabub teie rakenduse kinnitamiseks. See aitab hoida oma kuu piirmäära telemeetria sees. 

## <a name="no-usage-data"></a>Kasutus andmed

*Näen kohta taotlusi ja vastuse korda, aga pole leheküljevaade, brauseri või kasutaja andmeid.*

Saate edukalt häälestada rakenduse saata telemeetria serverist. Nüüd saate järgmiseks [veebilehtede saata telemeetria veebibrauseri kaudu]luua[usage].

Teine võimalus, kui teie klient on rakendus [telefoni või muud seadet][platforms], saate saata telemeetria seal. 

Sama instrumentation klahvi abil saate häälestada oma kliendi ja serveri telemeetria. Andmed kuvatakse sama rakenduse ülevaated ressurss ja jagamisega oleksid sündmused kliendi ja serveri kaudu.



## <a name="disabling-telemetry"></a>Telemeetria keelamine

*Kuidas keelata telemeetria saidikogumi?*

Kood:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Või** 

Värskendage ApplicationInsights.xml (kaustas ressursid projektis). Lisage jaotises juurkausta sõlm järgmine tekst:

    <DisableTelemetry>true</DisableTelemetry>

XML-i meetodi abil peate taaskäivitage rakendus, kui muudate väärtust.

## <a name="changing-the-target"></a>Sihtrühma muutmine

*Kuidas muuta mis Azure ressursi oma projekti saadab andmeid?*

* [Saada instrumentation võti uue ressursi.][java]
* Kui lisatud rakenduse ülevaated projekti Azure'i tööriistakomplekt kasutamise Eclipse, paremklõpsake web projekti, valige **Azure**, **Konfigureerida rakenduse ülevaated**, ja muuta võti.
* Muul juhul värskendage klahvi ApplicationInsights.xml projekti ressursid kausta.


## <a name="the-azure-start-screen"></a>Azure'i avakuva

*Otsin [Azure'i](https://portal.azure.com)portaal. Kas kaardi mulle midagi minu rakenduse kohta?*

Ei, see näitab seisundi Azure'i serverid kogu maailmas.

*: Azure'i start tahvli (avakuva), kuidas leida andmete minu rakenduse kohta?*

Eeldades, et saate [häälestada rakenduse jaoks rakenduse ülevaated][java], klõpsake nuppu Sirvi, valige rakenduse ülevaated ja valige oma rakenduse jaoks loodud rakenduse ressurss. Saada seal kiiremini tulevikus saate kinnitada rakenduse start tahvlile.

## <a name="intranet-servers"></a>Sisevõrk serverid

*Kas ma saan jälgida server minu sisevõrgus?*

Jah, kui teie server saata telemeetria rakenduse ülevaated portaali avaliku Interneti kaudu. 

Tulemüüri, peate avage TCP-pordid 80 ja 443 väljamineva liikluse dc.services.visualstudio.com ja f5.services.visualstudio.com.

## <a name="data-retention"></a>Andmete säilitamine 

*Kui kaua andmeid säilitatakse portaali? See on turvaline?*

Lugege teemat [andmete säilitamise ja privaatsuse][data].

## <a name="next-steps"></a>Järgmised sammud

*Häälestada rakenduse ülevaated rakendus Java server. Mida peaksin veel teha?*

* [Veebilehtede kättesaadavus jälgimine][availability]
* [Jälgida veebilehe kasutamine][usage]
* [Jälgida kasutust ja diagnoosida oma seadme rakenduste probleemid][platforms]
* [Jälgida kasutust rakenduse koodi kirjutamine][track]
* [Jäädvustada diagnostikalogid][javalogs]


## <a name="get-help"></a>Abi saamine

* [Virnlintdiagrammil ületäitumine](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 