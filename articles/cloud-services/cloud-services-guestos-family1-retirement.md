<properties
   pageTitle="Külalisena OS pere 1 aegumine teade | Microsoft Azure'i"
   description="Kui Azure Külastajate OS pere 1 aegumiskuupäeva juhtus ja kuidas kindlaks teha, kui teil esineb teave"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Külalisena OS pere 1 pensionile jäämise teatis

Esmalt teatati pensionile OS pere 1 1 juuni 2013.

**2 sept 2014** Azure'i Külastajate operatsioonisüsteem (Külastajate OS) pere 1.x, mis põhineb operatsioonisüsteemi Windows Server 2008, on ametliku kasutuselt kõrvaldatud. Kõigi katsete juurutada uusi teenuseid või uuendada olemasolevad teenused, kasutades pere 1 nurjub tõrketeate teatega, et Külastajate OS pere 1 on lõpetatud.

**3 november 2014** Pikendatud toega Külastajate OS pere 1 lõppenud ja see on täielikult kasutuselt kõrvaldatud. Kõigi teenuste endiselt pere 1 on mõjutada. Me võib nende teenuste igal ajal peatada. Ei ole kindel, mis kasutab ka edaspidi käivitada, v.a juhul, kui te käsitsi värskendada nende enda teenuste.

Kui teil on täiendavaid küsimusi, külastage [Cloud Services Foorumid](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) või [Azure tugiteenuste](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Mida see mõjutab?

Teie pilveteenustega mõjutab kui kehtib mõni järgmistest väidetest:

1. Teil on väärtus "osFamily ="1"järel konkreetselt nimetatud ServiceConfiguration.cscfg faili jaoks oma pilveteenuses.
2. Teil pole osFamily järel konkreetselt nimetatud ServiceConfiguration.cscfg faili jaoks oma pilveteenuses väärtus. Praegu kasutab süsteemi sel juhul vaikeväärtust, milleks on "1".
3. Azure'i klassikaline portaalis loetletud teie Külastajate operatsioonisüsteemi pere väärtus "Windows Server 2008".

Leidmiseks, mis teie pilveteenuste töötavad mis OS pere käivitada skripti all Azure PowerShelli, kuigi peate [Azure PowerShelli häälestamine](../powershell-install-configure.md) . Skripti kohta lisateabe saamiseks lugege teemat [Azure Külastajate OS pere 1 End elu: juuni 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Kui skript väljund osFamily veerg on tühi või sisaldab "1" kuvatakse teie pilveteenustega mõjutada OS pere 1 aegumiskuupäeva.

## <a name="recommendations-if-you-are-affected"></a>Kui teil esineb soovitused

Soovitame migreerite oma pilveteenuses rollid üks toetatud Külastajate OS peredele:

**Külaline OS pere 4.x** -Windows Server 2012 R2 *(soovitatav)*

1. Veenduge, et teie rakendus kasutab .NET Frameworki 4.0, 4.5 või 4.5.1 SDK 2.1 või uuem versioon.
2. Määrake osFamily atribuudi "4" ServiceConfiguration.cscfg faili ja Juurutage uuesti oma pilveteenusesse.


**Külaline OS pere 3.x** -Windows Server 2012

1. Veenduge, et teie rakendus kasutab koos .NET framework 4.0 või 4.5 SDK 1.8 või uuem versioon.
2. Määrake atribuudi osFamily "3" ServiceConfiguration.cscfg faili ja Juurutage uuesti oma pilveteenuses.


**Külaline OS pere 2.x** -Windows Server 2008 R2

1. Veenduge, et teie rakendus kasutab SDK 1.3 ja ülaltoodud koos .NET framework 3.5 või 4.0.
2. Määrake atribuudi osFamily "2" ServiceConfiguration.cscfg faili ja Juurutage uuesti oma pilveteenuses.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Pikendatud toega Külastajate OS pere 1 lõppenud 3 november 2014
Pilveteenustega Külastajate OS pere 1 enam ei toetata. Palun migreerida välja pere 1 nii kiiresti kui võimalik, et vältida tõrkeid.  

## <a name="next-steps"></a>Järgmised sammud
Vaadake üle uusima [Külastajate OS välja](cloud-services-guestos-update-matrix.md).
