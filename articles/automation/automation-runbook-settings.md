<properties 
   pageTitle="Käitusjuhendi sätted"
   description="Kirjeldatakse mõnda käitusjuhendi Azure automatiseerimine ja kuidas neid Azure'i haldusportaal ja Windows PowerShelli abil muuta sätete konfigureerimine."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Käitusjuhendi sätted

Azure'i automaatika iga käitusjuhendi on mitu sätteid, mis aitab tuvastada ja muuta oma logimine käitumist. Kõiki neid sätteid on kirjeldatud allpool, millele järgneb toimingute kohta, kuidas neid muuta.

## <a name="settings"></a>Sätted

### <a name="name-and-description"></a>Nimi ja kirjeldus

Pärast selle loomist ei saa muuta mõne käitusjuhendi nime. Kirjeldus on valikuline ja võib olla kuni 512 märki.

### <a name="tags"></a>Sildid

Siltide abil saate määrata erinevate sõnad ja laused, mis aitab tuvastada on käitusjuhendi. Näiteks kui saadate on käitusjuhendi [Käitusjuhendi Galerii](https://msdn.microsoft.com/library/dn781422.aspx), saate määrata teatud siltide käitusjuhendi olema loetletud kategooriate tuvastamiseks. Saate määrata mitu sildid on käitusjuhendi, eraldades need komaga.

### <a name="logging"></a>Logimine

Vaikimisi on kirjutatud Verbose ja edenemise kirjete töö ajalugu. Saate muuta sätteid kindla käitusjuhendi logige need kirjed. Nende kirjete kohta leiate lisateavet teemast [Käitusjuhendi väljundi ja -teadetest](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Käitusjuhendi sätete muutmine

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Azure'i haldusportaal käitusjuhendi sätete muutmine

Saate muuta mõne käitusjuhendi Azure'i haldusportaal sätete **konfigureerimine** lehel käitusjuhendi.

1. Azure'i haldusportaal, valige **automatiseerimine** ja klõpsake seejärel automatiseerimise konto nimi.
1. Valige vahekaart **tegevusraamatud** .
1. Klõpsake soovitud käitusjuhendi nime.
1. Valige vahekaart **konfigureerimine** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Windows PowerShelli abil käitusjuhendi sätete muutmine

Cmdlet [Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) abil soovitud käitusjuhendi sätete muutmine. Kui soovite määrata mitu sildid, võite teha pakkuda massiivi või ühele tekstistringile komaga eraldatud väärtuste siltide parameeter. Saate praeguse siltide abil [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx).

Valimi järgmised käsud näitab, kuidas on käitusjuhendi atribuutide seadmine. See näide lisab kolme siltide olemasolevad sildid ja määrab Paljusõnaline kirjed olema sisse logitud.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Seotud artiklid
- [Käitusjuhendi väljundi ja teated](../automation-runbook-output-and-messages) 
- [Loomise või importimise on Käitusjuhendi](https://msdn.microsoft.com/library/dn643637.aspx) 