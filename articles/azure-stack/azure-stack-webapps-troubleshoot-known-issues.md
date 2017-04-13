<properties
    pageTitle="Web rakenduste Azure virnas - teadaolevate probleemide ja tõrkeotsing | Microsoft Azure'i"
    description="Üksikasjalikud juhised veebirakenduste Azure'i virnas"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web Appsi ressursi pakkuja - teadaolevate probleemide ja tõrkeotsing

> [AZURE.NOTE] Järgmine teave kehtib ainult Azure virnas TP1 juurutuste.

Kui teil tekib probleeme juurutamine või töötades veebirakendustega Azure'i virnas kohta vaadake alltoodud juhiseid.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure'i virnas Web Appsi pole saadaval portaalis

![Azure'i virnas veebirakenduste luua uue veebirakenduse][1]

Kui olete lõpetanud [registreerimise Azure virnas Web Apps pakkuja](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) , kui te ei saa leida Web + mobiilsideseadmete tera, siis proovige järgmist:
* **Portaali Logi välja** ja **sulgege brauser** ja seejärel **uue brauseri akna kasutajanime portaali** ja proovige uuesti.

Kui te ikka ei näe web ja mobiilne blade, proovige järgmist:

1.  Leidke **Azure'i virnas Host masina** **Hyper-V** halduri **xRPVM** ja **ühenduse** VM.
2.  Avage mõni **administraatorina käsuviiba** ja käivitage **IISRESET**
3.  Naaske **ClientVM** ja avage **Uus brauseriaken**, **portaali sisse logida** ja proovige uuesti.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Juurutamise nurjub, kui veebirakenduse ressursside uue rühma loomine

Esimese Web Apps eelvaade, tuleb **Kõik veebirakenduste** luua sama ressursirühm Web Appsi ressursi pakkuja (**AppService-LOCAL**).  See on probleem ja lahendatakse tulevaste eelvaade.

## <a name="other-issues"></a>Muud küsimused

Kui teil esineb muid probleeme veebirakenduste kohta Azure virnas saatke [Azure'i virnas Foorum] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) kus liikmed meie meeskond on postituste jälgimine.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
