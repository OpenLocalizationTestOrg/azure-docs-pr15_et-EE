<properties
   pageTitle="Azure'i log integreerimine FAQ | Microsoft Azure'i"
   description="Sellest artiklist leiate vastused küsimustele integreerimine Azure log."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Azure'i log integreerimine korduma kippuvad küsimused (KKK)

Sellest artiklist leiate vastused küsimused Azure log integreerimine teenus, mis võimaldab teil integreerida kohapealse Turbeteave ja sündmuse Management (SIEM) süsteemid töötlemata logid oma Azure ressursse. Selle integreerimine pakub ühendatud armatuurlaua oma varasid, kohapealse või pilves, nii, et saate liitmine, oleksid, analüüsida ja Teavita seostatud rakenduste turvalisus sündmuste jaoks.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Kuidas vaadata salvestusruumi kontod, millest integreerimine Azure log tõmbab Azure VM logid kaudu?

Käivitage käsk **azlog lähteloendis**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Kuidas värskendada puhverserveri konfigureerimine?

Kui teie puhverserveri Luba juurdepääs Azure'i otse, avage **AZLOG. EXE. OTSINGUKONFIGURATSIOONI** faili **c:\Program Files\Microsoft Azure'i Log integreerimine**. Faili kaasamiseks **defaultProxy** jaotise puhverserveri aadressi teie ettevõtte värskendada. Pärast värskendamine on lõpule jõudnud, lõpetamine ja käsud **Lõpeta azlog** ja **net start azlog**abil teenuse käivitamine.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Kuidas vaadata Windowsi sündmuste tellimuse teave?

Lisab **subscriptionid** allika lisamisel sõbralik nimi.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Sündmuse XML-i on metaandmeid, nagu on näidatud allpool, sh tellimuse id.

![Sündmuse XML-i][1]

## <a name="error-messages"></a>Tõrketeated

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Kui käsk **azlog createazureid**, miks kuvada järgmine tõrketeade?

Tõrge:

  *Ei saanud luua AAD rakendus – rentniku 72f988bf-86f1-41af-91ab-2d7cd011db37-põhjus = 'Keelatud' – sõnumi = "Toimingu lõpuleviimiseks piisavaid õigusi."*

**Azlog createazureid** püüab loomine teenuse põhilise Azure AD rentnikud tellimused, mille Azure login on juurdepääs. Kui teie Azure sisselogimise Külastajate kasutaja selle Azure AD rentniku, siis käsk nurjub "Toimingu lõpuleviimiseks piisavaid õigusi." Koosolekukutse rentnikuadministraatori rentniku kasutaja konto lisamiseks.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Kui käsk **azlog lubada**, miks kuvada järgmine tõrketeade?

Tõrge:

  *Hoiatus loomise Rollimäärang - AuthorizationFailed: kliendi janedo@microsoft.com' objekti id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' ei ole 'Microsoft.Authorization/roleAssignments/write' toimingu sooritamiseks autoriseerimine üle ulatus "/ tellimuste/70 d 95299-d689-4c 97-b971-0d8ff0000000".*

**Azlog lubada** käsk määrab roll lugeja Azure AD teenusega põhisumma (loodud **Azlog createazureid**) esitatud tellimused. Kui Azure login pole koostöö administraator või tellimuse omanik, nurjub, kuvatakse tõrketeade "autoriseerimine nurjus. Selle toimingu lõpuleviimiseks on vaja Azure Rollipõhine pääsu juhtimine (RBAC) koostöö administraator või omanik.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Kust leida atribuutide määratluse auditilogi?

Järgmistest teemadest.

- [Auditi toimingud koos ressursihaldur](../resource-group-audit.md)
- [Loendi tellimuse Azure'i kuvari REST API halduse sündmused](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Kui leiate Azure'i turbekeskus teatised üksikasjad?

Vt [haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Kuidas muuta, mis on kogutud VM diagnostika?

Vaadake teemat [PowerShelli kasutamine lubamiseks klõpsake töötab Windows Azure'i diagnostika](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) täpsemat teavet saada, muutmine, ja määrata *(WAD)* Windows Azure'i diagnostika konfigureerimine. Järgmine on näide:

### <a name="get-the-wad-config"></a>Saan WAD config

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>WAD Config muutmine

Järgmises näites on konfiguratsioon, kus sündmus on kogutud ainult EventID 4624 ja EventId 4625. Microsoft Antimalware sündmused on kogutud süsteemi sündmuste logi. Vt [tarbimine sündmuste] (XPath avaldiste kasutamise kohta lisateavet https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85).

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Selle konfiguratsiooni seadmine

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Pärast muudatuste tegemist, märkige ruut õige sündmuste kogumise talletusmahu konto.

Kui teil on küsimusi integreerimine Azure Log, saatke meile e-posti [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
