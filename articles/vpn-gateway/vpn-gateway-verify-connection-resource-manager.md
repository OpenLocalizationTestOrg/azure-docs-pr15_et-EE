<properties
   pageTitle="Kontrollige ühenduse lüüs | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas kontrollida ühenduse lüüs ressursihaldur juurutamise mudeli"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Kontrollige ühenduse lüüs

Saate kontrollida ühenduse lüüs on mitu võimalust. Selles artiklis näitab teile, kuidas kontrollida ühenduse ressursihaldur lüüsi olekut Azure portaali kaudu ja PowerShelli abil.


## <a name="verify-using-powershell"></a>Veenduge, et PowerShelli abil

Azure'i ressursihaldur PowerShelli cmdlet-käskude uusima versiooni installimiseks peate. Installimise PowerShelli cmdlet-käskude kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Ressursihaldur cmdlettide kasutamise kohta leiate lisateavet teemast [Windows PowerShelli kasutamine koos ressursihaldur](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Samm 1: Azure'i kontosse sisselogimine

1. Avage oma PowerShelli konsooli administraatoriõigustega ja kontoga ühenduse loomiseks.

        Login-AzureRmAccount

2. Kontrollige konto tellimused.

        Get-AzureRmSubscription 

3. Määrake tellimus, mida soovite kasutada.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Samm 2: Ühenduse kontrollimine


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Veenduge, et Azure'i portaalis

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Järgmised sammud

- Saate lisada virtuaalse võrguga virtuaalmasinates. Üksikasjalikud juhised leiate teemast [loomine virtuaalse masina](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

