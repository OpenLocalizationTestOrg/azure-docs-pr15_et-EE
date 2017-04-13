<properties 
    pageTitle="Teksti tegevusraamatud Azure'i automaatika redigeerimine"
    description="Selles artiklis antakse erinevate toimingute töötamiseks PowerShelli ja PowerShelli töövoo Azure'i automaatika tegevusraamatud teksti redaktori abil."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Teksti tegevusraamatud Azure'i automaatika redigeerimine

Azure'i automaatika teksti redaktori saab muuta [PowerShelli tegevusraamatud](automation-runbook-types.md#powershell-runbooks) ja [PowerShelli töövoo tegevusraamatud](automation-runbook-types.md#powershell-workflow-runbooks). See on tüüpilised omadused muu koodi redaktorid nagu IntelliSense'i ja värvikoodid teisiti lisavõimalusi aidata teil juurdepääs levinud tegevusraamatud ressursid.  Sellest artiklist leiate üksikasjalikud juhised läbimiseks selle toimetaja erinevaid funktsioone.

Teksti redaktori sisaldab funktsiooni code tegevusi, varad ja lapse tegevusraamatud lisamiseks on käitusjuhendi. Asemel ise koodi tippima, saate valida loendist saadaolevad ressursid ja on sobiv kood käitusjuhendi lisada.

Azure'i automaatika iga käitusjuhendi on saadaval kahes versioonis mustand ja avaldatud. Redigeeri käitusjuhendi mustand versiooni ja seejärel avaldada nii, et seda saab käivitada. Avaldatud versioon ei saa redigeerida. Lisateabe saamiseks vaadake [avaldamine on käitusjuhendi](automation-creating-importing-runbook.md#publishing-a-runbook) .

