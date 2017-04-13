<properties 
   pageTitle="Azure'i automaatika ajakava | Microsoft Azure'i"
   description="Automaatika ajakavasid kasutatakse plaanimine tegevusraamatud Azure'i automaatika automaatselt käivituma. Kirjeldab, kuidas luua ja hallata ajakava nii, et saate alustada ka käitusjuhendi automaatselt teatud ajal või korduva ajakava."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="mgoedtel" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Azure'i automaatika lisamine käitusjuhendi plaanimine

Azure'i automaatika käivitada määratud ajal on käitusjuhendi plaanimiseks linkida selle ühe või mitme ajakava. Ajakava saab konfigureerida Käivita üks kord või kordumise mõne tunni või päeva ajakava tegevusraamatud Azure klassikaline portaali ja tegevusraamatud Azure'i portaalis, saate lisaks ajakava nende jaoks iga nädal, iga kuu, kindlate päevade nädala või kuu päeva või kuu kindla päeva.  Mõne käitusjuhendi saab linkida mitme ajakavasid ja ajakava võib olla mitu tegevusraamatud sellega lingitud.

>[AZURE.NOTE]  Ajakava Azure automatiseerimine DSC konfiguratsioone ei toeta.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShelli cmdlet-käsud

Saate luua ja hallata ajakava Azure automatiseerimine Windows PowerShelli abil kasutatakse järgmises tabelis cmdlet-käsud. Need saata [Azure PowerShelli mooduli](../powershell-install-configure.md)osana.

