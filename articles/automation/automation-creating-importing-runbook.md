<properties
    pageTitle="Loomise või importimise on käitusjuhendi Azure'i automaatika"
    description="Selles artiklis kirjeldatakse, kuidas luua uus käitusjuhendi Azure automatiseerimine või ühte faili importida."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Loomise või importimise on käitusjuhendi Azure'i automaatika

Soovitud käitusjuhendi saate lisada Azure automatiseerimine, kas [uue](#creating-a-new-runbook) või mõne olemasoleva käitusjuhendi importimisega failist või [Käitusjuhendi Galerii](automation-runbook-gallery.md). Sellest artiklist leiate teavet loomise ja tegevusraamatud importimine failist.  Saate kõik üksikasjad juurdepääs ühenduse tegevusraamatud ja moodulid [Käitusjuhendi ja mooduli galeriide Azure automatiseerimine](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Uue käitusjuhendi loomine

Azure'i automaatika ühte Azure portaalide või Windows PowerShelli abil saate luua uue käitusjuhendi. Kui käitusjuhendi on loodud, saate selle teabe [Õ PowerShelli töövoo](automation-powershell-workflow.md) ja [graafiline loome Azure'i automaatika](automation-graphical-authoring-intro.md)abil redigeerida.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Luua uue Azure automatiseerimine käitusjuhendi Azure klassikaline portaal

