<properties
    pageTitle="Puhverserveri ja tulemüüri sätete konfigureerimine Log Analytics | Microsoft Azure'i"
    description="Puhverserveri ja tulemüüri sätete konfigureerimine, kui teie agentide või OMS teenuseid kasutada teatud pordid."
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
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Log Analytics puhverserveri ja tulemüüri sätete konfigureerimine

Puhverserveri konfigureerimine vajalikud toimingud ja tulemüüri sätete Log Analytics rakenduses OMS erinevad Toiminguhalduri ja selle agentide võrreldes Microsoft jälgimise agentide, et ühendada serverid kasutamisel. Vaadake üle järgmised jaotised jaoks kasutatav tüüp.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Microsoft Agenti jälgimine ja puhverserveri ja tulemüüri sätete konfigureerimine

Microsoft Agenti jälgimine ühenduse ja OMS teenuse registreerida, see peab olema juurdepääsu oma domeenide ja URL-ide pordinumber. Kui kasutate puhverserverit side agent ja teenuse OMS, peate tagamaks, et asjakohased ressursid on kättesaadavad. Kui kasutate tulemüüri juurdepääsu piiramiseks Interneti-ühendus, peate oma tulemüüri lubada juurdepääsu OMS konfigureerida. Järgmises tabelis on loetletud pordid OMS vajab.

|**Agent ressurss**|**Pordid**|**Meediumierandi HTTPS kontroll**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443|Jah|
|\*. oms.opinsights.azure.com|443|Jah|
|\*. blob.core.windows.net|443|Jah|
|ODS.systemcenteradvisor.com|443| |

