<properties
    pageTitle="Azure'i teenus siini abil Azure automatiseerimine haldamine | Microsoft Azure'i"
    description="Saate teada, kuidas hallata Azure'i teenus siini Azure automatiseerimine teenuse abil."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Haldamise Azure'i siini abil Azure automatiseerimine

Käesolev juhend tutvustab teile Azure automatiseerimine teenuse ja kuidas seda saab kasutada haldamise Azure'i teenus siini lihtsustamiseks.

## <a name="what-is-azure-automation"></a>Mis on Azure automatiseerimine?

[Azure'i automaatika](../automation/automation-intro.md) on Azure'i teenus lihtsustamine pilve haldus väljaprotsesside automatiseerimine ja soovitud olek konfigureerimise kaudu. Azure'i automaatika, käsitsi, kasutades korrata, pikaajalisi ja vigu tööülesannete automatiseerida suurendamiseks töökindlust, tõhusust ja kellaaja väärtuse oma ettevõtte jaoks.

Azure'i automaatika pakub väga usaldusväärne, mis on väga saadaval töövoo täitmise mootori, mis teie vajadustele skaala. Azure'i automaatika, protsesside saate olla käivitati käsitsi, 3-osaline süsteemide või ajastatud intervalliga nii, et tööülesanded tehakse täpselt vajadusel.

Vähendage funktsionaalseid pea kohal ja vabastage see ja DevOps töötajate keskenduda töö, mis lisab väärtusega, teisaldades cloud haldustoiminguid Azure'i automaatika käivituda.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Kuidas aitab Azure automatiseerimine Azure'i teenus siini hallata?

[Teenuse siini REST API](https://msdn.microsoft.com/library/azure/mt639375.aspx)abil saate hallata teenuse siini Azure automatiseerimine. Azure'i automaatika käivitamist PowerShelli skriptide paljude tööülesannete teenuse siini REST API abil. Saate siduda need REST API kõned Azure'i automaatika koos cmdlet-käskudega muude Azure'i teenuste Azure'i teenuste ja 3 tootja süsteemid üle keerukate automatiseerida.

Siin on mõned näited PowerShelli kaudu haldamise Azure'i teenus siini.

* [Kohandatud PowerShelli cmdlet-käskude Azure'i teenus siini järjekorrad haldamine](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Kuidas luua teenuse järjekorrad, teemade ja tellimuste PowerShelli skripti abil](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Azure'i teenus siini nimeruumid PowerShelli kaudu loomine](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

PowerShelli moodul Azure'i teenus siini sisse automaatika tegevusraamatud töötamiseks saate alla laadida [PowerShelli Galerii](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid Azure automatiseerimine ja kuidas seda saab kasutada Azure'i teenus siini haldamiseks, järgige neid linke Lisateavet Azure automatiseerimine.

* Teemast Azure automatiseerimine [Alustamine õpetus](../automation/automation-first-runbook-graphical.md)
* Lugege teemat Kuidas [hallata teenuse siini PowerShelli abil](service-bus-powershell-how-to-provision.md)