[PowerShelli töövoo tegevusraamatud](automation-runbook-types.md#powershell-workflow-runbooks) saate töötada ainult Azure'i portaalis.

1. Azure'i klassikaline portaalis, klõpsake nuppu **Uus**, **rakenduse teenuseid**, **automaatika**, **Käitusjuhendi**, **Kiiresti luua**.
2. Sisestage nõutav teave ja seejärel klõpsake nuppu **Loo**. Käitusjuhendi nimi peab algama tähega ja võib-olla tähtede, numbrite, allakriipsutatud märkideks ja kriipsud.
3. Kui soovite redigeerida käitusjuhendi kohe ja seejärel klõpsake nuppu **Redigeeri Käitusjuhendi**. Klõpsake nuppu **OK**.
4. Teie uus käitusjuhendi kuvatakse menüü **tegevusraamatud** .


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Luua uue Azure automatiseerimine käitusjuhendi Azure'i portaal

1. Avage Azure'i portaalis konto automatiseerimine.
2. Klõpsake paani **tegevusraamatud** tegevusraamatud loendi avamiseks.
3. Klõpsake nuppu **Lisa soovitud käitusjuhendi** ja seejärel **Loo uus käitusjuhendi**.
2. Tippige käitusjuhendi **nimi** ja valige selle [Tüüp](automation-runbook-types.md). Käitusjuhendi nimi peab algama tähega ja võib-olla tähtede, numbrite, allakriipsutatud märkideks ja kriipsud.
3. Klõpsake nuppu **Loo** käitusjuhendi loomiseks ja avage redaktor.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Luua uue Azure automatiseerimine käitusjuhendi Windows PowerShelli abil

[Uus-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) cmdlet-käsu abil saate luua [PowerShelli töövoo käitusjuhendi](automation-runbook-types.md#powershell-workflow-runbooks)on tühi. Saate määrata, kas **nimi** parameetri luua tühja käitusjuhendi, mida saate hiljem redigeerida või saate määrata **tee** parameetri käitusjuhendi-faili importimise kohta. **Tippige** parameetri tuleks lisada ka selle määrata käitusjuhendi nelja tüüpi.

Valimi järgmised käsud näitab, kuidas luua uus tühi käitusjuhendi.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Faili lisamine käitusjuhendi importimine Azure automatiseerimine

Saate luua uue käitusjuhendi Azure'i automaatika importimisega PowerShelli skripti või PowerShelli töövoo (.ps1 laiend) või eksporditud graafilise käitusjuhendi (.graphrunbook).  Määrake [tüüpi käitusjuhendi](automation-runbook-types.md) , mis luuakse importida, võttes arvesse järgmisi asjaolusid.

- .Graphrunbook faili ainult importida uue [graafilise käitusjuhendi](automation-runbook-types.md#graphical-runbooks)ja graafiline tegevusraamatud saab luua ainult .graphrunbook failist.
- .Ps1 faili, mis sisaldab PowerShelli töövoo saab importida ainult on [PowerShelli töövoo käitusjuhendi](automation-runbook-types.md#powershell-workflow-runbooks).  Kui fail sisaldab mitut PowerShelli töövood, nurjub importimine. Peate iga töövoo salvestada eraldi faili ja importida iga eraldi.
- .Ps1 faili, mis ei sisalda töövoo saab importida on [PowerShelli käitusjuhendi](automation-runbook-types.md#powershell-runbooks) või on [PowerShelli töövoo käitusjuhendi](automation-runbook-types.md#powershell-workflow-runbooks).  Kui see imporditakse PowerShelli töövoo käitusjuhendi, siis selle töövoo teisendatakse ja kommentaarid kaasatakse käitusjuhendi, milles tehtud muudatused.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Azure'i klassikaline portaalis faili lisamine käitusjuhendi importimine
Järgmiste toimingute abil saate skripti faili importimine Azure automatiseerimine.  Pange tähele, et saate ainult importida .ps1 faili PowerShelli töövoo käitusjuhendi, selle portaalis.  Muud tüüpi peate kasutama Azure portaali.

1. Azure'i haldusportaal, valige **automatiseerimine** ja seejärel valige konto automatiseerimine.
2. Klõpsake nuppu **impordi**.
3. Leidke skriptifail importimiseks klõpsake **liikuge sirvides failini** .
4. Kui soovite redigeerida käitusjuhendi kohe ja seejärel klõpsake nuppu **Redigeeri Käitusjuhendi**. Klõpsake nuppu OK.
5. Uue käitusjuhendi kuvatakse menüü **tegevusraamatud** automatiseerimise konto.
6. Peate [avaldamine käitusjuhendi](#publishing-a-runbook) , enne kui saate selle käivitada.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Azure'i portaalis faili lisamine käitusjuhendi importimine
Järgmiste toimingute abil saate skripti faili importimine Azure automatiseerimine.  

>[AZURE.NOTE] Pange tähele, et saate ainult importida .ps1 faili PowerShelli töövoo käitusjuhendi, portaalis.

1. Avage Azure'i portaalis konto automatiseerimine.
2. Klõpsake paani **tegevusraamatud** tegevusraamatud loendi avamiseks.
3. Klõpsake nuppu **Lisa soovitud käitusjuhendi** ja seejärel **impordi**.
4. Valige **Käitusjuhendi faili** valimiseks imporditav fail
2. Kui väli **Name** on lubatud, siis on teil võimalus seda muuta.  Käitusjuhendi nimi peab algama tähega ja võib-olla tähtede, numbrite, allakriipsutatud märkideks ja kriipsud.
3. [Käitusjuhendi tüüp](automation-runbook-types.md) valitakse automaatselt, kuid saate muuta tüüp, võttes arvesse rakendatav piirangud järel. 
3. Uue käitusjuhendi kuvatakse loendis tegevusraamatud automatiseerimise konto.
4. Peate [avaldamine käitusjuhendi](#publishing-a-runbook) , enne kui saate selle käivitada.

>[AZURE.NOTE] Pärast importimist graafilise käitusjuhendi või graafilise PowerShelli töövoo käitusjuhendi, on teil võimalus kui muud tüüpi teisendada. Te ei saa teisendada teksti.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Windows PowerShelli skripti faili importimine on käitusjuhendi

[Impordi-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) cmdlet-käsu abil saate mustandina PowerShelli töövoo käitusjuhendi skripti faili importimine. Kui käitusjuhendi on juba olemas, importimine nurjub, kui kasutate funktsiooni *-Jõusta* parameeter. 

Valimi järgmised käsud Kuva skripti faili importimine on käitusjuhendi.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Avaldamine on käitusjuhendi

Kui loomine või importimine uue käitusjuhendi, avaldada enne selle käivitada.  Iga käitusjuhendi automaatika on mustand ja avaldatud versioon. Avaldatud versioon on saadaval käivitada ja saab redigeerida ainult mustand versiooni. Avaldatud versioon on mustand versiooni muudatused ei mõjuta. Kui mustand versioon tuleks teha kättesaadavaks, siis avaldate selle, mis kirjutab avaldatud versioon mustand versiooniga.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Avaldada käitusjuhendi, mis on Azure klassikaline portaalis

1. Avage käitusjuhendi Azure klassikaline portaalis.
1. Klõpsake ekraani ülaosas nuppu **Autor**.
1. Ekraani allservas nuppu **Avalda** ja seejärel **Jah** sõnumi kinnitamine.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Avaldada käitusjuhendi, mis on Azure portaalis

1. Avage käitusjuhendi Azure'i portaalis.
1. Klõpsake nuppu **Redigeeri** .
1. Klõpsake nuppu **Avalda** ja seejärel **Jah** sõnumi kinnitamine.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Avaldada käitusjuhendi, mis on Windows PowerShelli abil

[Avalda-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) cmdlet-käsu abil saate avaldada on käitusjuhendi Windows PowerShelli abil. Valimi järgmised käsud näitab, kuidas avaldada valimi käitusjuhendi.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Järgmised sammud
- Teavet selle kohta, kuidas saate kasutada Käitusjuhendi ja PowerShelli mooduli Galerii, leiate [Azure'i automaatika Käitusjuhendi ja mooduli galeriid](automation-runbook-gallery.md)
- Teksti redaktoriga PowerShelli ja PowerShelli töövoo tegevusraamatud redigeerimise kohta leiate lisateavet teemast [redigeerimine teksti tegevusraamatud Azure'i automaatika](automation-edit-textual-runbook.md)
- Graafilised käitusjuhendi loome kohta lisateabe saamiseks lugege teemat [graafilised loome Azure'i automaatika](automation-graphical-authoring-intro.md)
