<properties
   pageTitle="Pilvepõhise teenuse container loomine PowerShelli abil | Microsoft Azure'i"
   description="Selles artiklis selgitatakse, kuidas luua PowerShelli pilvepõhise teenuse container. Ümbris majutab veebi ja töötaja rollid."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Azure'i PowerShelli käsu käivitava abil saate luua mõnda tühja pilvepõhise teenuse container
Selles artiklis selgitatakse, kuidas saate kiiresti luua pilveteenustega ümbris Azure PowerShelli cmdlet-käskude abil. Järgige alltoodud juhiseid:

1. Installige Microsoft Azure'i PowerShelli cmdleti [Azure PowerShelli allalaadimiste](http://aka.ms/webpi-azps) lehe kaudu.
2. Avage käsuviip PowerShelli.
3. [Lisa-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) abil saate sisse logida.

    > [AZURE.NOTE] Täiendavaid juhiseid installimist Azure PowerShelli cmdleti ja Azure tellimuse ühenduse, vaadake [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

4. **Uus-AzureService** cmdlet-käsu abil saate luua mõnda tühja Azure pilveteenuse teenuse ümbrises.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Selles näites kasutada cmdlet tehke järgmist.
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Azure'i pilveteenuses loomise kohta lisateabe saamiseks käivitage:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Järgmised sammud

 * Pilvepõhise teenuse juurutamise juhtida viidata [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Eemalda-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)ja [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) käsud. [Kuidas seadistada pilveteenustega](cloud-services-how-to-configure.md) võib viidata ka täiendavat teavet.

 * Azure'i teenus cloud projekti avaldamiseks viidata: [pidev kohaletoimetamise jaoks pilveteenuses Azure](cloud-services-dotnet-continuous-delivery.md) **PublishCloudService.ps1** proovi kood.
