<properties 
    pageTitle="Andmed - rakenduse ülevaated .net-i tõrkeotsing" 
    description="Te ei näe Visual Studio rakenduse ülevaated andmeid? Proovige siin." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Andmed - rakenduse ülevaated .net-i tõrkeotsing

## <a name="some-of-my-telemetry-is-missing"></a>Minu telemeetria osa on puudu

*Rakenduse ülevaated, näen ainult murdosa sündmused, mis on loodud rakendus.*

* Kui kuvatakse sama murdosa pidevalt, tõenäoliselt kohandatava [valimite](app-insights-sampling.md)tõttu. Kontrollimaks, avage otsing (keelest ülevaade) ja vaadata eksemplari taotluse või mõne muu sündmuse. Jaotises Atribuudid allosas nuppu "..." täielik atribuudi üksikasjade saamiseks. Kui arv > 1 ja seejärel valimite on kasutusel. 
* Muul juhul on võimalik, et te tabab [andmete määr Piirake](app-insights-pricing.md#limits-summary) oma hinnakirjad lepingu raames. Need piirangud kehtivad minutis.

## <a name="no-data-from-my-server"></a>Minu server andmed

*Rakendus minu installis minu veebiserver ja mis tahes telemeetria seda ei kuvata. Töötas OK arendaja minu arvutis.*

* Tõenäoliselt tulemüüri küsimus. [Määrata tulemüüri erandid rakenduse ülevaated saata andmed](app-insights-ip-addresses.md).

*Ma [installitud oleku jälgimine](app-insights-monitor-performance-live-website-now.md) minu web serveris olemasoleva rakenduste jälgimine. Ma ei näe tulemusi.*

* Vaadake teemat [tõrkeotsing oleku jälgimine](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>Visual Studio võimalus lisada rakenduse ülevaated

*Uue projekti Visual Studios loomise või olemasoleva projekti Solution Exploreris paremklõpsamisel ei kuvata rakenduse ülevaated suvandid.*

+ Kõik tüübid .NET projekti ei toeta tööriistu. Veebi ja WCF-i projektide on toetatud. Muud projekti puhul, näiteks töölaua- või rakendused, saate siiski [lisada ka rakenduse ülevaateid SDK projekti käsitsi](app-insights-windows-desktop.md).
+ Veenduge, et teil on [Visual Studio 2013 värskenduse 3 või uuem versioon](http://go.microsoft.com/fwlink/?LinkId=397827). Tegemist tööriistad rakenduste ülevaated eelinstallitud.
+ Valige **Tööriistad**, **laiendid ja värskendused** ja veenduge, et **Tööriistad rakenduste ülevaated** on installitud ja lubatud. Kui jah, klõpsake **värskenduste** kuvamiseks, kui värskendus on saadaval.
+ Avage dialoogiboks uue projekti ja valige ASP.net-i veebirakenduse. Kui te ei näe rakenduse ülevaated olemas, installitakse tööriistu. Kui ei, proovige desinstallida ja seejärel uuesti installida tööriistad rakenduste ülevaated.


## <a name="q02"></a>Rakenduse ülevaated lisamine nurjus

*Uue web projekti loomisel või olemasoleva projekti rakenduse ülevaated lisamisel, näen tõrketeade.*

Tõenäolisemad põhjused:

* Rakenduse ülevaated portaali suhtlus nurjus; või
* On mõni probleem Azure'i konto;
* Teil on ainult [lugemisõigus tellimuse või rühma, kuhu te proovisite luua uue ressursi](app-insights-resources-roles-access-control.md).

Probleemi lahendamine:

+ Kontrollige sisselogimismandaati ettenähtud paremale Azure'i konto. 
+ Brauseris, kontrollige, et teil on juurdepääs [Azure'i portaalis](https://portal.azure.com). Avage sätted ja vaadake, kas on mingeid piiranguid.
+ [Olemasoleva projekti lisamine rakenduse ülevaated](app-insights-asp-net.md): lahenduste Explorer, paremklõpsake projekti ja valige käsk "Lisada rakenduse ülevaated."
+ Kui see ei tööta ikkagi, järgige [käsitsi toimingu](app-insights-windows-services.md) lisamiseks ressursi portaali ja seejärel lisage SDK projekti. 

## <a name="emptykey"></a>Kuvatakse tõrge "Instrumentation võti ei saa olla tühi"

Paistab, et midagi läks valesti, samas kui installisite rakenduse ülevaated või võib-olla logimine adapterit.

Paremklõpsake Solution Exploreris `ApplicationInsights.config` ja valige **Rakenduse ülevaated konfigureerimine**. Kuvatakse dialoogiboks, mis palub teil sisse logida Azure ja kas loomine rakenduse ülevaated on ressurss, või kasutada mõne olemasoleva.


##<a name="NuGetBuild"></a>"Nugeti pakett (s) puuduvad" minu serveris koostamine

*Kõik koostab OK, kui ma olen silumine minu arvutis arengu, kuid tõrge Nugeti serveris koostamine.*

Palun vaadake [Nugeti pakett taastamine](http://docs.nuget.org/Consume/Package-Restore) ja [Automaatse paketi taastada](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Puuduvate menüükäsk Visual Studio rakenduse ülevaated avamine

*Minu projekti Solution Exploreris paremklõpsamisel ei kuvata rakenduse ülevaated sortimiskäskude või ei kuvata, avage rakenduse ülevaated käsu käivitava.*

Tõenäolisemad põhjused:

* Kui lõite rakenduse ülevaated ressursi käsitsi või kui projekt on tüüp, mis ei toeta rakenduse ülevaated tööriistu.
* Rakenduse ülevaated tööriistad on keelatud oma Visual Studios.
* Teie Visual Studio on vanemad kui 2013 Update 3.

Probleemi lahendamine:

* Veenduge, et teie Visual Studio versioon on 2013 värskenduse 3 või uuem versioon.
* Valige **Tööriistad**, **laiendid ja värskendused** ja veenduge, et **Tööriistad rakenduste ülevaated** on installitud ja lubatud. Kui jah, klõpsake **värskenduste** kuvamiseks, kui värskendus on saadaval.
* Paremklõpsake Solution Exploreris projekti. Kui näete käsu **Konfigureerimine rakenduse ülevaated**, kasutage rakenduse ülevaated service ressursi projekti loomiseks.


Muul juhul oma projekti tüüp otse ei toeta rakenduse ülevaated tööriistu. Kui soovite vaadata oma telemeetria [Azure portaali](https://portal.azure.com)sisse logida, valige vasakul asuvalt navigeerimisribalt rakenduse ülevaated ja valige rakenduse.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"Juurdepääs keelatud" avamisel rakenduse ülevaated Visual Studio

*Käsu 'Ava rakenduse ülevaated' võtab mind Azure portaali, kuid kuvatakse tõrge "juurdepääs keelatud".*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Funktsiooni Microsoft sisselogimist viimati kasutatud vaikebrauseriks pole juurdepääsu [ressursile, mis on loodud, kui see rakendus on lisatud rakenduse ülevaated](app-insights-asp-net.md). Kahe tõenäolisemad põhjused, miks on: 

* Teil on rohkem kui ühe Microsofti kontoga – võib-olla töö ja isikliku Microsofti kontoga? Sisselogimine viimati kasutatud vaikebrauseriks oli mõne muu kontoga, kui see, kellel on juurdepääs [Rakenduse ülevaated projekti lisada](app-insights-asp-net.md). 

 * Probleemi lahendamine: Klõpsake oma nime brauseriakna paremas ülanurgas- ja väljalogimine. Logige sisse kontoga, millel on juurdepääs. Klõpsake vasakpoolsel navigeerimisribal klõpsake rakenduse ülevaated ja valige oma rakenduse.

* Kellegi poolt lisatud projekt rakenduse ülevaated ja need unustanud [juurdepääsu ressursirühma](app-insights-resources-roles-access-control.md) on loodud. 

 * Probleemi lahendamine: Kui neid kasutada organisatsioonikonto, nad saate lisada teie meeskond; või ta saab teile andma üksikute juurdepääsu ressursirühma.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>"Varade ei leitud" Visual Studio rakenduse ülevaated avamine

*Käsu 'Ava rakenduse ülevaated' võtab mind Azure portaali, kuid kuvatakse tõrge "varade ei leitud".*

Tõenäolisemad põhjused:

* Rakenduse ülevaated ressursi rakenduse on kustutatud; või
* Haldusteenuse võti oli seadmine või muudetud ApplicationInsights.config, redigeerides see otse värskendamata projekti faili. 

Sisestage instrumentation ApplicationInsights.config juhtelemendid, kus telemeetria saadetakse. Projekti faili joone juhtelemendid, mis ressursi avatakse selle käsu kasutamisel Visual Studios. 

Probleemi lahendamine:

* Solution Exploreris projekti paremklõpsake ja valige rakenduse ülevaated rakenduse ülevaated konfigureerimine. Dialoogiboksis, kas saate saata telemeetria olemasolevate ressursside või looge uus. Või:
* Avage otse ressurss. [Azure portaali](https://portal.azure.com)sisse logida, klõpsake rakenduse ülevaated vasakpoolsel navigeerimisribal ja seejärel valige oma rakenduse.



## <a name="where-do-i-find-my-telemetry"></a>Kust leian oma telemeetria?

*Minu [Microsoft Azure'i portaali](https://portal.azure.com)sisse logitud ja otsin Azure home armatuurlaud. Kust ma leida minu rakenduse ülevaated andmeid?*

* Klõpsake vasakpoolsel navigeerimisribal rakenduse ülevaated ja seejärel oma rakenduse nimi. Kui teil pole olemas projektide, peate [lisada või konfigureerida rakenduse ülevaated web projekti](app-insights-asp-net.md).

    Kuvatakse mõned kokkuvõttediagrammid. Saate klõpsata nende täpsemaks vaatamiseks kaudu.

* Visual Studio, kui olete silumine rakenduse, nuppu rakenduse ülevaated.


## <a name="q03"></a>Serveri andmed (või pole üldse andmeid)

*Ma käivitasite rakendus ja seejärel avada rakenduse ülevaated teenuse Microsoft Azure, kuid kõik diagrammid kuvada "Saate teada, kuidas koguda..." või "Pole konfigureeritud."* Või, *ainult lehe vaatamine ja kasutajale andmeid, kuid serveri andmeid.*

+ Käivitage rakenduse silumine režiimis Visual Studios (F5). Kasutage nii, et luua mõne telemeetria. Kontrollige, et näete sündmuste Visual Studio väljundi aknas sisse logitud. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Avage rakenduse ülevaated portaalis [Diagnostika otsing](app-insights-diagnostic-search.md). Andmed kuvatakse enamasti siin esmalt.
+ Klõpsake nuppu Värskenda. Tera värskendab ise aeg-ajalt, kuid saate teha seda käsitsi. Värskendusintervalli on pikem suuremat kellaaja vahemike jaoks.
+ Märkige vastavad instrumentation võtmed. Peamised enne oma rakenduse rakenduse ülevaated portaalis **Essentialsi** ripploendis vaadake **Instrumentation võti**. Projekti Visual Studios, avage ApplicationInsights.config seejärel otsige soovitud `<instrumentationkey>`. Kontrollige, kas kaks võtit on võrdsed. Muul juhul:
 + Portaalis, klõpsake rakenduse ülevaated ja otsige rakenduse ressursside õige võti; või
 + Visual Studio lahenduste Explorer, paremklõpsake projekti ja valige rakenduse ülevaated konfigureerimine. Lähtesta rakenduse telemeetria saatmiseks õige ressursi.
 + Kui te ei leia vastavat klahvid, kontrollige, et kasutate samasse mandaat Visual Studios, nagu portaali.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ [Microsoft Azure'i home armatuurlaua](https://portal.azure.com)Vaata kaarti teenuse seisund. Kui on mõned teatiste viiteid, oodake, kuni ta naasevad OK ja seejärel sulgege ja avage see uuesti oma rakenduse ülevaated rakenduse tera.
+ Kontrollige, kas [meie oleku ajaveebi](http://blogs.msdn.com/b/applicationinsights-status/).
+ Sa kirjutada koodi [serveripoolne SDK](app-insights-api-custom-events-metrics.md) instrumentation klahvi muutuda võivate `TelemetryClient` eksemplarid või `TelemetryContext`? Või sa kirjutada [filter või valimite konfiguratsiooni](app-insights-api-filtering-sampling.md) , mis võib filtreerimine välja liiga palju?
+ Kui saate redigeerida ApplicationInsights.config, hoolega konfiguratsiooni [TelemetryInitializers ja TelemetryProcessors](app-insights-api-filtering-sampling.md). Parameetri või valesti nimega tüüp, võivad põhjustada SDK saata andmed.

## <a name="q04"></a>Lehe vaateid, brauserites, kasutus andmed

*Lehe Kuva laadimise ajal või brauseris või kasutus labad näen andmete serveri vastuse aja-ja serveri kutsed, kuid andmeid.*

Andmed on pärit veebilehtedel skriptide. 

+ Kui lisasite rakenduse ülevaated web projekti, [peate lisama skriptide käsitsi](app-insights-javascript.md).
+ Veenduge, et Internet Explorer ei ole saidile kuvatud ühilduvusrežiimis.
+ Veenduge, et andmed saadetakse brauseri silumine funktsioon (F12 mõnes brauseris, siis valige võrgu) abil `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Sõltuvus või erandi andmed

Lugege teemat [sõltuvus telemeetria](app-insights-asp-net-dependencies.md) ja [telemeetria erand](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Jõudluse andmed

Jõudlusandmeid (CPU, IO määr jne) on saadaval [Java veebiteenused](app-insights-java-collectd.md), [Windowsi töölaua rakendused](app-insights-windows-desktop.md), [IIS web rakendused ja teenused, kui installite oleku jälgimine](app-insights-monitor-performance-live-website-now.md)ja [Azure pilveteenustega](app-insights-azure.md). leiate jaotises sätted serverites.

Pole saadaval Azure veebisaitide.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Kuna rakendus avaldada oma serveri andmed (server)

+ Märkige kõigi Microsofti tegelikult kopeeritud. Server, koos Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll ApplicationInsights dll-teekidega
+ Tulemüüri, peate [mõne TCP-pordid](app-insights-ip-addresses.md#data-access-api)avamiseks.
+ Kui teil on puhverserveri kasutamine saata välja teie ettevõtte võrguga, Web.config [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) määramine
+ Windows Server 2008: Veenduge, et olete installinud järgmised värskendused: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Saab kasutada andmete kuvamiseks, kuid see on lõpetanud

* [Ajaveebi oleku](http://blogs.msdn.com/b/applicationinsights-status/)kontrollimine
* On klõpsamist kuu meiliteavitus andmepunktide? Avage sätted/kvoodi ja hinnakirjad välja selgitada. Sel juhul saate uuendada oma lepingut või täiendava läbilaskevõime eest maksta. Vt [hinnad värviskeemi](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>Ma ei näe ma olen oodanud kõik andmed

Kui teie taotlus saadetakse palju andmeid ja kasutate rakenduse ülevaateid SDK ASP.net-i versioon 2.0.0-beta3 või uuem versioon, [kohandatava valimite](app-insights-sampling.md) funktsiooni käitamiseks ja saatmine ainult teie telemeetria protsent. 

Saate selle keelata, kuid see pole soovitatav. Valimite on kujundatud nii, et seotud telemeetria õigesti edastada, ajutised. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Vale geograafiliste andmete kasutaja telemeetria

Linn, piirkond ja riik mõõtmed on tuletatud IP-aadressid ja ei kasuta alati täpne.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Erandi "meetod ei leitud" klõpsake Azure pilveteenustega töötab?

Kas koostate .NET 4.6? Azure'i pilveteenustega rolliga 4.6 automaatselt ei toetata. Enne käivitamist rakenduse [installimine 4.6 iga rolli kohta](../cloud-services/cloud-services-dotnet-install-dotnet.md) .

## <a name="still-not-working"></a>Kas ikka ei tööta...

* [Rakenduse ülevaateid Foorum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