Töötada [Graafiline tegevusraamatud](automation-runbook-types.md#graphical-runbooks), leiate [Azure'i automaatika loome graafilised](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Redigeerimiseks on käitusjuhendi Azure'i portaalis

Järgmiste toimingute abil saate avada mõne käitusjuhendi teksti redaktoris redigeerimiseks.

1. Azure'i portaalis, valige konto automatiseerimine.
2. Klõpsake paani **tegevusraamatud** tegevusraamatud loendi avamiseks.
3. Klõpsake selle nime, mida soovite redigeerida, ja seejärel klõpsake nuppu **Redigeeri** käitusjuhendi.
6. Tehke nõutav redigeerimine.
7. Kui teie muudatused on lõpule jõudnud, klõpsake nuppu **Salvesta** .
8. Kui soovite käitusjuhendi avaldatakse mustand uusim versioon, klõpsake nuppu **Avalda** .

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Mõne käitusjuhendi cmdlet-käsu lisamine

2. Asetage kursor teksti redaktori lõuendile, kuhu soovite cmdlet.
3. Teegi juhtelemendi laiendage **cmdlet-käsud** . 
3. Laiendage moodul, mis sisaldab cmdlet-käsk, mida soovite kasutada.
4. Paremklõpsake cmdlet-käsk Lisa ja valige **Lisa lõuend**.  Kui cmdlet määrata rohkem kui ühe parameetri, siis vaikimisi komplekt lisatakse.  Saate valida erinevaid parameetri rea cmdlet ka laiendada.
4. Cmdlet-i kood, lisatakse selle kogu parameetrite loendi.
5. Sisestage sobivat väärtust asemel ümbritsetud looksulud <> mõni nõutav parameetrite andmetüüpi.  Eemaldada ei pea te parameetrid.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Mõne käitusjuhendi lapse käitusjuhendi koodi lisamine

2. Asetage kursor teksti redaktori lõuendile, kuhu soovite [lapse käitusjuhendi](automation-child-runbooks.md)kood.
3. Laiendage **tegevusraamatud** teegi juhtelemendis. 
3. Paremklõpsake käitusjuhendi lisada, ja valige **Lisa lõuend**.
4. Kõigi käitusjuhendi parameetrite kohatäidetest lisatakse lapse käitusjuhendi kood.
5. Asendage väärtused iga parameetri jaoks kohatäited.

### <a name="to-insert-an-asset-into-a-runbook"></a>Vara lisamine on käitusjuhendi

2. Asetage kursor teksti redaktori lõuendile, kuhu soovite lapse käitusjuhendi kood.
3. Laiendage **varasid** teegi juhtelemendis. 
4. Laiendage varade soovitud tüüpi.
3. Paremklõpsake varade lisada, ja valige **Lisa lõuend**.  [Muutuv varad](automation-variables.md), valige **Lisa "saada muutuja" lõuend** või **lisada "seadmine muutuja" lõuend** sõltuvalt sellest, kas soovite saada või määratud muutuja.
4. Vara kood lisatakse käitusjuhendi.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Redigeerimiseks on käitusjuhendi Azure'i portaalis

Järgmiste toimingute abil saate avada mõne käitusjuhendi teksti redaktoris redigeerimiseks.

1. Azure'i portaalis valige **automatiseerimine** ja klõpsake seejärel automatiseerimise konto nimi.
2. Valige vahekaart **tegevusraamatud** .
3. Klõpsake käitusjuhendi, mida soovite redigeerida ja seejärel valige vahekaart **autori** nime.
5. Klõpsake ekraani allservas nuppu **Redigeeri** .
6. Tehke nõutav redigeerimine.
7. Kui teie muudatused on lõpule jõudnud, klõpsake nuppu **Salvesta** .
8. Kui soovite käitusjuhendi avaldatakse mustand uusim versioon, klõpsake nuppu **Avalda** .

### <a name="to-insert-an-activity-into-a-runbook"></a>Toimingu lisamiseks on Käitusjuhendi

1. Asetage kursor teksti redaktori lõuendile, kuhu soovite tegevuse.
1. Ekraani allservas nuppu **Lisa** ja seejärel **tegevus**.
1. Valige veerus **Integreerimine mooduli** moodul, mis sisaldab tegevuse.
1. Valige paanil **tegevuse** tegevus.
1. Veerus **Kirjeldus** Märkus tegevuse kirjeldus. Teise võimalusena võite klõpsata linki Kuva üksikasjalikku abi spikker brauseris tegevuse käivitada.
1. Klõpsake paremnoolt.  Kui tegevuse parameetrid, on need loetletud teabe.
1. Klõpsake nuppu kontrolli.  Koodi käivitamiseks tegevuse saab lisada käitusjuhendi.
1. Kui tegevuse nõuab parameetrid, pakkuda sobivat väärtust asemel ümbritsetud looksulgudega <> andmetüüpi.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Mõne käitusjuhendi lapse käitusjuhendi koodi lisamine

1. Asetage kursor teksti redaktori lõuendile, kuhu soovite [lapse käitusjuhendi](automation-child-runbooks.md).
2. Ekraani allservas nuppu **Lisa** ja seejärel **Käitusjuhendi**.
3. Valige käitusjuhendi keskmist veeru lisada, ja klõpsake paremnoolt.
4. Kui käitusjuhendi on parameetrid, on need loetletud teabe.
5. Klõpsake nuppu kontrolli.  Koodi käivitamiseks valitud käitusjuhendi saab lisada praeguse käitusjuhendi.
7. Kui käitusjuhendi nõuab parameetrid, pakkuda sobivat väärtust asemel ümbritsetud looksulud <> andmetüüpi.

### <a name="to-insert-an-asset-into-a-runbook"></a>Vara lisamine on käitusjuhendi

1. Asetage kursor teksti redaktori lõuendile, kuhu soovite tegevuse toomiseks vara.
1. Ekraani allservas nuppu **Lisa** ja seejärel **sätet**.
1. Valige veerus **Toiming säte** soovitud toiming.
1. Valige saadaval varasid veerus keskele.
1. Klõpsake nuppu kontrolli.  Käitusjuhendi saab lisada koodi või vara seadmine.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Azure'i automaatika käitusjuhendi, mis Windows PowerShelli kaudu redigeerimiseks

Windows PowerShelli abil soovitud käitusjuhendi redigeerimiseks teie valitud redaktori kasutamiseks ja see .ps1 faili salvestada. Saate tuua sisu käitusjuhendi, ja seejärel Asendage olemasolev mustand käitusjuhendi muutmine üks [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) cmdlet-käsk [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) cmdlet-käsk.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Toomiseks sisu Käitusjuhendi, mis on Windows PowerShelli abil

Valimi järgmised käsud näitab, kuidas tuua script on käitusjuhendi ja salvestage see skripti faili. Selles näites tuuakse mustand versioon. Samuti on võimalik tuua käitusjuhendi avaldatud versiooni, kuigi see versioon ei saa muuta.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Käitusjuhendi, mis on Windows PowerShelli kaudu sisu muutmine

Valimi järgmised käsud näitab, kuidas asendage olemasolev sisu on käitusjuhendi skripti faili sisu. Pange tähele, et see on valimi samamoodi nagu [on käitusjuhendi Windows PowerShelli skripti faili importida](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Seotud artiklid

- [Loomise või importimise on käitusjuhendi Azure'i automaatika](automation-creating-importing-runbook.md)
- [Õppekeskuse PowerShelli töövoo](automation-powershell-workflow.md)
- [Graafilise Azure'i automaatika koostamine](automation-graphical-authoring-intro.md)
- [Serdid](automation-certificates.md)
- [Ühendused](automation-connections.md)
- [Identimisteave](automation-credentials.md)
- [Ajakava](automation-schedules.md)
- [Muutujad](automation-variables.md)