<properties
   pageTitle="Luua VM turuplatsil PowerShelli häälestamine | Microsoft Azure'i"
   description="Juhised Azure PowerShelli häälestamise ja selle kasutamine on valikuline protsess flow luua VM pilte juurutada ja müüa, Azure turuplatsiga"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Azure'i turuplatsil pakkumise loomiseks Azure PowerShelli häälestamine
Üksikasjalikku teavet selle kohta, kuidas luua PowerShelli Azure, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Lihtne lähenemine on kasutada serdi meetod, mis allalaaditavate failide ja impordib autentimise jaoks vajalik sert. Vaja serdi saamiseks kasutage **Get-AzurePublishSettingsFile** cmdlet-käsk. Kui teil palutakse, salvestage fail. Serdi importimine PowerShelli seanss, kasutage **Impordi-AzurePublishSettingsFile** cmdlet-käsk.

Levinud Microsoft Azure'i tellimuse sätete PowerShelli seansi talletada ja konfigureerimiseks kasutada **Set-AzureSubscription** ja **Valige-AzureSubscription** cmdlet-käsud:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Esimese käsu seostab vaikekonto salvestusruumi tellimuse (VM ettevalmistamise tegevuse jaoks vajalik).  Teine teeb tellimuse praegune nimi (tuvasta cmdletid).

## <a name="see-also"></a>Vt ka
- [Alustamine: Azure'i turuplatsi avaldamine pakkumise kohta](marketplace-publishing-getting-started.md)
- [Turuplatsil virtual arvuti pildi loomine](marketplace-publishing-vm-image-creation.md)
