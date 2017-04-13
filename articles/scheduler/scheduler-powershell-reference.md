<properties
 pageTitle="Ajasti PowerShelli cmdlet-käskude viide"
 description="Ajasti PowerShelli cmdlet-käskude viide"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Ajasti PowerShelli cmdlet-käskude viide

Järgmises tabelis kirjeldatakse ja põhi cmdlet-käskude Azure'i Scheduler viide lehele lingid.

Azure'i PowerShelli installimine ja seostada Azure tellimuse, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). 

[Azure'i ressursihaldur cmdlet-käskude](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx)kohta leiate lisateavet teemast [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md).

|Cmdlet-käsk|Cmdlet-käsk kirjeldus|
|---|---|
[Keela-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Töö saidikogumi keelatakse. 
[Luba-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Võimaldab töö saidikogumi.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Saab Scheduler tööde haldamine.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Saab töökohtade saidikogumid.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Saab töö ajalugu.
[Uue AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Loob mõne HTTP töö.
[Uue AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Loob töö saidikogumi.
[Uue AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Loob teenuse siini järjekorra töö.
[Uue AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Loob siini teema töö.
[Uue AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Loob salvestusruumi järjekorra töö. 
[Eemalda – AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Eemaldab ajasti töö.  
[Eemalda – AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Eemaldab töö saidikogumi. 
[Set-AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Muudab ajasti HTTP töö.
[Set-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Muudab töö saidikogumi. 
[Set-AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Muudab teenuse siini järjekorra töö.  
[Set-AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Muudab siin teemas töö. 
[Set-AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Muudab salvestusruumi järjekorra töö.   

Üksikasjalikumat teavet, saate kasutada mis tahes järgmised cmdlet-käsud. 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Vt ka


 [Mis on ajasti?](scheduler-intro.md)

 [Azure'i Scheduler põhimõtet, terminoloogia ja üksuse hierarhia](scheduler-concepts-terms.md)

 [Azure'i portaalis Scheduler kasutamise alustamine](scheduler-get-started-portal.md)

 [Lepingud ja arvelduse Azure'i ajasti](scheduler-plans-billing.md)

 [Azure'i Scheduler REST API viide](https://msdn.microsoft.com/library/mt629143)

 [Azure'i Scheduler kõrge-saadavus ja usaldusväärsus](scheduler-high-availability-reliability.md)

 [Azure'i Scheduler piirangud, vaikesätted ja kuvatavad tõrkekoodid](scheduler-limits-defaults-errors.md)

 [Azure'i Scheduler väljaminevat autentimist](scheduler-outbound-authentication.md)