|Cmdlet-käsud|Kirjeldus|
|:---|:---|
|**Azure'i ressursihaldur cmdlet-käsud**||
|[Get-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603733.aspx)|Toob ajakava.|
|[Uue AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx)|Loob uue ajakava.|
|[Eemalda – AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603691.aspx)|Eemaldab ajakava.|
|[Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx)|Määrab olemasoleva ajakava atribuudid.|
|[Get-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt619406.aspx)|Toob ajastatud tegevusraamatud.|
|[Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx)|Mõne käitusjuhendi seostab ajakava.|
|[Unregister AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603844.aspx)|Dissotsieerub on käitusjuhendi ajakava kaudu.|
|**Azure'i teenuse halduse cmdletid**||
|[Get-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)|Toob ajakava.|
|[Uue AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|Loob uue ajakava.|
|[Eemalda – AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)|Eemaldab ajakava.|
|[Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690270.aspx)|Määrab olemasoleva ajakava atribuudid.|
|[Get-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)|Toob ajastatud tegevusraamatud.|
|[Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690265.aspx)|Mõne käitusjuhendi seostab ajakava.|
|[Unregister AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Dissotsieerub on käitusjuhendi ajakava kaudu.|

## <a name="creating-a-schedule"></a>Ajakava loomine

Azure'i portaalis klassikaline portaalis või Windows PowerShelli abil saate luua uue ajakava tegevusraamatud. Teil on ka luua uue ajakava lisamine käitusjuhendi linkimisel Azure klassikaline või Azure'i portaalis ajakava.

>[AZURE.NOTE] Kui ajakava seostada mõne käitusjuhendi automatiseerimine kehtivate versioonidega moodulid salvestab teie konto ja neid linke selle ajakava.  See tähendab, et kui oleksite mooduli versiooni 1.0 teie konto, kui olete loonud ajakava ja seejärel värskendage mooduli versiooni 2.0, ajakava jätkub 1.0 kasutama.  Mooduli värskendatud versiooni kasutamiseks peate looma uue ajakava. 

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Luua uue ajakava Azure'i portaalis

1. Klõpsake Azure'i portaalis automatiseerimise kontolt **varad** paani avamiseks **varad** tera.
2. Klõpsake paani avamiseks **ajakavasid** tera **ajakava** .
3. Klõpsake nuppu **Lisa ajakava** tera ülaosas.
4. Enne **uue ajakava** , tippige soovitud **nimi** ja soovi korral **Kirjeldus** uus ajakava.
5. Valige kas Ajasta käivitavad üks kord või reoccurring ajakava, valides **üks kord** või **Korduvus**.  Kui valite **üks kord** Määrake **algusaeg** ja seejärel klõpsake nuppu **Loo**.  Kui valite **Korduvus**, määrake **algusaeg** ja sagedus, kui sageli soovite korrata käitusjuhendi - **tund**, **päeva**-, **nädala**või **kuu**.  Kui valite loendist drop-down **nädala** või **kuu** , **Korduvus suvand** kuvatakse tera ja valikut **kordumist** tera esitatakse ja nädalapäeva saate valida, kui valisite **nädal**.  Kui valisite **kuu**, saate valida **nädala päeva** või kuu kalendris teatud päeva ja lõpuks tehke te soovite käivitage see kuu viimase päeva või ei ja seejärel klõpsake nuppu **OK**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Luua uue ajakava Azure klassikaline portaalis

1. Azure'i klassikaline portaalis valige automatiseerimine ja valige seejärel automatiseerimise konto nimi.
1. Valige vahekaart **varad** .
1. Klõpsake akna allosas **Lisada säte**.
1. Klõpsake **Lisa ajakava**.
1. Tippige **nimi** ja soovi korral **Kirjeldus** uus schedule.your ajakava kestab **Üks kord**, **tunni**, **päeva**-, **nädala**või **kuu**.
1. Määrake **Algusaeg** ja muid võimalusi, sõltuvalt teie valitud ajakava.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Uue ajakava loomine Windows PowerShelli abil

Saate luua uue ajakava Azure'i automaatika tegevusraamatud klassikaline [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet-käsk või cmdlet-käsu [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) tegevusraamatud Azure'i portaalis. Määrake algusaeg ajakava ja see peaks töötama sagedus.

Valimi järgmised käsud näitab, kuidas luua ajakava 15 ja 30 iga kuu Azure'i ressursihaldur cmdlet-käsu abil.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Valimi järgmised käsud näitab, kuidas luua uus ajakava, mis töötab iga päev 3:30 PM 20 jaanuar 2015 alustades Azure'i Teenusehaldus cmdlet-käsk.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Linkimine on käitusjuhendi ajakava

Mõne käitusjuhendi saab linkida mitme ajakavasid ja ajakava võib olla mitu tegevusraamatud sellega lingitud. Kui soovitud käitusjuhendi parameetrid, saate väärtused neid sisestada. Peate sisestama kõik kohustuslikud parameetrite väärtused ja võib ette mis tahes valikuliste parameetrite väärtused.  Iga kord, kui see ajakava käivitatakse käitusjuhendi kasutatakse neid väärtusi.  Saate lisada sama käitusjuhendi teise ajakava ja määrake erinevate parameetrite väärtused.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Ajakava linkimiseks soovitud käitusjuhendi Azure'i portaalis

1. Klõpsake Azure portaalis automatiseerimise kontolt **tegevusraamatud** paani avamiseks **tegevusraamatud** tera.
2. Klõpsake selle nime plaanida käitusjuhendi raames.
3. Kui käitusjuhendi on praegu seotud ajakava, siis antakse teile võimalus luua uue ajakava või lingi olemasoleva ajakava.  
4. Kui käitusjuhendi on parameetrid, valige suvand **Muuda käivitada sätted (vaikimisi: Azure'i)** ja **parameetrite** tera on esitatud, kuhu saate sisestada teavet vastavalt sellele.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Ajakava linkimiseks soovitud käitusjuhendi Azure klassikaline portaalis

1. Azure'i klassikaline portaalis valige **automatiseerimine** ja klõpsake seejärel automatiseerimise konto nimi.
2. Valige vahekaart **tegevusraamatud** .
3. Klõpsake selle nime plaanida käitusjuhendi raames.
4. Klõpsake vahekaarti **ajakava** .
5. Kui käitusjuhendi on praegu seotud ajakava, siis antakse teile võimalust **Link uus ajakava** või **lingi olemasoleva ajakava**.  Kui käitusjuhendi on praegu seotud ajakava, klõpsake **linki** akna allosas juurdepääsemiseks suvanditest.
6. Kui käitusjuhendi on parameetrid, küsitakse teilt, kas soovite nende väärtuste jaoks.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Ajakava linkimiseks soovitud käitusjuhendi Windows PowerShelli abil

Saate [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) klassikaline käitusjuhendi või [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet-käsu jaoks tegevusraamatud Azure'i portaalis ajakava linkida.  Saate määrata selle käitusjuhendi parameetrite väärtused koos parameetriga parameetrid. Parameetrite väärtused kohta lisateabe saamiseks vt [alates on Käitusjuhendi Azure'i automaatika](automation-starting-a-runbook.md) .

Valimi järgmised käsud näitab, kuidas ajakava linkimiseks soovitud parameetrid Azure'i ressursihaldur cmdlet-käsu kasutamine käitusjuhendi.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Valimi järgmised käsud Kuva kuidas link Azure'i Teenusehaldus cmdlet-käsu kasutamine parameetrite ajakava.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Ajakava keelamine

Kui keelate ajakava, käivituvad kõik sellega seotud tegevusraamatud enam selle ajakava. Käsitsi saate keelata ajakava või määrata aegumise ajaks ajakavasid sagedusega need loomisel. Aegumise aeg jõudmisel keelatakse ajakava.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure'i portaalis ajakava keelamine

1. Klõpsake Azure'i portaalis automatiseerimise kontolt **varad** paani avamiseks **varad** tera.
2. Klõpsake paani avamiseks **ajakavasid** tera **ajakava** .
2. Klõpsake ajakava avamiseks üksikasjad tera nime.
3. Muuta **pole** **lubatud** .

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Azure'i klassikaline portaalist ajakava keelamine

Saate keelata ajakava Azure klassikaline portaalis ajakava üksikasjade lehe ajakava kaudu.

1. Azure'i klassikaline portaalis valige automatiseerimine ja klõpsake seejärel automatiseerimise konto nimi.
1. Valige vahekaart varad.
1. Klõpsake nime selle üksikasjade lehe avamiseks ajakava.
2. Muuta **pole** **lubatud** .

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Windows PowerShelli abil ajakava keelamine

Cmdlet [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) abil saate muuta atribuutide olemasoleva ajakava klassikaline käitusjuhendi või [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet-käsu jaoks tegevusraamatud Azure'i portaalis. Ajasta keelamiseks määrata **false** **IsEnabled** parameeter.

Valimi järgmised käsud Kuva keelamine ajakava käitusjuhendi, mis on Azure ressursihaldur cmdlet-käsu abil.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Valimi järgmised käsud Kuva keelamine ajakava Azure'i Teenusehaldus cmdlet-käsu abil.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Järgmised sammud

- Azure'i automaatika tegevusraamatud alustamine leiate [Azure'i automaatika lisamine Käitusjuhendi alates](automation-starting-a-runbook.md) 