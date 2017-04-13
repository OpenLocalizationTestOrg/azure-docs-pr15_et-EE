<properties 
    pageTitle="Etapp pilvepõhise teenuse juurutamise (Node.js) | Microsoft Azure'i" 
    description="Saate teada, kuidas juurutada Azure rakendust lavastus keskkonnas, siis abil virtuaalse IP (VIP) Vaheta tootmiskeskkonda juurutamine." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="staging-an-application-in-azure"></a>Rakenduse Azure lavastus

Pakkimisel rakenduse saab juurutada lavastus keskkonnas Azure katsetada enne, kui teisaldate need tootmiskeskkonda, kus rakendus on juurdepääsetav Internetis. Lavastus keskkond on täpselt nagu tootmiskeskkonda, välja arvatud, et pääsete juurde ainult etapiviisilise rakenduse muudetud URL, mis on loodud Azure'i. Pärast teie rakendus töötab õigesti, seda saab kasutusele võtta tootmiskeskkonda täites virtuaalse IP (VIP) vahetamine.

> [AZURE.NOTE] Selles artiklis toodud juhiseid kehtivad ainult Azure pilveteenuse teenust sõlm rakendused.

## <a name="step-1-stage-an-application"></a>Samm 1: Etapis rakendus

Selles ülesandes antakse ülevaade etapp rakenduse **Microsoft Azure PowerShelli**abil.

1.  Teenuse avaldamisel lihtsalt läheb selle **-pesa** **Avalda-AzureServiceProject** cmdlet-parameeter.

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Valige **Pilveteenustega** [Azure klassikaline portaali] sisse logima. Pärast pilveteenusesse on loodud ja **lavastus** veerus olek on uuendatud **opsüsteemi**, klõpsake teenuse nimi.

    ![portaali kuvamise töötab teenus][cloud-service]

3.  Valige **armatuurlaua**ja seejärel valige **lavastus**.

    ![pilveteenuse teenuste armatuurlaud][cloud-service-dashboard]

4. Pange tähele **Saidi URL-i** kirje õige väärtus. DNS-i nimi on muudetud sisemine ID Azure'i loonud.

    ![saidi URL-i][cloud-service-staging-url]

Nüüd saate kontrollida, kas rakendus töötab õigesti lavastus keskkonnas lavastus saidi URL-i abil.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Samm 2: Üleminek rakenduse abil vahetada VIP valmistamisel

Pärast uuele versioonile üle viidud versiooni rakenduse lavastus keskkonnas, te saate kiiresti kättesaadavaks valmistamisel poolt vahetada on keskkondade lavastus virtuaalse IP (VIP).

> [AZURE.NOTE] See toiming eeldab, et teil on juba juurutatud rakenduse tootmisele ja etapiviisilise rakenduse täiendatud versioon.

1.  [Azure'i klassikaline portaali]sisse logida, klõpsake nuppu **Pilveteenustega** ja seejärel valige teenuse nimi.

2.  **Armatuurlaua**valige **lavastus**ja seejärel klõpsake nuppu **Vaheta** lehe allosas. Avaneb dialoogiboks VIP vahetamine.

    ![dialoogiboksi VIP vahetamine][vip-swap-dialog]

3.  Teave üle ja seejärel klõpsake nuppu **OK**. Kahe juurutuse alustage lavastus juurutamise aktiveerib tootmise ning tootmise juurutamise aktiveerib lavastus värskendamine.

Teil on edukalt etapiviisilise juurutamine ja uuendatud tootmise juurutamise poolt VIP rakenduses lavastus juurutamise abil vahetada.

## <a name="additional-resources"></a>Lisaressursid

- [Juurutamise uuele versioonile üle viidud tootmise poolt vahetada VIP Azure]

[Azure'i klassikaline portaal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Juurutamise uuele versioonile üle viidud tootmise poolt vahetada VIP Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
