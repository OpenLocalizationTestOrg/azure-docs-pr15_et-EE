<properties
   pageTitle="PowerShelli cmdlet-käskude kasutamine Azure RemoteApp | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada Windows PowerShelli cmdlet-käskude Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Azure RemoteApp Windows PowerShelli cmdlet-käskude kasutamine

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

 Saate hallata ja säilitada oma saidikogumid Azure RemoteApp PowerShelli cmdlet-käsud. Alustamiseks tehke järgmist.

## <a name="get-the-cmdlets"></a>Saada cmdlet-käsud 
-------------
Esmalt alla laadima Azure PowerShelli cmdletid [siin](http://go.microsoft.com/?linkid=9811175)RemoteApp cmdlet-käsud on kaasatud. 

Lugege teemat [Azure RemoteApp cmdlet-käsu spikker](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Azure'i cmdlet-käskude kasutamine tellimuse konfigureerimine
------------------
[Käesoleva](../powershell-install-configure.md) juhendi abil saate esitada Azure tellimuse cmdlet-käsud.

Järgmiste juhiste abil saate kiiresti alustada:

1.  Laadige alla ja installige [Azure'i PowerShelli cmdlet-käsud](http://go.microsoft.com/?linkid=9811175).
2.  Käivitage Microsoft Azure'i PowerShelli.
3.  Käivitage **Lisa-AzureAccount** Azure tellimuse autentimiseks. Küsimise korral sisestage sama kasutajanimi ja parool, mida kasutate Azure portaali sisse logida.  
4.  Käivitage **Get-AzureSubscription** kasutaja kontoga seotud tellimuste loendi. 
5.  **Valige-AzureSubscription** käivitamine ja määrake soovitud tellimuse nime või ID, et kasutada PowerShelli konsooli.

Palju õnne, teie Azure PowerShelli konsooli on konfigureeritud ja kasutamiseks valmis. Arvestage, et peate korrake juhiseid 2 – 5 iga kord, kui käivitate selle Azure PowerShelli konsooli.  

## <a name="create-a-cloud-collection"></a>Pilveteenuse saidikogumi loomine
--------------------
See on lihtne, käivitage järgmine käsk:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Eeltoodud käsu avaldab automaatselt Microsoft Office 365 rakenduste (Excel, OneNote, Outlook, PowerPoint, Visio ja Word).

Saidikogumi loomine võib kuluda 30 minutit või kauem aega. Seetõttu tagastab selle käsu jälgimise ID, mille abil saate järgmiselt:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Pärast selle saidikogumi on lõpule jõudnud, saate kasutajad lisada saidikogumi järgmine käsk:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

Ja olete valmis! Kasutaja peaks oskama ühenduse rakenduse abil Azure RemoteApp klient leida [siin](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Saadaval cmdlet-käsud
On palju muud käsud, mis on meil, dokumentatsioonist need tulevad varsti.

Tavaline RemoteApp saidikogumi cmdlet-käsud: 

- Uue AzureRemoteAppCollection
- Get-AzureRemoteAppCollection.
- Set-AzureRemoteAppCollection.
- Update-AzureRemoteAppCollection.
- Eemalda – AzureRemoteAppCollection.
- Lisage AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Eemalda – AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Eemaldage AzureRemoteAppSession
- Autonoomsest AzureRemoteAppSessionLogoff
- Saada-AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Avaldamine AzureRemoteAppProgram
- Avaldamise AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

RemoteApp virtuaalse võrgu cmdlet-käsud:

- Uue AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Set-AzureRemoteAppVNet
- Eemalda – AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Get - AzureRemoteAppVpnDeviceConfigScript
- Lähtesta-AzureRemoteAppVpnSharedKey

RemoteApp malli pilti cmdlet-käsud:

- Uue AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Nimeta-AzureRemoteAppTemplateImage
- Eemalda – AzureRemoteAppTemplateImage

Muud RemoteApp cmdlet-käsud:

- Get-AzureRemoteAppLocation.
- Get-AzureRemoteAppWorkspace
- Set-AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
