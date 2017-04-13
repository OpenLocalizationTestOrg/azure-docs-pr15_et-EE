<properties
    pageTitle="Log Analytics Toiminguhalduri ühenduse | Microsoft Azure'i"
    description="Teie süsteemi keskmist Toiminguhalduri olemasoleva investeeringute haldamine ja Log Analytics laiendatud võimaluste kasutamiseks saate integreerida OMS tööruumi Toiminguhalduri."
    services="log-analytics"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="magoedte"/>

# <a name="connect-operations-manager-to-log-analytics"></a>Log Analytics Toiminguhalduri ühendamine

Teie süsteemi keskmist Toiminguhalduri olemasoleva investeeringute haldamine ja Log Analytics laiendatud võimaluste kasutamiseks saate integreerida OMS tööruumi Toiminguhalduri.  See võimaldab teil ära kasutada OMS võimalusi Toiminguhalduri, et kasutada:

- Jätkake Toiminguhalduri oma IT-teenuste seisundi jälgimine
- Integreerimine säilitada oma ITSM lahendusi, mis toetavad langeva ja probleem haldus
- Kohapealse ja avaliku pilve IaaS virtuaalmasinates, mida jälgida Toiminguhalduri juurutatud agentide elutsükli haldamine

Integreerimise System Center Operations Manager lisab väärtus teenuse toimingute strateegia kavandamine, kiirust ja tõhusust OMS kogumiseks, talletamine ja Toiminguhalduri andmete analüüsimine kasutamine.  OMS aitab oleksid ja vead probleemide tuvastamine ja kaetud reoccurrences toetavad oma olemasoleva probleemi protsess.   Otsingumootori tutvuda jõudluse sündmuse teatise andmete, rikas armatuurlaudade ja selle andmete esitamist mõtestatud viisil, aruandlusvõimalused paindlikkust näitab OMS toob congratulating Toiminguhalduri tugevus.

Andmete kogumine agentide, aruandlus Toiminguhalduri halduse rühma oma serverid Log Analytics andmeallikate ja teil on lubatud teie tellimus OMS lahendusi.  Sõltuvalt lahendus on sisse lülitatud, andmete järgmisi lahendusi ühte saadetakse otse Toiminguhalduri management serverist OMS veebiteenuse või tõttu agent haldusega süsteemi kogutud andmete maht on saadetud otse agent OMS veebiteenuse. Management server edastab otse OMS veebiteenuse OMS andmed, on kunagi kirjutatud OperationsManager või OperationsManagerDW andmebaasi.  Kui management serveri Ühenduvus OMS veebiteenuse, vahemälu andmeid kohalikult kuni side on taastatud OMS abil.  Kui management server on võrguühenduseta kavandatud hooldustööde või planeerimata katkestuste tõttu, jätkub teise management serveri haldus jaotises OMS Ühenduvus.  

Järgmisel joonisel on kujutatud management serverid ja agentide System Center Operations Manager management rühma ja OMS, sh suund ja pordid vahelise ühenduse.   

