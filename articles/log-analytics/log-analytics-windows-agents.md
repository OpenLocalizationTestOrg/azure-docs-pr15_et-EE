<properties
    pageTitle="Ühenduse loomine Windows arvutis Log Analytics | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse toiminguid, mida tuleb ühendada oma kohapealse infrastruktuur Windowsi arvutid otse OMS, kasutades kohandatud versiooni Microsoft jälgimise Agent (MMA)."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# <a name="connect-windows-computers-to-log-analytics"></a>Ühenduse loomine Windows arvutis Log Analytics

Selles artiklis kirjeldatakse toiminguid, mida tuleb ühendada oma kohapealse taristu Windowsi arvutid otse OMS tööruumid, kasutades kohandatud versiooni Microsoft jälgimise Agent (MMA). Peate installima ja agentide kõik arvutid, mida soovite ühendada pardal OMS soovite, et neid andmeid saata OMS ja vaadata ja toimivad OMS portaalis andmete. Iga agent saate aruande mitme tööruumid.

Saate installida agentide häälestuse käsurea, abil või kus soovitud oleku konfiguratsioon (DSC) Azure'i automaatika.  

>[AZURE.NOTE] Jaoks virtuaalmasinates töötavad Azure saate lihtsustada installi [virtuaalse masina laiendi](log-analytics-azure-vm-extension.md)abil.

Arvutites Interneti-ühendus, kasutab agent OMS andmeid saata Interneti-ühendus. Arvutites, millel on Interneti-ühendus, saate kasutada proxy või OMS Log Analytics ekspediitor.

Teie Windowsi arvutite ühendamiseks OMS on väga lihtne, kasutades 3 lihtsad juhised:

1. Agent setup faili alla laadida
2. Installige agent valite meetodi abil
3. Konfigureerige agent või vajadusel lisada täiendavad tööruumid,

Järgmisel joonisel on esitatud teie Windowsi arvutisse ja OMS seos, kui olete installinud ja konfigureerinud agentide.

