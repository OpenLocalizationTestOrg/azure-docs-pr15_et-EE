<properties 
    pageTitle="Luba kaugtöölaud puhul pilveteenuste (Node.js)" 
    description="Saate teada, kuidas virtuaalmasinates, majutusteenuse Azure'i Node.js rakenduse remote-töölaua juurdepääsu lubamiseks." 
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

# <a name="enabling-remote-desktop-in-azure"></a>Kaugtöölaua Azure lubamine

Kaugtöölaua võimaldab teil juurde pääseda töölaua töötab Azure rolli eksemplari. Kaugtöölaua ühendus abil saate konfigureerida virtuaalse masina või rakenduse probleemide tõrkeotsing.

> [AZURE.NOTE] See artikkel kehtib Azure pilveteenuse teenust Node.js rakendused.


## <a name="prerequisites"></a>Eeltingimused

- Installima ja konfigureerima [Azure PowerShelli](../powershell-install-configure.md).
- Node.js minirakenduse juurutamine on Azure pilveteenusesse. Lisateavet leiate teemast [luua ja juurutada Node.js rakendus on Azure pilveteenusesse](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Samm 1: Azure'i PowerShelli kasutamine kaugtöölaua juurdepääsu teenuse konfigureerimine

Kaugtöölaua kasutamine, peate värskendama Azure teenuse määratlus ja konfiguratsiooni, kasutajanimi, parool ja sert. 

Arvuti, mis sisaldab allikas faile oma rakenduse kaudu tehke järgmist.

1. **Windows PowerShelli** Käivita administraatorina. ( **Menüü Start** või **Avakuvale**, otsing **Windows PowerShelli**jaoks.)

2.  Liikuge kausta, mis sisaldab teenuse määratlus (.csdef) ja teenuse konfiguratsiooni (.cscfg) faile.

3. Sisestage järgmine PowerShelli cmdlet-käsk:

        Enable-AzureServiceProjectRemoteDesktop

4. Kuvatakse vastav viip, sisestage kasutajanimi ja parool.

    ![luba-azureserviceprojectremotedesktop][enable-rdp]

3.  Sisestage järgmine PowerShelli cmdlet-käsk muudatused avaldada.

        Publish-AzureServiceProject

    ![avaldamine azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Samm 2: Ühenduse rolli eksemplari

Pärast avaldamist Update'i teenuse määratlus, saate luua ühenduse rolli eksemplari.

1.  [Azure'i klassikaline portaali]valige **Pilveteenustega** ja seejärel valige teenust.

    ![Azure'i klassikaline portaal][cloud-services]

2.  Klõpsake **eksemplarid**ja klõpsake **tootmise** või **lavastus** vaadata eksemplari teie teenuse jaoks. Näiteks valige ja seejärel klõpsake nuppu **Loo ühendus** lehe allosas.

    ![Lehe eksemplarid][3]

2.  Kui klõpsate nuppu **Loo ühendus**, veebibrauser palub teil RDP-faili salvestada. Seda faili avada. (Näiteks kui kasutate Internet Explorerit, klõpsake nuppu **Ava**.)

    ![Küsi avada või salvestada RDP-faili][4]

3.  Kui fail on avatud, kuvatakse järgmine viip turvalisus:

    ![Windowsi turbe kaudu][5]

4.  Klõpsake nuppu **Loo ühendus**ja turvalisuse küsimus kuvatakse sisestamise üksusele eksemplari. Sisestage parool, mis on loodud [samm 1] [samm 1: Azure'i PowerShelli kaudu kaugtöölaua juurdepääsu teenuse konfigureerimine], ja seejärel klõpsake nuppu **OK**.

    ![kasutajanime ja parooli küsimus][6]

Kui ühendus on loodud, kuvatakse kaugtöölaua ühendus Azure'i töölaua astme. 

![Seansil][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Samm 3: Kaugtöölaua juurdepääsu teenuse konfigureerimine 

Kui te enam ei kasuta kaugtöölaua ühendused rolli aknad pilves, Keela kaugtöölaua juurdepääs [Azure'i PowerShelli]kaudu.

1.  Sisestage järgmine PowerShelli cmdlet-käsk:

        Disable-AzureServiceProjectRemoteDesktop

2.  Sisestage järgmine PowerShelli cmdlet-käsk muudatused avaldada.

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Lisaressursid

- [Eemalt juurdepääs Azure'i rolli aknad] 
- [Kaugtöölaua kasutamine Azure rollid]
- [Node.js Arenduskeskus](/develop/nodejs/)

  [Azure'i PowerShelli]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure'i klassikaline portaal]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Eemalt juurdepääs Azure'i rolli aknad]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Kaugtöölaua kasutamine Azure rollid]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 