![OMS-toimingute-halduri-integreerimine-skeem](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

## <a name="system-requirements"></a>Süsteeminõuded
Enne alustamist vaadake üle kinnitamaks, et vastate vajalikud eeltingimused järgmisi üksikasju.

- OMS toetab ainult toimingute Manager 2012 SP1 UR6 ja suurem, ja toimingute Manager 2012 R2 UR2 ja suuremad.  Puhverserveri tugi on lisatud toimingute Manager 2012 SP1 UR7 ja toimingute Manager 2012 R2 UR3.
- Kõik Toiminguhalduri agentide peab vastama minimaalne tugi nõuetele. Veenduge, et agentide on minimaalne värskenduse ülespoole, muidu ei õnnestu Windows agent liikluse ja palju tõrked võivad täita Toiminguhalduri sündmuste logi.
- Tellimuse OMS.  Lisateabe saamiseks lugege läbi [Log Analytics alustamine](log-analytics-get-started.md).

## <a name="connecting-operations-manager-to-oms"></a>Toiminguhalduri ühenduse OMS
Tehke oma Toiminguhalduri rühma loomiseks üks tööruumide OMS konfigureerimiseks järgmised etapisarja.

1. Valige konsooli Toiminguhalduri **Administreerimine** tööruum.
2. Laiendage Operations Management Suite ja klõpsake nuppu **ühendus**.
3. Klõpsake linki **toimingute Management Suite registreerida** .
4. Klõpsake soovitud **toimingute haldamise komplekti kasutuselevõtt viisardit: autentimine** leht, Sisestage meiliaadress või telefoninumber ja mis on seotud tellimuse OMS administraatorikonto parooli ja klõpsake nuppu **Logi sisse**.
5. Kui teil on edukalt autenditud, sisse on **toimingute haldamise komplekti kasutuselevõtt viisardit: valige tööruumi** lehel, palutakse teil oma OMS tööruumi valimiseks.  Kui teil on rohkem kui üks tööruumi, valige tööruum, mida soovite registreerida Toiminguhalduri halduse rühma rippmenüü loendist ja seejärel klõpsake nuppu **edasi**.

    >[AZURE.NOTE] Toiminguhalduri toetab ainult üks OMS tööruumi korraga. Ühenduse ja eelmise tööruumi OMS registreeritud arvutid eemaldatakse OMS.

6. Klõpsake soovitud **toimingute haldamise komplekti kasutuselevõtt viisardit: Kokkuvõte** lehel, veenduge, et teie sätted ja kui need on õige, klõpsake nuppu **Loo**.
7. Klõpsake soovitud **toimingute haldamise komplekti kasutuselevõtt viisardit: valmis** lehel, klõpsake nuppu **Sule**.

### <a name="add-agent-managed-computers"></a>Agent hallatava arvutite lisamine
Integreerimine konfigureerinud oma OMS tööruumiga see ainult loob ühenduse OMS, andmed on kogutud agentide, aruandlus halduse rühmale. See ei toimu, kuni pärast saate konfigureerida, millised teatud agent hallatavad arvutid koguda andmeid Log Analyticsi. Võite valida arvuti objektide ükshaaval või valige rühm, mis sisaldab Windowsi arvuti objektid. Ei saa te valida rühma, mis sisaldab eksemplari teise klassi, nt loogika ketast või SQL andmebaase.

1. Avage Toiminguhalduri konsooli ja valige **Administreerimine** tööruumi.
2. Laiendage Operations Management Suite ja klõpsake nuppu **ühendus**.
3. Klõpsake jaotises toimingud linki **Lisa arvuti/rühma** pealkiri paani paremal.
4. Dialoogiboks **Otsing arvuti** saate otsida arvutites või rühmade Toiminguhalduri jälgida. Valige arvutis või rühmi, et pardal OMS, klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **OK**.

Saate vaadata arvutite ja konfigureeritud andmete kogumine hallatavate arvutite sõlm Operations Management Suite toimingud konsooli tööruumis **haldus** jaotises rühmad.  Siin saate lisada või eemaldada arvutite ja rühmade vastavalt vajadusele.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>Toimingud konsooli OMS puhverserveri sätete konfigureerimine
Kui sisemisse puhverserverisse on rühma – OMS veebiteenuse, tehke järgmist.  Nende sätete on ühes keskses kohas hallata rühma haldus ja jaotatud süsteemide agent haldusega koguda andmeid OMS ulatuse kaasatud.  See on kasulik, kui teatud lahenduste mööduda management server ja saata andmed otse OMS veebiteenus.

1. Avage Toiminguhalduri konsooli ja valige **Administreerimine** tööruumi.
2. Laiendage Operations Management Suite ja seejärel klõpsake nuppu **ühendused**.
3. Klõpsake vaates OMS ühenduse **Puhverserveri konfigureerimine**.
4. Klõpsake **toimingute haldamise komplekti viisardit: puhverserveri** lehel, valige **Kasuta puhverserverit Operations Management Suite juurdepääsu**, ja seejärel sisestage pordi number, näiteks http://corpproxy:80 URL ja klõpsake nuppu **valmis**.

Kui teie puhverserveri server nõuab autentimist, tehke järgmist identimisteabe ja sätteid, mis on vaja kajastuma hallatavad arvutid, mis annab OMS haldus jaotises konfigureerimiseks.

1. Avage Toiminguhalduri konsooli ja valige **Administreerimine** tööruumi.
2. Märkige jaotises **RunAs konfiguratsiooni** **profiilid**.
3. Avage **System Center Advisor käivitada nimega profiili puhverserveri** profiili.
4. Käivitage nimega profiili viisardis nuppu Lisa konto käivitada nimega. Saate luua uue [nimega käivitada konto](https://technet.microsoft.com/library/hh321655.aspx) või kasutada olemasolevat kontot. Selle konto peab olema läbi puhverserveri piisavaid õigusi.
5. Konto, mida soovite hallata, valige **valitud klassi, rühma või objekti**, klõpsake nuppu **Select...** ja seejärel klõpsake käsku **Rühmita...** **Rühma** otsinguvälja avamine
6. Otsige ja valige **Microsoft System Center Advisor jälgimise serveri rühma**.  Valige jaotises **Rühma** otsinguvälja sulgemiseks klõpsake nuppu **OK** .
7.  Klõpsake nuppu **Lisa konto käivitada nimega** dialoogiboksi sulgemiseks **OK** .
8.  Klõpsake viisardi lõpuleviimine ja salvestada muudatused **salvestada** .

Kui ühendus on loodud ja saate konfigureerida, millised agentide kogumine ja aruandlus andmete OMS, rakendatakse järgmist konfiguratsiooni haldus jaotises mitte tingimata samas järjestuses:

- Käivitage konto **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** luuakse.  See on seotud profiili nimega käivitamine **Microsoft System Center Advisor käivitada nimega profiili bloobimälu** ja kahte klassi - **Saidikogumi serveri** ja **Toimingute halduri rühma**eesmärk.
- Luuakse kaks konnektorid.  Esimene nimega **Microsoft.SystemCenter.Advisor.DataConnector** ja on konfigureeritud automaatselt tellimus, mis edastab kõik teatised, mis on loodud kõik tunnid haldus jaotises eksemplarid OMS Log Analytics abil. Teine konnektor on **Advisor konnektor**, kes vastutab OMS veebiteenuse suhtlemine ja andmete ühiskasutus.
- **Microsoft System Center Advisor jälgimise serveri rühma**lisatakse agentide ja valitud andmekogumise haldus jaotises rühmad.

## <a name="management-pack-updates"></a>Juhtimise keelepaketi värskendused
Pärast konfigureerimine on lõpetatud, Toiminguhalduri haldus jaotises loob ühenduse teenusega OMS.  Halduse server sünkroonimine veebiteenuse ja vastu võtta värskendatud konfiguratsiooniteavet halduse pakendis Toiminguhalduri integreerimine on lubatud lahendused kujul.   Toiminguhalduri värskendusi nende juhtimise paketid automaatselt alla laadida ja neid importida, kui need on saadaval.  On kaks reeglid eelkõige mis juhtida seda käitumist.

- **Microsoft.SystemCenter.Advisor.MPUpdate** - värskendab base OMS halduse paketid. Vaikimisi käivitatakse iga tunni kaksteist (12).
- **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - värskendused lahenduse halduse pakendis lubatud tööruumis. Vaikimisi käivitatakse iga viis (5) minuti järel.

Saate alistada reegleid kaks automaatse allalaadimise keelamisel neid või muuta sagedus sageduse management server sünkroonib OMS määratlemiseks kui uus halduspaketi on saadaval ja tuleks alla laadida.  Järgige juhiseid [kuidas alistada reegli või kuvari](https://technet.microsoft.com/library/hh212869.aspx) muutmise **sagedus** parameetri väärtuse muutmiseks Sünkroonimisajakava sekundites või muuta **lubatud** parameetri keelamiseks reegleid.  Suunata alistab kõik objektidele klassi toimingute halduri rühma.

Kui soovite jätkata jälgima teie olemasoleva muutmine juhtelemendi protsessi management pack väljalasete tootmise haldus jaotises kontrollimiseks, saate keelata reeglid ja neil teatud ajal, kui värskendused on lubatud. Kui teil on teie keskkonnas arengu või kv rühma ja see on Interneti-ühendus, saate konfigureerida selle rühma OMS Workspace'iga toetama seda stsenaariumi.  See võimaldab teil läbi vaadata ja hinnata OMS halduse paketid iteratiivne versioonides enne nende sisse oma Tootmisgrupp haldus.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Uue tööruumi OMS ka Toiminguhalduri rühma aktiveerimine
1. Tellimuse OMS sisse logida ja uue tööruumi loomine rakenduses [Microsoft Operations Management Suite](http://oms.microsoft.com/).
2. Valige **haldus** tööruumi avatud Toiminguhalduri konsooli kontoga, millel on toimingute halduri administraatorid rolli liige.
3. Laiendage Operations Management Suite ja valige väärtus **ühendused**.
4. Valige paani keskel küljel **Uuesti konfigureerimine toiming Management Suite** link.
5. Järgige **Toimingud halduse komplekti kasutuselevõtt viisard** ja Sisestage meiliaadress või telefoninumber ja mis on seotud teie uue OMS tööruumi administraatorikonto parooli.

    > [AZURE.NOTE] Funktsiooni **toimingute haldamise komplekti kasutuselevõtt viisardit: valige tööruumi** lehe esitab olemasoleva tööruumi, mis kasutab.


## <a name="validate-operations-manager-integration-with-oms"></a>Kinnitage toimingute halduri OMS integreerimine
On paar võimalust, saate kontrollida oma OMS Toiminguhalduri integreerimine õnnestumise.

### <a name="to-confirm-integration-from-the-oms-portal"></a>Kinnitamiseks portaalist OMS integreerimine

1.  OMS portaalis, klõpsake paani **sätted**
2.  Valige **ühendatud allikatest**.
3.  Jaotises System Center Operations Manager tabelis, peaksite nägema kui andmete vastuvõtmise Viimane number ja olek loetletud halduse rühma nime.

    ![connectedsources-OMS-sätted](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

4.  Pange tähele vasakus servas sätete lehe jaotises **Tööruumi ID** väärtus.  Saate kinnitada seda vastu Toiminguhalduri haldus jaotises allpool.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Integreerimine toimingud konsooli kinnitamiseks

1.  Avage Toiminguhalduri konsooli ja valige **Administreerimine** tööruumi.
2.  Valige **Haldus pakendit** ja on **otsida:** tekstitüüp kasti **Advisor** või **jälitusteabe**.
3.  Sõltuvalt lahendusi, kui teil on lubatud, kuvatakse vastav halduspakett toiminguhalduri otsingutulemuste loendis.  Näiteks kui olete lubanud teatiste haldus lahenduse, halduspakett toiminguhalduri Microsoft System Center Advisor teatiste haldus on loendis.
4.  Liikuge **jälgimis** vaates **Toimingud halduse Suite\Health oleku** kuvamine.  Valige Management server **Management serveri olekus** paani jaotises ja **Detail View** paanil kinnitada **autentimist teenuse URI** atribuudi väärtus vastab OMS tööruumi ID-ga.

    ![OMS-OpsMgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)


## <a name="remove-integration-with-oms"></a>Integreerimine OMS eemaldamine
Kui te enam ei kasuta teie Toiminguhalduri rühma ja OMS tööruumi, on mitu etapid õigesti haldus jaotises ühendus ja konfiguratsiooni eemaldamine. Järgnevalt on teil värskendada oma OMS tööruumi kustutades viide oma rühma, kustutada OMS ühendused ja seejärel kustutage halduse paketid toetavad OMS.   

1.  Avage toimingud halduri cmd kontoga, millel on toimingute halduri administraatorid rolli liige.

    >[AZURE.WARNING] Veenduge, et teil on mis tahes kohandatud halduse pakendis sõna Advisor või IntelligencePack enne jätkamist nimi, muidu järgmist kustutada rühmast haldus.

2.  Tippige käsuviiba shell`Get-SCOMManagementPack -name "*advisor*" | Remove-SCOMManagementPack`

3.  Järgmise tüüp`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack`

4.  Avage toimingud halduri toimingud konsooli kontoga, millel on toimingute halduri administraatorid rolli liige.
5.  Valige jaotises **Administreerimine** **Halduse paketid** sõlm ja selle **otsida:** väljale, tippige **Advisor** ja veenduge, et järgmised halduse paketid imporditakse ikkagi oma haldus jaotises:

    - Microsoft System Center Advisor
    - Microsoft System Center Advisor sisemise

6. OMS portaalis, klõpsake paani **sätted** .
7.  Valige **ühendatud allikatest**.
8.  Jaotises System Center Operations Manager tabelis, peaksite nägema tööruumist eemaldada halduse rühma nime.  Veerus **Viimase andmeid**, klõpsake nuppu **Eemalda**.  

    >[AZURE.NOTE] Kui ei ole tegevust tuvastatud rühmast ühendatud haldus ei ole **Eemalda** link availble 14 päeva pärast.  
   
9.  Kuvatakse aken, mis palub teil kinnitada, et soovite jätkata eemaldamine.  Klõpsake nuppu **Jah** jätkata. 

Kustutamiseks kaks konnektorid - Microsoft.SystemCenter.Advisor.DataConnector ja Advisor konnektor, PowerShelli skripti all oma arvutisse salvestada ja käivitada, kasutades järgmist.

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

>[AZURE.NOTE] Arvuti käivitate selle skripti kaudu, kui mitte management server, peaks olema toimingute Manager 2012 SP1 või R2 käsu shell sõltuvalt oma rühma versiooni installitud.

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

Tulevikus kui plaanite oma rühma OMS tööruumi taastamine, peate uuesti importimiseks on `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` halduse keelepaketi faili viimase värskenduse rollup rakendatud oma rühma.  Leiate selle faili selle `%ProgramFiles%\Microsoft System Center 2012` või `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` kausta.

## <a name="next-steps"></a>Järgmised sammud

- Funktsioonide lisamine ja andmete kogumine [lisamine Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md) .
- Kui teie ettevõte kasutab tulemüüri või puhverserveri nii, et agentide saaksid suhelda Log Analytics teenuse [puhverserveri ja tulemüüri sätete konfigureerimine Log Analytics](log-analytics-proxy-firewall.md) .