![OMS otsesed – agent skeem](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Süsteeminõuded ja nõutav konfiguratsioon
Enne installimist või agentide juurutada, lugege läbi tagamiseks vajalikud vastavusnõudeid järgmisi üksikasju.

- Saate installida ainult OMS MMA arvutites, kus töötab Windows Server 2008 SP 1 või uuem versioon või Windows 7 SP1 või uuem versioon.
- Peate tellimuse OMS.  Lisateabe saamiseks lugege teemat [Alustamine Log Analytics](log-analytics-get-started.md).
- Iga Windowsi arvuti peate saama HTTPS-i kasutamine Interneti-ühenduse. Sellega saab otse, proxy, või OMS Log Analytics ekspediitor kaudu.
- Saate installida OMS MMA eraldiseisev arvutid, serverid ja virtuaalmasinates. Kui soovite majutatava Azure'i virtuaalmasinates ühenduse OMS, vt [ühenduse Azure'i virtuaalmasinates Log Analytics abil](log-analytics-azure-vm-extension.md).
- Agent tuleb kasutada TCP pordi 443 erinevate ressursse. Lisateavet leiate teemast [puhverserveri ja tulemüüri sätete konfigureerimine Log Analytics](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>Agent setup faili alla laadida OMS
1. OMS portaalis, klõpsake **lehel,** klõpsake paani **sätted** .  Klõpsake vahekaarti **Ühendatud andmeallikate** ülaosas.  
    ![Ühendatud andmeallikate menüü](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Klõpsake jaotises **Manustamine otse arvutisse** **Alla laadida Windows Agent** kehtivad teie arvuti protsessori tüüp setup faili alla laadida.
3. **Tööruumi ID**paremal, klõpsake ikooni koopia ja kleepige Notepadi ID-d.
4. **Primaarvõtme**paremal ikooni kopeerige ja kleepige Notepadi võti.     
    ![Tööruumi ID "ja" primaarvõtme kopeerimine](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>Installige agent häälestuse abil
1. Installiprogramm installida agent arvutis, kus soovite hallata.
2. Klõpsake lehel Tere tulemast nuppu **Järgmine**.
3. Lehel litsentsitingimused läbi lugeda litsents ja seejärel klõpsake nuppu **nõustun**.
4. Lehel sihtkausta muutmine või vaikimisi installi kausta ja seejärel klõpsake nuppu **edasi**.
5. Lehel Agent häälestuse suvandite seadmine saate valida, et Azure'i Log Analytics (OMS), Toiminguhalduri, agent ühendamiseks või võite Valikud tühjaks jätta kui soovite konfigureerida hiljem agent. Klõpsake nuppu **edasi**.   
    - Kui valisite ühenduse Azure'i Log Analytics (OMS), kleepige **Tööruumi ID** ja eelmise jaotise Notepadi kopeeritud **Tööruumi Key (primaarvõti)** ja klõpsake nuppu **edasi**.  
        ![kleepige tööruumi ID "ja" primaarvõti](./media/log-analytics-windows-agents/connect-workspace.png)
    - Kui valisite ühenduse Toiminguhalduri, tippige **Halduse rühma nime**, **Management serveri** nimi ja **Management Server Port**ja seejärel klõpsake nuppu **edasi**. Kontolehel Agent toimingu valida, kas kohalik süsteem või kohaliku domeeni konto ja seejärel klõpsake nuppu **edasi**.  
        ![juhtimise rühma konfigureerimine](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent toimingu konto](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Valmis installilehest, vaadake oma valikud ja klõpsake nuppu **Installi**.
7. Lehe konfigureerimine on lõpule viidud, klõpsake nuppu **valmis**.
8. Kui olete lõpetanud, kuvatakse **Microsoft Agenti jälgimine** **Juhtpaneel**. Saate vaadata oma konfiguratsioon ja veenduge, et agent on ühendatud, et funktsionaalseid ülevaateid (OMS). Kui ühendatud OMS, agent kuvab teadet, mis kinnitab: **Microsoft jälgimise Agent on ühenduse loonud teenusesse Microsoft Operations Management Suite.**

## <a name="install-the-agent-using-the-command-line"></a>Installige käsureal agent
- Muuta ja seejärel installige käsureal agent järgmises näites abil.

    >[AZURE.NOTE] Kui soovite agent versioonile, peate kasutama funktsiooni Log Analytics skriptimise API. Versioonitäienduse agent järgmisest jaotisest.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>Versioonitäienduse agent ja lisage tööruumi skripti abil
Saate uuendada agent ja lisamine tööruumi, kasutades funktsiooni Log Analytics skriptimise API koos PowerShelli järgmises näites.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Kui olete kasutanud käsurea või skripti varem installima ja konfigureerima agent, `EnableAzureOperationalInsights` asendati `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Installige Azure'i automaatika DSC kasutamine agent

>[AZURE.NOTE] See toiming ja skripti näide on täiendage olemasoleva agent.

1. Importida xPSDesiredStateConfiguration DSC mooduli [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) Azure automatiseerimine.  
2.  Azure'i automaatika muutuv varad loomine *OPSINSIGHTS_WS_ID* ja *OPSINSIGHTS_WS_KEY*. Määratud *OPSINSIGHTS_WS_ID* oma OMS Log Analytics tööruumi ID-d ja *OPSINSIGHTS_WS_KEY* määramine teie tööruumi primaarvõti.
3.  Kasutage alltoodud skripti ja salvestage see MMAgent.ps1
4.  Muuta ja seejärel installige Azure'i automaatika DSC kasutamine agent järgmises näites abil. Importida MMAgent.ps1 Azure automatiseerimine Azure automatiseerimine kasutajaliidese või cmdlet-käsu abil.
5.  Saate määrata konfiguratsiooni sõlm. 15 minuti jooksul sõlme kontrollib selle konfiguratsiooni ja MMA on sõlm lükata.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Agent käsitsi konfigureerida või lisada täiendavad tööruumid
Kui olete installinud agentide aga ei konfigureerinud, neid või kui soovite teatada mitme tööruumidesse agent, saate lubada agent või selle konfigureerida järgmine teave. Kui olete konfigureerinud agent, agent teenuse register ja saavad vajalikud konfiguratsiooniteavet ja haldus tuvastamine, mis sisaldavad lahenduse teave.

1. Kui olete installinud Microsoft Agenti jälgimine, avage **Juhtpaneel**.
2. Avage **Microsoft Agenti jälgimine** ja seejärel vahekaarti **Azure'i Log Analytics (OMS)** .   
3. Klõpsake nuppu **Lisa** **lisamine tööruumi Log Analytics** dialoogiboksi avamiseks.
4. Kleepige **Tööruumi ID** ja **Tööruumi Key (primaarvõti)** kopeeritud Notepadi eelmise jaotise tööruumi, mille soovite lisada, ja seejärel klõpsake nuppu **OK**.  
    ![Töö ülevaated konfigureerimine](./media/log-analytics-windows-agents/add-workspace.png)

Pärast andmete kogutakse arvutitest agent jälgida, ilmub jälgida OMS arvutite arvu **Serverid ühendatud**OMS portaalis **sätted** vahekaardil **Ühendatud allikad** .


## <a name="to-disable-an-agent"></a>Agent keelamine
1. Pärast installimist agent, avage **Juhtpaneel**.
2. Avage Microsoft Agenti jälgimine ja seejärel vahekaarti **Azure'i Log Analytics (OMS)** .
3. Valige tööruumi ja seejärel klõpsake käsku **Eemalda**. Korrake seda toimingut, kõik muud tööruumide jaoks.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Soovi korral saate konfigureerida agentide teatada Toiminguhalduri rühma haldamine

Kui kasutate oma IT-taristu Toiminguhalduri, saate kasutada ka MMA agent Toiminguhalduri agendina.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>MMA agentide teatada rühma Toiminguhalduri halduse konfigureerimine
1.  Arvutites, kuhu on installitud agent, avage **Juhtpaneel**.
2.  Avage **Microsoft Agenti jälgimine** ja seejärel klõpsake vahekaarti **Toiminguhalduri** .
    ![Microsoft jälgimise Agent Toiminguhalduri menüü](./media/log-analytics-windows-agents/om-mg01.png)
3.  Kui teie Toiminguhalduri serverid Active Directory integreerimine, klõpsake nuppu **haldus jaotises ülesanded: AD DS automaatselt värskendada**.
4.  Klõpsake nuppu **Lisa rühm halduse** dialoogiboksi avamiseks **lisamine** .  
    ![Microsoft Agenti jälgimine halduse rühma lisamine](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  **Juhtimise rühma nimi** väljale Tippige oma rühma nimi.
6.  Tippige väljale **peamine management server** peamine management serveri arvuti nimi.
7.  Tippige väljale **Management serveri port** TCP pordi number.
8.  Valige jaotises **Toimingu kontolt**, kas kohalik süsteem või kohaliku domeeni kontoga.
9.  Klõpsake nuppu **OK** , sulgege dialoogiboks **Lisa rühm haldus** ja seejärel klõpsake nuppu **OK** , et sulgeda dialoogiboks **Microsoft Agenti atribuudid** .

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Soovi korral saate konfigureerida agentide kasutada OMS Log Analytics ekspediitor

Kui teil on serverid või kliendid, mis on Interneti-ühendus, saate siiski on neid andmeid saata OMS OMS Log Analytics ekspediitor abil.  Ekspediitor kasutamisel saadetakse kõik andmed agentide ühe server, mis on Interneti kaudu. Ekspediitor edastab andmed agentide OMS otse ilma analüüsimine andmed, mis on üle.

[OMS Log Analytics ekspediitor](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) ekspediitor, sh häälestamise ja konfigureerimise kohta lisateabe saamiseks vt.

Täpsemat teavet selle kohta, kuidas konfigureerida oma agentide kasutada puhverserverit, mis sel juhul on OMS ekspediitor, [Log Analytics puhverserveri ja tulemüüri sätete konfigureerimine](log-analytics-proxy-firewall.md).

## <a name="optionally-configure-proxy-and-firewall-settings"></a>Soovi korral võite puhverserveri ja tulemüüri sätete konfigureerimine
Kui teil on puhverserverid ja tulemüürid keskkond, mis Internetis juurdepääsu piiramiseks, vaadake [puhverserveri ja tulemüüri sätete konfigureerimine Log Analytics](log-analytics-proxy-firewall.md) lubamiseks oma agentide OMS teenusega suhelda.

## <a name="next-steps"></a>Järgmised sammud

- Funktsioonide lisamine ja andmete kogumine [lisamine Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md) .
- Kui teie ettevõte kasutab tulemüüri või puhverserveri nii, et agentide saaksid suhelda Log Analytics teenuse [puhverserveri ja tulemüüri sätete konfigureerimine Log Analytics](log-analytics-proxy-firewall.md) .
