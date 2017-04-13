<properties
    pageTitle="Lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine"
    description="Saate teada, kuidas kasutada etapiviisilise avaldamine teenuses Azure rakenduse web apps."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine
<a name="Overview"></a>

Kui juurutate oma veebirakenduse [Teenuse rakendus](http://go.microsoft.com/fwlink/?LinkId=529714), saate juurutada eraldi juurutamine pesa asemel vaikimisi tootmise pesa kui režiimis **Standard** - või **rakenduse Premiumi** leping. Juurutamine on tegelikult reaalajas veebirakenduste abil oma hostinimed. Web Appi sisu ja konfiguratsioone elemendid saate vahetasin juurutamise pesast, sh tootmise pesa vahel. Rakenduse juurutamine pesa juurutamine on järgmised eelised:

- Saate kinnitada web appi muudatuste juurutamise vahekausta pesa enne selle tootmise pesa vahetamine.

- Teenuse juurutamisel web appi pesa esmalt ja vahetamine selle tootmisse tagab, et kõik eksemplarid pesa soojendada enne on vahetasin tootmisse. See kaob tööseisakute kui juurutate oma veebirakenduse. Liikluse ümbersuunamine on sujuvalt ja pole taotlused kõrvaldatakse Vaheta tegevuse tõttu. Kogu töövoo automatiseerida konfigureerimisega [Automaatne Vaheta](#configure-auto-swap-for-your-web-app) kui eelse Vaheta valideerimine pole vaja.

- Pärast soovitud Vaheta pesa varem etapiviisilise web app on nüüd eelmise tootmise web appi. Kui vahetasin tootmise pesa muudatused on pole nii, nagu soovite, saate teha sama vaheta kohe, et saada "viimase teadaolevad hea saidile" tagasi.

Iga rakenduse teenuse leping režiimi toetab juurutamine erinevat hulka. Välja selgitada arvu teenindusaegade web appi režiimi toetab, leiate teemast [Rakenduse teenuse hinnad](/pricing/details/app-service/).

- Kui teie web Appis on mitu teenindusaegade, ei saa muuta režiimi.

- Skaleerimist pole saadaval, mitte tekitamiseks Slots.

- Lingitud ressursihalduse mitte tekitamiseks slots ei toetata. [Azure portaali](http://go.microsoft.com/fwlink/?LinkId=529715) ainult, saate vältida selle võib mõjutada tootmise pesa ajutiselt teisaldada mitte tekitamiseks pesa rakenduse teenuse leping režiimi. Pange tähele, et mitte tekitamiseks pesa peab taas jagada sama režiimi tootmise pesa enne saate vahetada kahe slots.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Juurutamise pesa lisada web app ##

Veebirakenduse peab töötama selleks, et aktiveerida mitut juurutamise slots **Standard** - või **Premium** režiimis.

1. [Azure portaali](https://portal.azure.com/), avage oma veebirakenduse tera.
2. Klõpsake nuppu **sätted**ja seejärel nuppu **juurutamine**. Seejärel klõpsake labale **juurutamine** **Pesa lisada**.

    ![Lisage uus juurutamine pesa][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Kui web app ei ole juba **Standard** või **Premium** režiimis, saate toetatud režiimi, mis võimaldab etapiviisilise avaldamise teade. Sel hetkel, on teil võimalus valida **täiendamine** ja liikuge oma veebirakenduse vahekaarti **skaala** enne jätkamist.

2. **Lisa pesa** labale pesa pange nimi ja valige, kas klooni web appi konfigureerimine teise olemasoleva juurutamise pesa kaudu. Klõpsake märkeruutu jätkata.

    ![Andmeallika konfiguratsiooni][ConfigurationSource1]

    Esimest korda pesa lisamist, siis ainult on kaks võimalust: klooni konfiguratsiooni vaikimisi pesa valmistamisel või üldse mitte.

    Kui olete loonud mitu teenindusaegade, on võimalik klooni pesa muus valmistamisel konfiguratsioon:

    ![Konfiguratsiooni allikad][MultipleConfigurationSources]

5. Klõpsake **juurutamine** labale juurutamise pesa avamiseks höövlitera pesa, mõõdikute kogum nagu web appi konfigureerimine. **Your-Web-App-Name-Deployment-slot-Name** kuvatakse blade meelde, et vaatate juurutamine pesa ülaosas.

    ![Juurutamise pesa tiitel][StagingTitle]

5. Klõpsake soovitud pesa blade rakenduse URL. Pange tähele, et juurutamine pesa oma hostname (hostinimi) on ja on ka reaalajas rakendus. Juurutamise pesa juurdepääsu piiramiseks leiate [Rakenduse teenuse Web App – Blokeeri web juurdepääsu mitte tekitamiseks juurutamine](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

On sisu pole pärast juurutamise pesa loomine. Saate juurutada pesa erinevate hoidla haru või mõne hoopis muud hoidla. Samuti saate muuta selle pesa konfigureerimine. Avalda profiili või juurutuse andmetega seotud juurutamine pesa sisu värskendusi kasutada.  Näiteks saate [selle pesa git avaldada](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Konfiguratsiooni juurutamine ##
Kui te klooni teise juurutamise pesa konfiguratsioon, kloonitud konfiguratsiooni saab redigeerida. Lisaks jooni konfiguratsiooni jälgib sisu üle mõne vahetamine (ei ava teatud) ajal konfigureerimine muude elementide jäävad sama pesa pärast lisamine vahetamine (Ava teatud). Järgmises loendis Kuva konfiguratsiooni, mis muutub, kui saate vahetada slots.

**Mis on vahetasin sätted**:

- Üldsätted – nt framework versiooni 32/64-bitine Web sockets
- Rakenduse sätete (saab konfigureerida jääda pesa)
- Ühendusstringi (saab konfigureerida jääda pesa)
- Sündmuseohjuri vastendused
- Järelevalve ja sätted
- WebJobs sisu

**Mis on vahetasin sätted**:

- Avaldamise lõpp-punktid
- Kohandatud domeeni nimed
- SSL-sertide ja seosed
- Skaala sätteid
- WebJobs skeduloijat

Mõni rakendus säte või ühenduse string jääda pesa (mitte vahetasin), juurdepääsu teatud pesa tera **Rakenduse sätted** ja seejärel valige konfiguratsiooni elemente, mis peaks jääma pesa **Pesa säte** ruut konfigureerida. Pange tähele, et selle märkimise konfiguratsiooni elemendi pesa teatud on mõju, millega see element pole kiirvahetuse üle kõik selle juurutamise seotud web appi.

![Pesa sätted][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Vahetada juurutamine ##

>[AZURE.IMPORTANT] Saate vahetada web appi kaudu juurutamine pesa tootmisse, veenduge, et kõik-pesa teatud sätted on konfigureeritud täpselt nii, nagu soovite seda soovitavas Vaheta.

1. Vaheta juurutamine, klõpsake nuppu **Vaheta** käsuriba veebirakenduse või juurutamine pesa käsuriba. Veenduge, et Vaheta lähte-kui ka Vaheta target õigesti seatud. Tavaliselt oleks Vaheta target tootmise pesa.  

    ![Vaheta nupp][SwapButtonBar]

3. Klõpsake nuppu **OK** , toimingu lõpuleviimiseks. Pärast selle toimingu lõppemist on vahetatud juurutamise slots.

## <a name="configure-auto-swap-for-your-web-app"></a>Vaheta automaatne konfigureerimine veebirakenduse jaoks ##

Automaatne vahetamine sujuvamaks DevOps stsenaariumid, kus soovite pidevalt juurutada oma veebirakenduse null külmkäivitusel ja null tööseisakute end klientide web appi. Kui juurutamise pesa on konfigureeritud lubama automaatne vahetamine tootmisesse iga kord, kui vajutada värskendamise kood selle pesa, Vaheta rakenduse teenuse automaatselt tootmisse web appi, see on juba soojenenud pesa.

>[AZURE.IMPORTANT] Kui lubate automaatse Vaheta pesa, veenduge, et pesa konfiguratsioon on täpselt konfiguratsiooni mõeldud target pesa (tavaliselt tootmise pesa).

Vaheta automaatselt konfigureerida pesa on lihtne. Järgige alltoodud juhiseid.

1. **Juurutamine** tera, valige mitte tekitamiseks pesa, valige **Kõik sätted** , et pesa Blade.  

    ![][Autoswap1]

2. Klõpsake **rakenduse sätted**. Valige **Vaheta automaatselt** **sisse** , valige soovitud target pesa **Automaatne Vaheta pesa**ja klõpsake nuppu **Salvesta** käsuriba. Veenduge, et pesa konfigureerimine on täpselt mõeldud target pesa konfiguratsioon.

    Vahekaarti **teatised** kuvatakse flash roheline **edu** pärast selle toimingu lõpuleviimist.

    ![][Autoswap2]

    >[AZURE.NOTE] Veebirakenduse jaoks testimiseks Vaheta automaatselt, saate esmalt mitte tekitamiseks target pesa **Automaatne Vaheta pesa** funktsiooniga tuttavaks saada.  

3. Selle juurutamine pesa PTT koodi käivitada. Automaatne vahetamine juhtub mõne aja pärast ja Värskenda kajastub target pesa URL.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Kasutage mitme etapi Vaheta veebirakenduse jaoks ##

Mitme etapi vahetamine on saadaval lihtsustamiseks valideerimine kontekstis konfiguratsiooni elementide loodud jääda pesa nagu ühendusstringi. Sellisel juhul võib olla kasulik Vaheta target sellise konfiguratsiooni elementide rakendamine Vaheta allikas ja kinnitage enne Vaheta tegelikult jõustub. Üks kord Vaheta target konfiguratsiooni elemendid on rakendatud Vaheta allikas saadaolevad toimingud on kas lõpuleviimine nende ega taastamist algse konfiguratsiooni Vaheta allikat, mis on ka mõju nende tühistamine. Azure'i PowerShelli cmdlet-käsud saadaval mitme etapi Vaheta jaoks on kaasatud juurutamise slots jaotise Azure PowerShelli cmdlet-käsud.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Tagasivõtmisel tootmise rakenduse pärast vahetamine ##

Kui pärast pesa Vaheta tuvastatakse vigu, selle tagasi pöörata nende eelse Vaheta riigid poolt vahetada sama pesast kohe.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Kohandatud sooja enne vahetamine ##

Mõni rakendus võib olla vaja sooja kohandatud toiminguid. Web.config elemendi applicationInitialization konfiguratsioon võimaldab teil määrata kohandatud initialization toimingud enne, kui taotlus on vastu. Vaheta toimingu ootama selle kohandatud sooja lõpuleviimiseks. Siin on näide web.config fragment.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>Juurutamise pesa kustutamine##

Juurutamise pesa labale nuppu **Kustuta** käsuriba.  

![Juurutamise pesa kustutamine][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Azure'i juurutamine PowerShelli cmdlet-käsud

Azure'i PowerShelli on moodul, mis pakub cmdlet-käskude Azure, sh tugi web appi juurutamise teenindusaegu Azure'i rakendust Service haldamine Windows PowerShelli kaudu.

- Installimist ja konfigureerimist Azure PowerShelli ja autentimist Azure PowerShelli Azure tellimuse kohta teabe saamiseks vaadake, [Kuidas installida ja konfigureerida Microsoft Azure PowerShelli](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>Veebirakenduse loomine

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Juurutamise pesa web appi loomine

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Mitme etapi Vaheta algatada ja rakendada allika pesa target pesa konfigureerimine

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Mitme etapi Vaheta esimese etapi tagasipööramine ja andmeallika pesa konfiguratsiooni taastamine

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Vaheta juurutamine

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Juurutamise pesa kustutamine

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Azure'i käsurea liides (Azure'i CLI) käsud juurutamine

Azure'i CLI pakub platvormidel käsku Azure, võimalusega haldamise veebirakenduse juurutamise slots töötamine.

- Juhised installimist ja konfigureerimist Azure'i CLI, sh Azure CLI ühenduse Azure tellimuse kohta vt [installida ja konfigureerida Azure CLI](../xplat-cli-install.md).

-  Saadaolevate käskude loendi Azure'i rakenduse teenuse CLI Azure, helistage `azure site -h`.

----------
### <a name="azure-site-list"></a>Azure'i saidi loend
Helistage praeguse tellimuse veebirakenduste kohta leiate **Azure'i saidi loend**, nagu järgmises näites.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Azure'i saidi loomine
Pesa juurutamise loomiseks kõne **Azure'i saidi loomine** ja määrake mõne olemasoleva veebirakenduse nimi ja nime pesa loomiseks, nagu järgmises näites.

`azure site create webappslotstest --slot staging`

Uue pesa andmeallika juhtelemendi lubamiseks kasutada **--git** valikut, nagu järgmises näites.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>Azure'i saidi vahetamine
Veendumaks, et värskendatud juurutamise pesa tootmise rakendus, kasutage **Azure'i saidi Vaheta** käsku Vaheta toimingut, nagu järgmises näites. Tootmise rakendus ei kogenud mõnda aega ega läbivad külmkäivitust.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Azure'i saidi kustutamine
Juurutamise pesa, mis pole enam vaja kustutamiseks käsu **Azure'i saidi kustutamine** , nagu järgmises näites.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="next-steps"></a>Järgmised sammud ##
[Azure'i rakendust Service Web Appi – plokk web juurdepääsu mitte tekitamiseks juurutamine](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Microsoft Azure'i tasuta prooviversioon](/pricing/free-trial/)

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ning selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
