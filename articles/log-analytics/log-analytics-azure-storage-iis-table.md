<properties
    pageTitle="Sündmuste IIS-i ja tabeli talletamist bloobimälu kasutamise | Microsoft Azure'i"
    description="Log Analytics saate lugeda kirjutamine diagnostika abil tabelimälu Azure teenuste logid või IIS-i logid Bloobivahemälu salvestusruumi kirjutada."
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
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Kasutades bloobimälu IIS ja tabelimälu sündmuste jaoks.

Log Analytics saab lugeda, mis kirjutamine diagnostika abil tabelimälu teenuse logid või IIS-i logid Bloobivahemälu salvestusruumi kirjutada:

+ Teenuse struktuuri kogumite (eelvaade)
+ Virtuaalmasinates
+ Web/töötaja rollid

Enne Log Analytics saate nende ressursside otsinguväli, peab olema lubatud Azure diagnostika.

Kui diagnostika on lubatud, saate kasutada Azure portaali või PowerShelli konfigureerimine Log Analytics kogumiseks logid.

Azure'i diagnostika on Azure laiend, mis võimaldab koguda Diagnostikaandmete töötaja roll, web rolli või Azure töötab. Andmed salvestatakse Azure storage konto ja seejärel kogutakse Log Analytics.

Log Analytics need Azure'i diagnostika logid kogumiseks, peab olema logid järgmisest kohast:

| Logi tüüp | Ressursi tüüp | Asukoht |
| -------- | ------------- | -------- |
| IIS-i logid | Virtuaalmasinates <br> Web rollid <br> Töötaja rollid | wad-iis-logfiles (bloobimälu) |
| Logi | Virtuaalmasinates | LinuxsyslogVer2v0 (tabel salvestusruumi) |
| Teenuse struktuuri funktsionaalseid sündmused | Teenuse struktuuri sõlmed | WADServiceFabricSystemEventTable |
| Teenuse struktuuri usaldusväärne näitleja sündmused | Teenuse struktuuri sõlmed | WADServiceFabricReliableActorEventTable |
| Teenuse struktuuri töökindlat sündmused | Teenuse struktuuri sõlmed | WADServiceFabricReliableServiceEventTable |
| Windowsi sündmuste logid | Teenuse struktuuri sõlmed <br> Virtuaalmasinates <br> Web rollid <br> Töötaja rollid | WADWindowsEventLogsTable (Tabelimälu) |
| Windows ETW logid | Teenuse struktuuri sõlmed <br> Virtuaalmasinates <br> Web rollid <br> Töötaja rollid | WADETWEventTable (Tabelimälu) |

>[AZURE.NOTE] IIS-i logid Azure veebisaitide praegu ei toetata.

Teil on virtuaalmasinates, installimise [Log Analytics agent](log-analytics-azure-vm-extension.md) sisse arvuti virtuaalne võimalus lubada täiendavate ülevaateid. Lisaks on võimalik analüüsida IIS-i logid sündmuselogide, saate teha konfigureerimine muutuste jälituse, SQL-i assessment ja Värskenda assessment täiendav analüüs.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Luba Azure diagnostika virtual kohapeal jaoks sündmuste logi ja IIS-i Logi saidikogumi

Järgmiste toimingute abil saate lubada Azure diagnostika virtual kohapeal Microsoft Azure'i portaalis sündmuste logi ja IIS-i Logi saidikogumi jaoks.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Azure'i diagnostika virtual kohapeal Azure'i portaalis lubamiseks

1. Installige VM Agent virtuaalse masina loomisel. Kui virtuaalse masina juba olemas, veenduge, et VM Agent on juba installitud.
    - Azure'i portaalis, liikuge virtuaalse masina, valige **Valikuline konfigureerimine**ja seejärel **diagnostika** ja määratud **oleku** **kohta**.

    Pärast lõpetamist, on VM Azure diagnostika laiend installitud ja töötab. Sellele laiendile vastutab teie diagnostika andmete kogumine.

2. Jälgimise lubamine ja konfigureerimine sündmuste logimise sisse mõne olemasoleva VM. Saate lubada diagnostika VM tasemel. Diagnostika lubada ja konfigureerida sündmuste logimise, tehke järgmist.
    1. Valige VM.
    2. Klõpsake nuppu **kontrolli**.
    3. Klõpsake nuppu **diagnostika**.
    4. Määrake **olek** **sees**.
    5. Märkige iga diagnostika log, mida soovite koguda.
    7. Klõpsake nuppu **OK**.

Azure'i PowerShelli abil saate määrata täpsemalt sündmused, mis on kirjutatud Azure Storage. [Azure'i tabelimälu või IIS-i logid Bloobivahemälu kirjutada kirjutada diagnostika abil andmete](log-analytics-azure-storage-json.md)kogumine viidata.


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Azure'i diagnostika Web roll IIS-i Logi ja sündmuse kogumi lubamine

