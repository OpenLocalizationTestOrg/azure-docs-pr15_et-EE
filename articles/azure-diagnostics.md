<properties
    pageTitle="Ülevaade: Azure'i diagnostika | Microsoft Azure'i"
    description="Kasutage Azure diagnostika silumine, jõudluse mõõtmiseks, jälgimine, liikluse analüüsi pilveteenustega, virtuaalmasinates ja teenuse struktuuri"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Mis on Microsoft Azure'i diagnostika


Azure'i diagnostika on võimalus Azure'is, mis võimaldab Diagnostikaandmete juurutatud rakendus kogum. Saate diagnostika laiend paljudest erinevatest allikatest. Praegu toetatud on Azure pilveteenuse teenuse Web ja töötaja rollid, kus töötab Microsoft Windows ja teenuse struktuuri Azure'i Virtuaalmasinates. Muud Azure teenused on oma eraldi diagnostika.

## <a name="data-you-can-collect"></a>Saate koguda andmeid

Azure'i diagnostika saab koguda järgmist tüüpi andmeid:

Andmeallikas|Kirjeldus
---|---
Jõudluse hinnale | Operatsioonisüsteemi ja kohandatud jõudluse hinnale
Logid     | Jälita sõnumite kirjutanud rakenduse
Windowsi sündmuste logid   | Teave saadetakse Windowsi sündmuste logimine süsteem
.NET Sündmuse allikas    | Koodi kirjutamist sündmusi, kasutades.NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) klass
IIS-i logid             | Lisateavet IIS-i veebisaitide
Näidata vastavalt ETW   | Mis tahes loodud sündmuse Windowsi jälitamise sündmused
Puistab ootamatult sulguda          | Lisateavet rakenduse krahhi korral protsessi olek
Kohandatud Tõrkelogide    | Logide loodud oma rakenduse või teenuse
Azure'i diagnostika taristu logid|Teavet diagnostika ise

Azure'i diagnostika laiendamine saate andmed kanda Azure storage konto või saatke see teenustele nagu [Rakenduse ülevaated](./application-insights/app-insights-cloudservices.md). Saate andmeid silumine ja tõrkeotsingu, jõudluse mõõtmiseks, ressursikasutus, liikluse analüüsi ja valmisoleku kavandamine ja audit.


## <a name="versioning"></a>Versioonimine
Teemast [Azure diagnostika versioonimise ajalugu](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Järgmised sammud
Valige mis teenuse soovite koguda diagnostika- ja kasutada järgmisi artikleid alustada. Kasutage viite teatud üldine Azure diagnostika linke.

## <a name="web-apps"></a>Web Apps
Pange tähele, et Web Apps kasutamine Azure diagnostika. [Veebirakenduste](./app-service-web/web-sites-enable-diagnostic-log.md) samaväärset teavet leida

## <a name="cloud-services-using-azure-diagnostics"></a>Pilveteenustega Azure'i diagnostika abil
- Kui Visual Studio abil, lugege teemat [Jälgida pilveteenustega rakenduse kasutamine Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md) alustada. Muul juhul lugege teemat
- [Kuidas jälgida pilveteenustega Azure'i diagnostika abil](./cloud-services/cloud-services-how-to-monitor.md)
- [Azure'i diagnostika Cloud Services rakenduse häälestamine](./cloud-services/cloud-services-dotnet-diagnostics.md)

Täpsemate teemade, vaadake teemat

- [Azure'i diagnostika abil rakenduse ülevaated pilveteenustega](./application-insights/app-insights-cloudservices.md)
- [Jälita pilveteenustega rakenduse Azure'i diagnostika voo](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [PowerShelli kasutamine Cloud Services diagnostika häälestamine](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Azure'i diagnostika abil Virtuaalmasinates
- Kui Visual Studio abil, lugege teemat [Kasutamine Visual Studio jälgida Azure'i Virtuaalmasinates](./vs-azure-tools-debug-cloud-services-virtual-machines.md) alustada. Vastasel korral lugege teemat
- [Azure'i diagnostika kohta on Azure virtuaalse masina häälestamine](./virtual-machines-dotnet-diagnostics.md)

Täpsemate teemade, vaadake teemat

- [PowerShelli abil saate seadistada diagnostika Azure'i Virtuaalmasinates](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Luua Windowsi virtuaalse masina jälgida ja diagnostika Azure'i ressursihaldur malli abil](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Teenuse struktuuri Azure'i diagnostika abil
Alustamine veebisaidil [teenuse struktuuri rakenduste jälgimine](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Palju muud teenuse struktuuri diagnostika artikleid on saadaval vasakul navigeerimisribal puus, kui teil on see artikkel.

## <a name="general-azure-diagnostics-articles"></a>Üldine Azure'i diagnostika artikleid
- [Azure'i diagnostika skeemi konfiguratsiooni](https://msdn.microsoft.com/library/azure/mt634524.aspx) – saate teada, kuidas muuta skeemifaili koguda ja marsruutimine diagnostika andmed. Pange tähele, et saate Visual Studio skeemi faili muutmiseks.
- [Kuidas andmed talletatakse Azure Storage Azure'i diagnostika](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - teada tabelite ja plekid, kus on kirjutatud diagnostikateavet nimesid.
- Teada, kuidas [kasutada jõudluse hinnale Azure'i diagnostika sisse](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Saate teada, kuidas [marsruutimiseks Azure'i diagnostika teavet rakenduse ülevaated](./azure-diagnostics-configure-applicationinsights.md)
- Kui teil on probleeme diagnostika käivitamine või Azure Storage tabeli andmete otsimine, leiate [Azure'i diagnostika tõrkeotsing](./azure-diagnostics-troubleshooting.md)
