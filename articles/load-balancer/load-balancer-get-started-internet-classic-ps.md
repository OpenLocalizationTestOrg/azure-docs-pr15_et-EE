<properties
   pageTitle="Vastastikuste laadi koormusetasakaalustusteenuse klassikalise režiimi PowerShelli kaudu Interneti loomise alustamiseks | Microsoft Azure'i"
   description="Saate teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse klassikalise režiimi PowerShelli abil"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Vastastikuste laadi koormusetasakaalustusteenuse (klassikaline) PowerShellis Interneti loomise alustamiseks

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Samuti saate [teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse Azure'i ressursihaldur abil](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Laadi koormusetasakaalustusteenuse PowerShelli kaudu häälestamine

Laadi koormusetasakaalustusteenuse, PowerShelli kaudu seadistamiseks järgige alltoodud juhiseid:

1. Kui te pole kunagi varem Azure PowerShelli, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../../articles/powershell-install-configure.md) ja järgige juhiseid kõik viis selleks Azure'i sisse logida ja valige oma tellimuse.


2. Pärast virtuaalse masina loomist saate PowerShelli cmdlet-käskude laadi koormusetasakaalustusteenuse lisamiseks virtuaalse masina sees sama pilveteenuses.

Järgmises näites tuleb lisada laadi koormusetasakaalustusteenuse seadmine nimega "webfarm" pilve "mytestcloud" (või myctestcloud.cloudapp.net), nimega "web1" ja "web2" virtuaalmasinates laadi koormusetasakaalustusteenuse lõpp-punktide lisamine. Laadi koormusetasakaalustusteenuse saab võrguliikluse pordi 80 ja laadi nõuded vahel virtuaalmasinates, kohalik lõpp-punkt (jaotises selle juhul pordi 80) määratletud TCP abil.


### <a name="step-1"></a>Samm 1
Esimese VM "web1" on koormus tasakaalustatud lõpp-punkti loomine

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Samm 2

Teise VM "web2" sama laadi koormusetasakaalustusteenuse määramine nimi teise lõpp-punkti loomine

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Laadi koormusetasakaalustusteenuse virtuaalse masina eemaldamine

Eemalda – AzureEndpoint abil saate laadi koormusetasakaalustusteenuse virtuaalse masina endpoint eemaldamine

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Järgmised sammud

Samuti võite teha [ka sisemise koormuse koormusetasakaalustusteenuse loomise alustamiseks](load-balancer-get-started-ilb-classic-ps.md) ja mis tüüpi on especific laadi koormusetasakaalustusteenuse võrgu liikluse käitumine [jaotuse režiimi](load-balancer-distribution-mode.md) konfigureerimine.

Kui teie rakendus peab ühendused toitsid serverite laadi koormusetasakaalustusteenuse taga, saavad aru veel [jõudeolekus TCP ajalõpp sätete laadi koormusetasakaalustusteenuse](load-balancer-tcp-idle-timeout.md)kohta. Lisateavet jõude ühenduse käitumise kasutamisel Azure'i laadi koormusetasakaalustusteenuse aitab.