Vaadake [Kuidas lubada diagnostika pilveteenus](../cloud-services/cloud-services-dotnet-diagnostics.md) üldised juhised Azure diagnostika lubamise kohta. Järgmised juhised seda teavet ja Log analüüsi jaoks kohandada.

Azure'i diagnostika lubatud:

- IIS-i logid salvestatakse vaikimisi Logi andmetega üle scheduledTransferPeriod edastamine ajavahemiku tagant.
- Vaikimisi Windows sündmuselogide ei edastata.

### <a name="to-enable-diagnostics"></a>Kui soovite lubada diagnostika

Windowsi sündmuselogide lubamiseks või muuta selle scheduledTransferPeriod konfigureerida Azure diagnostika abil XML-i konfiguratsioonifail (diagnostics.wadcfg), nagu on näidatud [Samm 4: Looge oma diagnostika konfiguratsioonifail ja installige laiendamine](../cloud-services/cloud-services-dotnet-diagnostics.md)

Järgmises näites konfiguratsioonifail kogub IIS-i logid ja kõik sündmused rakenduse ja logid:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Veenduge, et teie ConfigurationSettings määrab salvestusruumi konto, nagu järgmises näites:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Azure'i portaali salvestusruumi konto armatuurlaua, klõpsake jaotises Halda kiirklahvide leidub **account_name** ja **AccountKey** väärtused. Ühendusstringi protokolli peab olema **https**.

Kui värskendatud diagnostika konfiguratsiooni rakendatakse oma pilveteenusesse ja selle kirjutamise diagnostika Azure Storage, siis olete valmis Log Analytics konfigureerimiseks.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Logide kogumine Azure Storage Azure portaali abil

Azure portaali abil saate konfigureerida Log Analytics kogumiseks Azure teenuse logid:

+ Teenuse struktuuri kogumite
+ Virtuaalmasinates
+ Web/töötaja rollid

Azure'i portaalis, liikuge oma Log Analytics tööruumi ja teha järgmisi toiminguid:

1. Klõpsake *salvestusruumi kontod logid*
2. Klõpsake nuppu *Lisa* ülesanne
3. Valige salvestusruumi konto, mis sisaldab diagnostika logid
  - Selle konto võib olla klassikaline salvestusruumi konto või konto Azure ressursihaldur salvestusruum
4. Valige soovite koguda logisid andmetüüp
  - Valikud on IIS-i logid; Sündmusi; Logi (Linux); ETW logid; Teenuse struktuuri sündmused
5. Andmeallika väärtus lisatakse automaatselt andmete tüübist ja ei saa muuta
6. Konfiguratsiooni salvestamiseks klõpsake nuppu OK

Korrake juhiseid 2 – 6 täiendavat salvestusruumi kontod ja andmetüüpi, mida soovite Logi Analytics kogumiseks.

30 minuti jooksul, teil on võimalik salvestusruumi kontolt Log Analytics andmete kuvamiseks. Kuvatakse ainult andmeid, mis on kirjutatud salvestusruumi pärast konfiguratsiooni rakendatakse. Log Analytics ei loe olemasolevate andmete salvestusruumi konto kaudu.

>[AZURE.NOTE] Portaali Valideeri allikas on olemas salvestusruumi konto või kui uued andmed on kirjutatud.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Luba Azure diagnostika virtual kohapeal jaoks sündmuste logi ja IIS-i Logi saidikogumi PowerShelli abil

Kasutage juhiseid [Konfigureerimine Log Analytics registrisse Azure diagnostika](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) PowerShelli abil lugemise Azure diagnostika, mis on kirjutatud tabelimälu.

Azure'i PowerShelli abil saate määrata täpsemalt sündmused, mis on kirjutatud Azure Storage.
Lisateavet leiate teemast [Azure'i Virtuaalmasinates diagnostika lubamine](../virtual-machines-dotnet-diagnostics.md).

Saate lubada ja Azure diagnostika järgmist PowerShelli skripti abil värskendada.
Saate selle skripti kohandatud logimine konfiguratsioon.
Muuta skripti salvestusruumi konto, teenuse nimi ja virtuaalse masina nimi.
Skript kasutab klassikaline virtuaalmasinates cmdlet-käsud.

Vaadake üle järgmised skripti valimi, kopeerige see, seda vastavalt vajadusele muuta, proovi PowerShelli skripti failina salvestada ja seejärel käivitage skript.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Järgmised sammud

- Lugeda logisid Azure'i [bloobimälu failide kasutamine JSON](log-analytics-azure-storage-json.md) teenuste selle kirjutamine diagnostika bloobimälu JSON-vormingus.
- [Luba lahenduste](log-analytics-add-solutions.md) annab ülevaate andmed.
- [Otsingu päringute kasutamine](log-analytics-log-searches.md) andmete analüüsimiseks.