Järgmiste toimingute abil saate konfigureerida puhverserveri sätteid Microsoft Agenti jälgimine juhtpaneeli kaudu. Peate kasutama juhiseid iga server. Kui teil on palju servereid, mida teil on vaja konfigureerida, mida võib lihtsam skripti abil automatiseerida seda protsessi. Kui, siis lugege teemat järgmiste toimingute [Microsoft Agenti jälgimine kasutades skripti puhverserveri sätete konfigureerimine](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Microsoft Agenti jälgimine juhtpaneeli kaudu puhverserveri sätete konfigureerimine

1. Avage **Juhtpaneel**.

2. Avage **Microsoft Agenti jälgimine**.

3. Klõpsake vahekaarti **Puhverserveri sätted** .<br>  
  ![puhverserveri sätete menüü](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Valige **Kasuta puhverserver** ja sisestage URL-i ja pordi number, kui see on vajalik, nt sarnaselt. Kui teie puhverserveri server nõuab autentimist, sisestage kasutajanimi ja parool juurdepääsu puhverserveri.

Järgmiste toimingute abil saate luua PowerShelli skripti, et käivitada iga agent, mis ühendub serverid puhverserveri sätete määramiseks.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Microsoft Agenti jälgimine kasutades skripti puhverserveri sätete konfigureerimine

Kopeerige järgmises näites, värskendage seda teavet teatud keskkonna, salvestage see PS1 failinimelaiendiga ja seejärel käivitage skripti igas arvutis, mis ühendub OMS teenus.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Koos Toiminguhalduri puhverserveri ja tulemüüri sätete konfigureerimine

Toiminguhalduri halduse rühma loomiseks ja OMS teenuse registreerida, see peab olema juurdepääs oma domeenide ja URL-ide pordinumbrid. Kui kasutate puhverserverit side Toiminguhalduri management server ja teenuse OMS, peate tagamaks, et asjakohased ressursid on kättesaadavad. Kui kasutate tulemüüri juurdepääsu piiramiseks Interneti-ühendus, peate oma tulemüüri lubada juurdepääsu OMS konfigureerida. Isegi kui Toiminguhalduri management server ei taha puhverserverit, võib selle agentide. Sel juhul puhverserveri peaks olema konfigureeritud samal viisil nagu agentide on selleks, et lubada ja luba turvalisus ja haldamine lahenduse andmete hankimine saata selle OMS web teenus.

Selleks Toiminguhalduri agentide OMS teenuse suhelda, oma Toiminguhalduri infrastruktuur (sh agentide) peaks olema õige puhverserveri sätete ja versioon. Puhverserveri säte agentide jaoks on määratud Toiminguhalduri konsooli. Teie versioon peaks olema üks järgmistest:

- Toimingute Manager 2012 SP1 Update Rollup 7 või uuem versioon
- Toimingute Manager 2012 R2 Update Rollup 3 või uuem versioon


Järgmises tabelis on loetletud pordid, mis on seotud järgmised toimingud.

>[AZURE.NOTE] Mõni järgmistest ressurssidest mainida Advisor ja töö ülevaated, mõlemad OMS varasemaid versioone. Siiski loetletud ressursid võib tulevikus muutuda.

Siin on agent ressursid ja portide loendit:<br>

|**Agent ressurss**|**Pordid**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443|
|\*. oms.opinsights.azure.com|443|
|\*.blob.Core.Windows.net/\*|443|
|ODS.systemcenteradvisor.com|443|
<br>
Siit leiate loendi pordid ja management serveri ressursid:<br>

|**Management serveri ressurss**|**Pordid**|**Meediumierandi HTTPS kontroll**|
|--------------|-----|--------------|
|Service.systemcenteradvisor.com|443| |
|\*. service.opinsights.azure.com|443| |
|\*. blob.core.windows.net|443|Jah| 
|Data.systemcenteradvisor.com|443| | 
|ODS.systemcenteradvisor.com|443| | 
|\*. ods.opinsights.azure.com|443|Jah| 
<br>
Siin on OMS ja Toiminguhalduri konsooli ressursid ja portide loendit.<br>

|**OMS ja Toiminguhalduri konsooli ressurss**|**Pordid**|
|----|----|
|Service.systemcenteradvisor.com|443|
|\*. service.opinsights.azure.com|443|
|\*. live.com|Pordi 80 ja 443|
|\*. microsoft.com|Pordi 80 ja 443|
|\*. microsoftonline.com|Pordi 80 ja 443|
|\*. mms.microsoft.com|Pordi 80 ja 443|
|login.Windows.net|Pordi 80 ja 443|
<br>

OMS teenuse Toiminguhalduri rühma registreerimiseks tehke järgmist. Kui teil on probleeme side jaotise haldus ja OMS teenuse vahel, kasutage andmete edastamiseks OMS teenuse tõrkeotsing kinnitamine toiminguid.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Taotleda erandid OMS teenuse lõpp-punktid

1. Kasutage peate võib-olla esimese tabeli Toiminguhalduri management serveri vajalikud ressursid puuetega inimestele juurdepääsetavate mis tahes tulemüürid tagamiseks varem esitatud teavet.
2. Kasutage peate võib-olla teise tabeli tagada vajalike toimingute konsooli Toiminguhalduri ja OMS ressursside kaudu mis tahes tulemüürid varem esitatud teavet.
3. Kui kasutate Internet Exploreris puhverserverit, veenduge, et see on konfigureeritud ja töötab õigesti. Veenduge, et saate avada turvaline web ühenduse (HTTPS), näiteks [https://bing.com](https://bing.com). Kui brauseris ei toimi turvaline veebiühendus, see ilmselt ei toimi Toiminguhalduri halduskonsoolis veebiteenuste pilveteenuses.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>Toiminguhalduri konsooli puhverserveri konfigureerimine

1. Avage Toiminguhalduri konsooli ja valige **Administreerimine** tööruumi.

2. Laiendage **Töö ülevaated**, ja seejärel valige **Funktsionaalseid ülevaateid ühendus**.<br>  
    ![Toimingute halduri OMS ühendus](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. Klõpsake vaates OMS ühenduse **Puhverserveri konfigureerimine**.<br>  
    ![Toimingute halduri OMS ühenduse konfigureerimine puhverserver](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. Tekstivormingus funktsionaalseid ülevaateid häälestusviisard: Puhverserverit, valige **Kasuta juurdepääsu funktsionaalseid ülevaateid veebiteenuse puhverserveri**ja tippige URL-i Port arv, näiteks **http://myproxy:80**.<br>  
    ![Toimingute halduri OMS puhverserveri aadressi](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Kui puhverserver nõuab autentimist mandaadi määramiseks
 Puhverserveri mandaat ja sätted vaja kajastuma hallatavad arvutid, mis annab OMS. Need serverid peaks olema *Microsoft System Center Advisor jälgimise serveri rühma*. Registris iga serveri jaotises krüptitud mandaat.

1. Avage Toiminguhalduri konsooli ja valige **Administreerimine** tööruumi.
2. Märkige jaotises **RunAs konfiguratsiooni** **profiilid**.
3. Avage **System Center Advisor käivitada nimega profiili puhverserveri** profiili.  
    ![System Center Advisor käivitada nimega puhverserveri profiili pilt](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Käivitage nimega profiili viisardis nuppu **Lisa** kontot nimega käivitada. Saate luua uue nimega käivitada konto või kasutada olemasolevat kontot. Selle konto peab olema läbi puhverserveri piisavaid õigusi.  
    ![Käivitage nimega profiili viisardi pilt](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Konto haldamise seadmiseks valige väljale Otsi objekti avamiseks **valitud klassi, rühma või objekti** .  
    ![Käivitage nimega profiili viisardi pilt](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Otsige ja valige siis **Microsoft süsteemi Center Advisor jälgimine serveri rühm**.  
    ![Objekti otsinguvälja pilt](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Klõpsake nuppu **OK** , et sulgeda käivitada nimega konto väljale lisa.  
    ![Käivitage nimega profiili viisardi pilt](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Viisardi lõpuleviimine ja salvestage soovitud muudatused.  
    ![Käivitage nimega profiili viisardi pilt](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Paketid laaditakse kinnitada, et OMS haldamine

Kui olete lisanud lahenduste OMS, saate vaadata nende Toiminguhalduri konsooli jaotises **Administreerimine**halduse paketid. *System Center Advisor* leiate need kiiresti üles otsida.  
    ![halduse paketid allalaaditud](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) või, võite vaadata ka jaoks OMS halduse paketid Toiminguhalduri management serveri Windows PowerShelli järgmise käsu abil:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Selle Toiminguhalduri kinnitamiseks saadab andmeid OMS teenusega

1. Toiminguhalduri management server, avage jõudluse monitori (perfmon.exe) ja valige **Jõudluse kuvar**.
2. Klõpsake nuppu **Lisa**ja valige **Seisund teenuse haldus rühmad**.
3. Lisage hinnale, mis algavad **HTTP**.  
    ![hinnale lisamine](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Kui teie Toiminguhalduri konfiguratsiooni on hea, kuvatakse tegevuse seisund Teenusehaldus hinnale sündmused ja muude andmete üksuste jaoks, alusel halduse paketid OMS ja konfigureeritud Logi saidikogumi poliitika lisatud.  
    ![Jõudluse jälgimine kuvav tegevus](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Järgmised sammud

- Funktsioonide lisamine ja andmete kogumine [lisamine Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md) .
- Saate tutvuda [log otsingud](log-analytics-log-searches.md) üksikasjaliku teabe kogutud lahendusi.
