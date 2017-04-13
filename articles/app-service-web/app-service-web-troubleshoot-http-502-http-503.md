<properties
    pageTitle="502 vale lüüs parandamine, 503 teenus pole saadaval tõrgete | Microsoft Azure'i"
    description="502 vale lüüs ja 503 teenus pole saadaval tõrgete oma veebirakenduse majutatud teenuses Azure rakenduse tõrkeotsing."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 vale lüüs, 503 teenus pole saadaval, tõrge 503, viga 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>"502 vale lüüs" ja "503 teenus pole saadaval" oma Azure veebirakendustes HTTP tõrkeotsing

"502 vale lüüs" ja "503 teenus pole saadaval" on levinud vigade teie majutatud teenuses [Azure rakenduse](http://go.microsoft.com/fwlink/?LinkId=529714)web Appis. See artikkel aitab teil tõrkeotsingut.

Kui vajate rohkem abi, mis tahes hetkel selle artikli, võite pöörduda [Azure'i MSDN-i](https://azure.microsoft.com/support/forums/)ja virnas ületäitumise Foorumid Azure eksperdid. Teise võimalusena saate faili Azure'i tugiteenuse taotluse. [Azure'i tugiteabe veebisait](https://azure.microsoft.com/support/options/) ja klõpsake **Toe saamiseks**.

## <a name="symptom"></a>Sümptom

Veebirakenduse sirvimisel, tagastab see HTTP "502 vale lüüs" tõrge või HTTP "503 teenus pole saadaval" tõrge.

## <a name="cause"></a>Põhjus

See probleem on sageli põhjustatud rakenduse taseme probleemid, näiteks:

-   taotlusi võtab kaua aega
-   rakenduses, valides kõrge mälu/CPU
-   rakenduse krahh erandi tõttu.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Tõrkeotsingu juhiste "502 vale lüüs" ja "503 teenus pole saadaval" tõrgete lahendamiseks

Tõrkeotsingu saab jagada kolme erineva tööülesande ajalises järjestuses:

1.  [Jälgida ja selle kasutust jälgida rakenduse käitumine](#observe)
2.  [Andmete kogumine](#collect)
3.  [Probleemi leevendada.](#mitigate)

[Rakenduse teenuse veebirakenduste](/services/app-service/web/) annab teile erinevaid võimalusi iga toimingu juures.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. jälgida ja selle kasutust jälgida rakenduse käitumine

####    <a name="track-service-health"></a>Teenuse seisundi jälgimine

Microsoft Azure'i publicizes iga kord, kui seal on teenuse katkemise või jõudluse degradeerumine. Saate jälgida teenuse seisundi [Azure portaali](https://portal.azure.com/). Lisateabe saamiseks vt [Jälita teenuse seisund](../monitoring-and-diagnostics/insights-service-health.md).

####    <a name="monitor-your-web-app"></a>Oma veebirakenduse jälgimine

See suvand võimaldab teil teada saada, kui teie taotlus on mis tahes probleeme. Klõpsake oma veebirakenduse labale **taotlusi ja tõrgete** paani. **Meetermõõdustik** tera kuvatakse kõik mõõdikute, saate lisada.

Mõned võiksite jälgida oma veebirakenduse mõõdikud on

-   Keskmine Mälu töökomplekt
-   Vastuse Keskmine kellaaeg
-   CPU aega
-   Mälu töökomplekt
-   Taotlused

![veebirakenduse 502 vale lüüs ja 503 teenus pole saadaval HTTP tõrgete lahendamise suunas jälgimine](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Lisateavet leiate teemast:

-   [Rakenduste jälgimine Web App Azure'i teenus](web-sites-monitor.md)
-   [Teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. andmete kogumine

####    <a name="use-the-azure-app-service-support-portal"></a>Azure'i rakendust Service tugi portaali kasutamine

Web Apps pakub teile võimalust oma veebirakenduse vaadates HTTP logid, sündmuselogide protsessi puistab ja muu seotud probleemide tõrkeotsing. Pääsete juurde kõik selle teabe abil meie tugiteenuste portaali aadressil **http://&lt;oma rakenduse nimi >.scm.azurewebsites.net/Support**

Portaali Azure'i rakendus toetab teenus pakub teile kolme eraldi vahekaarte toetab kolme juhiseid tõrkeotsingu stsenaarium:

1.  Jälgige praeguse käitumine
2.  Sisseehitatud analüsaatorid diagnostika teabe kogumine ja analüüsimine
3.  Leevendada

Kui parajasti toimub probleemi, valige **analüüsi** > **diagnostika** > **Diagnoosimine nüüd** diagnostika seansi loomiseks, mis kogub HTTP logib, vaataja sündmuselogide mälu puistab, PHP Tõrkelogide ja PHP töötlemine aruanne.

Kui andmed on kogunud, samuti analüüsi käivitamine andmeid ning teile HTML-i aruande.

Juhuks, kui soovite alla laadida andmed, vaikimisi, peaks olema talletatud D:\home\data\DaaS kausta;

Azure'i rakenduse teenuse tugi portaali kohta leiate lisateavet teemast [Uued värskendused tugi saidi laiend Azure veebilehed](/blog/new-updates-to-support-site-extension-for-azure-websites).

####    <a name="use-the-kudu-debug-console"></a>Kasutage Kudu konsooli silumine

Web Apps on konsooli silumine, mille abil saate silumine, uurida, üleslaadimise failid, samuti JSON lõpp-punktide jaoks keskkonna kohta teabe saamine. Seda nimetatakse _Kudu konsooli_ või _SCM armatuurlaua_ veebirakenduse jaoks.

Pääsete juurde selle armatuurlaua lahkumiseks linki **https://&lt;oma rakenduse nimi >.scm.azurewebsites.net/**.

Mõned asjad, mida pakub Kudu on:

-   keskkonna sätteid rakenduse
-   log voo
-   diagnostika dump
-   kus saate käivitada PowerShelli cmdlet-käskude ja põhilised käsud käsud konsooli silumine.


Mõne muu kasulik funktsioon Kudu on, et juhuks, kui teie taotlus on esimene võimalus erandid korrutamine, saate kasutada Kudu ja SysInternals tööriista Procdump loomiseks mälu puistab. Nende mälutõmmiste on protsessi hetktõmmiste ja sageli aitavad teil oma veebirakenduse keerulisem tõrkeotsingu.

Kudu funktsioonide kohta leiate lisateavet teemast [Azure veebisaitide võrgutööriistad peaks teadma](/blog/windows-azure-websites-online-tools-you-should-know-about/).

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

 ![taaskäivitage rakendus 502 vale lüüs ja 503 teenus pole saadaval HTTP tõrgete lahendamine](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Saate hallata oma veebirakenduse Azure PowerShelli kaudu. Lisateavet leiate teemast [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md).
