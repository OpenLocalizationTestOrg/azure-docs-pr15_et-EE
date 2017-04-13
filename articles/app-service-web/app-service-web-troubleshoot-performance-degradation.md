<properties
    pageTitle="Aeglustada web app rakenduse teenus | Microsoft Azure'i"
    description="See artikkel aitab teil aeglane web appi jõudlusprobleemide Azure'i rakendust Service tõrkeotsing."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="Web Appi jõudluse tagamiseks aeglane rakenduse rakenduse aeglane"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Azure'i rakendust Service aeglane web appi jõudlusprobleemide tõrkeotsing

See artikkel aitab teil aeglane web appi jõudlusprobleemide [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714)tõrkeotsing.

Kui vajate rohkem abi, mis tahes hetkel selle artikli, võite pöörduda [Azure'i MSDN-i](https://azure.microsoft.com/support/forums/)ja virnas ületäitumise Foorumid Azure eksperdid. Teise võimalusena saate faili Azure'i tugiteenuse taotluse. [Azure'i tugiteabe veebisait](https://azure.microsoft.com/support/options/) ja klõpsake **Toe saamiseks**.

## <a name="symptom"></a>Sümptom

Sirvimisel veebirakenduse, lehtede laadimise aeglaselt ja mõnikord ajalõpp.

## <a name="cause"></a>Põhjus

See probleem on sageli põhjustatud rakenduse taseme probleemid, näiteks:

-   taotlusi võtab kaua aega
-   rakenduses, valides kõrge mälu/CPU
-   rakenduse krahh erandi tõttu.

## <a name="troubleshooting-steps"></a>Tõrkeotsingu juhiste

Tõrkeotsingu saab jagada kolme erineva tööülesande ajalises järjestuses:

1.  [Jälgida ja selle kasutust jälgida rakenduse käitumine](#observe)
2.  [Andmete kogumine](#collect)
3.  [Probleemi leevendada.](#mitigate)

[Rakenduse teenuse veebirakenduste](/services/app-service/web/) annab teile erinevaid võimalusi iga toimingu juures.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. jälgida ja selle kasutust jälgida rakenduse käitumine

#### <a name="track-service-health"></a>Teenuse seisundi jälgimine

Microsoft Azure'i publicizes iga kord, kui seal on teenuse katkemise või jõudluse degradeerumine. Saate jälgida teenuse seisundi [Azure portaali](https://portal.azure.com/). Lisateabe saamiseks vt [Jälita teenuse seisund](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Oma veebirakenduse jälgimine

See suvand võimaldab teil teada saada, kui teie taotlus on mis tahes probleeme. Klõpsake oma veebirakenduse labale **taotlusi ja tõrgete** paani. **Meetermõõdustik** tera kuvatakse kõik mõõdikute, saate lisada.

Mõned võiksite jälgida oma veebirakenduse mõõdikud on

-   Keskmine Mälu töökomplekt
-   Vastuse Keskmine kellaaeg
-   CPU aega
-   Mälu töökomplekt
-   Taotlused

![web appi jõudluse jälgimist](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Lisateavet leiate teemast:

-   [Rakenduste jälgimine Web App Azure'i teenus](web-sites-monitor.md)
-   [Teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web lõpp-punkti oleku jälgimine

Kui kasutate oma web appi **Standard** hinnad taseme, veebirakenduste võimaldab teil jälgida 2 lõpp-punktid: 3 geograafilised asukohad.

Lõpp-punkti jälgimise konfigureerib web testide aega ja sees web URL-ide geo jaotatud asukohtadest. Test teostab web URL-i määratlemiseks sees ja vastuse kellaaja iga asukohast HTTP GET toimingu. Iga konfigureeritud asukoha käivitatakse test iga viie minuti järel.

Sees on jälgida HTTP vastuse koode ja mõõtmise vastuse aeg millisekundites. Jälgimise test nurjub, kui HTTP vastuse koodi on suurem kui või võrdne 400 või vastuse võtab rohkem kui 30 sekundit. Lõpp-punkti käsitletakse saadaval juhul, kui selle jälgimise kontrollib õnnestub, alates määratud asukohtadesse.

Häälestada, lugege teemat [kohta: web lõpp-punkti oleku jälgimine](web-sites-monitor.md#webendpointstatus).

Vaadake [säilitamise Azure veebisaitide üles pluss lõpp-punkti jälgimine - Stefan Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) video lõpp-punkti jälgimine.

#### <a name="application-performance-monitoring-using-extensions"></a>Rakenduse jõudluse jälgimise laiendid

Rakenduse jõudluse jälgida kasutamine _saidi laiendid_.

Iga rakenduse teenuse web appi pakub laiendatav halduse lõpp-punkt, mis võimaldab teil ära kasutada võimsaid tööriistu, näiteks saidi laiendid. Nende tööriistade ulatuvad source code redaktorid nagu [Visual Studio meeskonnatöö teenuseid](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) Haldusriistad ühendatud ressursid, nt MySQL-andmebaasiga ühendatud web appi.

[Azure'i rakenduse ülevaated](/services/application-insights/) ja [Uus reliikvia](/marketplace/partners/newrelic/newrelic/) on kaks jõudluse kontrollimise saidi laiendid, mis on saadaval. Uue reliikvia kasutamiseks installimist agent käitusajal. Azure'i rakenduse ülevaated kasutamiseks saate taastada ka SDK kood ning saate installida laiendamine, mis pakub täiendavaid andmetele juurdepääsu. SDK võimaldab teil jälgida kasutus ja jõudluse rakenduse üksikasjalikumalt koodi kirjutamine.

Rakenduse ülevaated kasutamiseks lugege teemat [kuvari jõudluse veebirakendusi](../application-insights/app-insights-web-monitor-performance.md).

Uue reliikvia kasutamiseks lugege teemat [Uue reliikvia jõudluse Rakendusehaldus Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. andmete kogumine

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Diagnostika logimise veebirakenduse jaoks

Veebirakenduste keskkonna pakub diagnostika funktsioonid logiteabe veebiserver nii veebirakenduse. Need on loogiliselt jagatud web serveri diagnostika- ja rakenduse diagnostika.

##### <a name="web-server-diagnostics"></a>Web serveri diagnostika

Saate lubada või keelata järgmist tüüpi logid:

-   **Tõrke üksikasjalik logimine** - tõrke üksikasjalik teave HTTP olek koode, mis tähistavad tõrke (olekukoodi 400 või suurem). See võib sisaldada teavet, mis aitab kindlaks teha, miks server tagastas tõrkekoodi.
-   **Nurjus taotluse jälitamine** - üksikasjalikku teavet nurjunud taotlusi, sh IIS-i komponendid kasutada taotluse ja iga komponendi kulunud aja jälgi. See võib olla kasulik, kui proovite web appi jõudluse parandamiseks või eraldada, mis võib põhjustada teatud HTTP-tõrge.
-   **Web serveri logimine** - teavet HTTP kannete W3C laiendatud log-vormingus. See on kasulik, kui määratlemine üldine web appi mõõdikute, nt töödeldud taotluste või mitu taotlused teatud IP-aadress on arv.

##### <a name="application-diagnostics"></a>Rakenduse diagnostika

Rakenduse diagnostika abil saate jäädvustada veebirakenduse andmed. ASP.net-i rakendusi saate kasutada funktsiooni `System.Diagnostics.Trace` klassi logige teavet rakenduse diagnostika Logi.

Üksikasjalikud juhised selle kohta, kuidas konfigureerida rakenduse logimiseks, lugege teemat [diagnostika logimine veebirakenduste teenuses Azure rakenduse lubamine](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Remote profiilide kasutamine

Azure'i rakenduse teenus, veebirakenduste, API rakendused ja WebJobs saate kaugühenduse teel profiil. Kui teie protsess töötab aeglasem kui oodatud, või HTTP päringuid latentsus on suurem kui tavaline ja CPU protsessi on ka kõrge, saate kaugühenduse teel teie protsessi profiil ja saada CPU valimite kõne virnadena protsessi tegevuste ja koodi kuum teed analüüsimiseks.

Kohta leiate lisateavet teemast [Azure'i rakendust Service tugi Remote profiili](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Azure'i rakendust Service tugi portaali kasutamine

Web Apps pakub teile võimalust oma veebirakenduse vaadates HTTP logid, sündmuselogide protsessi puistab ja muu seotud probleemide tõrkeotsing. Pääsete juurde kõik selle teabe abil meie tugiteenuste portaali aadressil **http://&lt;oma rakenduse nimi >.scm.azurewebsites.net/Support**

Portaali Azure'i rakendus toetab teenus pakub teile kolme eraldi vahekaarte toetab kolme juhiseid tõrkeotsingu stsenaarium:

1.  Jälgige praeguse käitumine
2.  Sisseehitatud analüsaatorid diagnostika teabe kogumine ja analüüsimine
3.  Leevendada

Kui parajasti toimub probleemi, valige **analüüsi** > **diagnostika** > **Diagnoosimine nüüd** diagnostika seansi loomiseks, mis kogub HTTP logib, vaataja sündmuselogide mälu puistab, PHP Tõrkelogide ja PHP töötlemine aruanne.

Kui andmed on kogunud, samuti analüüsi käivitamine andmeid ning teile HTML-i aruande.

Juhuks, kui soovite alla laadida andmed, vaikimisi, peaks olema talletatud D:\home\data\DaaS kausta;

Azure'i rakenduse teenuse tugi portaali kohta leiate lisateavet teemast [Uued värskendused tugi saidi laiend Azure veebilehed](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Kasutage Kudu konsooli silumine

Web Apps on konsooli silumine, mille abil saate silumine, uurida, üleslaadimise failid, samuti JSON lõpp-punktide jaoks keskkonna kohta teabe saamine. Seda nimetatakse _Kudu konsooli_ või _SCM armatuurlaua_ veebirakenduse jaoks.

Pääsete juurde selle armatuurlaua lahkumiseks linki **https://&lt;oma rakenduse nimi >.scm.azurewebsites.net/**.

Mõned asjad, mida pakub Kudu on:

-   keskkonna sätteid rakenduse
-   log voo
-   diagnostika dump
-   kus saate käivitada PowerShelli cmdlet-käskude ja põhilised käsud käsud konsooli silumine.


Mõne muu kasulik funktsioon Kudu on, et juhuks, kui teie taotlus on esimene võimalus erandid korrutamine, saate kasutada Kudu ja SysInternals tööriista Procdump loomiseks mälu puistab. Nende mälutõmmiste on protsessi hetktõmmiste ja sageli aitavad teil oma veebirakenduse keerulisem tõrkeotsingu.

Kudu funktsioonide kohta leiate lisateavet teemast [Azure veebisaitide meeskonnatöö teenuste tööriistad peaks teadma](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. probleemi leevendada.

####    <a name="scale-the-web-app"></a>Veebirakenduse skaala

Azure'i rakendust Service parema jõudluse ja läbilaskevõime, saate reguleerida skaala, kus te kasutate rakenduse. Web appi ülespoole kahe seotud toiminguid hõlmab: muuta oma rakenduse teenusleping hinnakirjad jõudmine ja pärast seda, kui olete läinud hinnakirjad kõrgema taseme teatud sätete konfigureerimine.

Skaleerimist kohta leiate lisateavet teemast [skaala web appi teenuses Azure rakendus](web-sites-scale.md).

Lisaks saate rakenduse käivitada mitu eksemplari. See mitte ainult annab teile rohkem töötlemise võimalus, kuid annab teile tõrketaluvust teatud summa. Kui protsess läheb ühe eksemplari, teises eksemplaris jätkub serveeritakse taotlused.

Saate seada skaleerimist käsitsi või automaatne.

####    <a name="use-autoheal"></a>Kasutage AutoHeal

AutoHeal ringlusse töötaja protsessi oma rakenduse põhinevad sätted saate valida (nt konfiguratsioonimuudatuste, taotlusi, mälu põhinev piirangud või taotluse vajalik aeg). Enamasti, Prügikast protsess on kiireim viis soovitud probleemi lahendamiseks. Kuigi alati taaskäivitamist veebirakenduse otse Azure portaali, AutoHeal teevad selle automaatselt teie eest. Kõik, mida on vaja teha on lisada mõne päästikute juurkausta Web.config veebirakenduse jaoks. Pange tähele, et neid sätteid töötaks samamoodi ka siis, kui teie rakendus pole .net-i, üks.

Lisateavet leiate teemast [Automaatne probleemilahendus Azure veebisaitide](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Taaskäivitage web app

See on sageli lihtsaim viis taastada ühekordse probleemid. [Azure'i portaalis](https://portal.azure.com/)oma veebirakenduse tera, teil on võimalused peatamiseks või taaskäivitage rakendus.

 ![taaskäivitage veebirakenduse jõudlusega seotud probleemide lahendamiseks](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Saate hallata oma veebirakenduse Azure PowerShelli kaudu. Lisateavet leiate teemast [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md).
