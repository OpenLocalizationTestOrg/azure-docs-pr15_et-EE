<properties
   pageTitle="Azure'i (klassikaline) teenuste PowerShelli kaudu vastupidise DNS-i kirjeid hallata | Microsoft Azure'i"
   description="Kuidas hallata vastupidise DNS-i või PTR kirjete Azure'i teenuste klassikaline juurutamise mudeli PowerShelli kaudu. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Kuidas hallata vastupidise DNS-i kirjete Azure PowerShelli kaudu Azure'i teenused (klassikaline)

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Valideerimine vastupidise DNS-i kirjete
Muude tootjate ei saa luua vastupidise DNS-i kirjeid oma domeenide DNS-i vastendamise tagamiseks Azure ainult võimaldab luua vastupidise DNS-i kirje, kui üks järgmistest on täidetud:

- Vastupidi DNS-i FQDN on pilveteenusesse, mille jaoks on määratud, nimi või mis tahes pilveteenusesse ühe tellimuse nime nt, vastupidise DNS-i "contosoapp1.cloudapp.net.".
- Vastupidise DNS-i FQDN edasi lahendab nime või IP pilveteenusesse, mis on määratud, või mis tahes pilveteenuses nime või IP sees ühe tellimuse nt vastupidise DNS-i on "app1.contoso.com." mis on CName alias contosoapp1.cloudapp.net.

Kontroll on teha ainult siis, kui vastupidise DNS-i atribuut pilveteenus jaoks on määratud või muudetud. Perioodiliste uuesti valideerimine ei tehta.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Olemasoleva pilveteenustega vastupidise DNS-i lisamine
Olemasoleva pilveteenusesse "Set-AzureService" cmdlet-käsu abil saate lisada vastupidise DNS-kirje:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Pilveteenus vastupidise DNS-i loomine
Saate lisada uue pilveteenuses vastupidise DNS-i atribuut määratud abil "Set-AzureService" cmdlet-käsk:

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Vaate vastupidise DNS-i olemasoleva pilveteenused
Saate vaadata olemasolevaid pilveteenusesse "Get-AzureService" cmdlet-käsu abil konfigureeritud väärtus:

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Olemasoleva pilveteenustega vastupidise DNS-i eemaldamine
Olemasoleva pilveteenuses "Set-AzureService" cmdlet-käsu abil saate vastupidise DNS-i atribuudi eemaldada. Seda tehakse, määrates vastupidise DNS-i atribuudi väärtuseks tühi:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
