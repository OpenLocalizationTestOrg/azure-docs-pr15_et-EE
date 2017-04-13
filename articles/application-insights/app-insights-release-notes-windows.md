<properties 
    pageTitle="Windowsi rakenduse ülevaateid väljalaskemärkmed" 
    description="Windowsi poe SDK uusimad värskendused." 
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
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Väljalaskemärkmed – rakenduse ülevaated for Windows Phone SDK ja talletamine

Rakenduse ülevaateid SDK saadab [Rakenduse ülevaated](https://azure.microsoft.com/services/application-insights/), kus saate analüüsida kasutus- ja jõudluse telemeetria reaalajas rakenduse kohta.


## <a name="version-111"></a>Versioon 1.1.1

### <a name="windows-sdk"></a>Windows SDK

- Parandamine on hanguda ajal krahh Windows Phone Silverlighti SDK kasutamisel. Pärast selle muudatuse on mis tahes krahh, mis juhtub pärast kõne WindowsAppInitialier.InitializeAsync(...) ~ 2 sekundit hiljem sunnita abil kettale ja saadetakse järgmine kord, kui rakendus. Kui krahhi juhtub enne ~ 2 sekundi järel kõne, siis seda ignoreeritakse.  
- Seadke soovitud Nugeti sõltuvuste konkreetse versiooni põhi-ja Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Core SDK

- Core hallatakse github. Tulevaste väljalasete märkmete Core SDK leiate [github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Versiooni 1.1

### <a name="core-sdk"></a>Core SDK

- SDK nüüd tutvustab uusi telemeetria tüüp ```DependencyTelemetry``` mis sisaldab teavet sõltuvus kõne rakendusest
- Uue meetodi ```TelemetryClient.TrackDependency``` võimaldab saata teavet sõltuvus kõned rakendusest

## <a name="version-100"></a>Versioon 1.0.0

### <a name="windows-app-sdk"></a>Windowsi rakenduse SDK

- Uue lähtestamine Windows rakendused. Uue `WindowsAppInitializer` klassi koos `InitializeAsync()` meetod võimaldab eellaadimisel SDK saidikogumi lähtestamine. Selle muudatuse lubada täpsema kontrolli ja märkimisväärset rakenduse lähtestamise jõudlustäiustusi üle eelmise ApplicationInsights.config meetod.
- DeveloperMode seatakse enam automaatselt. DeveloperMode käitumise muutmiseks peate määrama kood.
- Nugeti pakett serveripoolse enam ApplicationInsights.config. Soovitame kasutada uut WindowsAppInitializer Nugeti pakett käsitsi lisamisel.
- Ainult loeb ApplicationInsights.config `<InstrumentationKey>`, ignoreeritakse kõiki muid sätteid eelistust WindowsAppInitializer sätted.
- Poe Market saab automaatselt kogutud SDK.
- Palju veaparandusi, stabiilsuse ja jõudluse täiustused.

### <a name="core-sdk"></a>Core SDK

- ApplicationInsights.config faili pole enam vajalikele. Ja pole lisatud Nugeti pakett. Konfiguratsiooni saate täielikult määratud koodi.
- Nugeti pakett lisa sihtkohtade faili enam teie lahendus. See eemaldab DeveloperMode automaatse sätte ajal silumine koostamine. Kood tuleks DeveloperMode käsitsi määrata.

## <a name="version-017"></a>Versiooni 0,17

### <a name="windows-app-sdk"></a>Windowsi rakenduse SDK

- Windowsi rakenduse SDK toetab nüüd Universal Windowsi rakendused Windows 10 tehniline eelvaade ja VS 2015 RC abil loodud.

### <a name="core-sdk"></a>Core SDK

- TelemetryClient vaikesätete lähtestamine abil soovitud InMemoryChannel.
- Uue API lisanud, `TelemetryClient.Flush()`. See meetod tühjendamise käivitab kohe blokeerimise üleslaadimine on sisse logitud kliendile kõik telemeetria. See võimaldab käsitsi käivitamise üleslaadimine enne protsessi sulgemine.
- Nugeti pakett lisatud .net 4.5 sihtkoht. See on väline sõltuvused (eemaldatud BCL ja EventSource sõltuvused).

## <a name="version-016"></a>Arvu 0,16 versioon 

2015-04-28 eelvaade

- SDK toetab nüüd DNX suunata platvorm lubamiseks [.NET Core Frameworki](http://www.dotnetfoundation.org/NETCore5) rakenduste jälgimine.
- Eksemplari ```TelemetryClient``` vahemälu Instrumentation klahvi enam. Nüüd, kui instrumentation klahvi ei olnud seadmine ```TelemetryClient``` konkreetselt ```InstrumentationKey``` tulemuseks null. See lahendab probleemi, kui seate ```TelemetryConfiguration.Active.InstrumentationKey``` pärast teatud telemeetria oli juba kogutud, telemeetria moodulid nagu sõltuvus koguja, web taotlused andmete kogumine ja jõudluse hinnale koguja kasutatakse instrumentation uue tootenumbri.

## <a name="version-015"></a>Versiooni 0,15

- Uue atribuudi ```Operation.SyntheticSource``` nüüd saadaval ```TelemetryContext```. Nüüd saate oma telemeetria üksuste märgi "mitte otse kasutaja liiklus" ja määrata, kuidas see liiklus loodi. Näide selle atribuudi järgi saate eristada liikluse laadi testi liiklus teie testi automatiseerimine.
- Kanali loogika teisaldati eraldi Nugeti, nimetatakse Microsoft.ApplicationInsights.PersistenceChannel. Vaikimisi on nüüdsest InMemoryChannel
- Uue meetodi ```TelemetryClient.Flush``` võimaldab tühjendage Alustuseks telemeetria üksuste sünkroonselt puhvri kaudu

## <a name="version-013"></a>Versiooni 0,13

Pole väljalaskemärkmed – vanemad versioonid on saadaval. 
