<properties
    pageTitle="PowerShelli kasutamine lubamiseks klõpsake töötab Windows Azure'i diagnostika | Microsoft Azure'i"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Saate teada, kuidas lubada Azure diagnostika töötab Windows PowerShelli kasutamine"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Lubada Azure diagnostika töötab Windows PowerShelli kasutamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure'i diagnostika on võimalus Azure'is, mis võimaldab Diagnostikaandmete juurutatud rakendus kogum. Diagnostika laiend abil saate näiteks logid või jõudluse hinnale Diagnostikaandmete kogumine Azure'i virtuaalarvuti (VM), kus töötab Windows. Selles artiklis kirjeldatakse, kuidas kasutada Windows PowerShelli VM diagnostika pikendamise lubamine. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) jaoks eeltingimused, mis on vaja seda artiklit.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Luba diagnostika laiend, kui ressursihaldur juurutamise mudeli kasutamine

Kui loote VM Windows Azure'i ressursihaldur juurutamise mudeli kaudu, lisades laiend konfiguratsiooni ressursihaldur malli, saate lubada diagnostika laiend. Lugege teemat [loomine Windowsi virtuaalse masina jälgida ja diagnostika Azure'i ressursihaldur malli abil](virtual-machines-windows-extensions-diagnostics-template.md).

Diagnostika laiend olemasoleva VM, läbi ressursihaldur juurutamise mudeli loodud lubamiseks saate [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) PowerShelli cmdlet-käsk, nagu allpool näidatud.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* on fail, mis sisaldab diagnostika konfiguratsiooni XML, nagu on kirjeldatud allpool [valimi](#sample-diagnostics-configuration) tee.  

Kui diagnostika konfiguratsioonifail määrab **StorageAccount** elemendi salvestusruumi konto nime, siis *Set-AzureRMVMDiagnosticsExtension* skripti automaatselt määrata diagnostika laiend saata Diagnostikaandmete salvestusruumi konto. Seda teha, peab olema sama nimega VM tellimuse salvestusruumi konto.

Kui diagnostika konfiguratsiooni **StorageAccount** pole määratud, siis peate parameetrit *StorageAccountName* cmdlet edasi. Kui parameeter *StorageAccountName* on määratud, siis cmdlet alati kasutatakse salvestusruumi konto, mis on määratud parameetri ja mitte diagnostika konfiguratsioonifailis määratud üks.

Kui diagnostika salvestusruumi konto on mõni muu tellimus: VM, siis peate selgesõnaliselt edasi *StorageAccountName* ja *StorageAccountKey* parameetrite cmdlet-i. Kui diagnostika salvestusruumi konto on sama tellimuse cmdlet automaatselt päringu ning seada võtmeväärtuse, kui diagnostika pikendamise lubamine *StorageAccountKey* parameetri pole vaja. Juhul, kui diagnostika salvestusruumi konto on mõni muu tellimus, seejärel cmdlet ei pruugi saada võti automaatselt ja peate määrata võti *StorageAccountKey* parameetri kaudu.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Kui diagnostika pikendamine on sisse lülitatud VM, saad praeguste sätete [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) cmdleti abil.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Selle cmdlet-käsk tagastab *PublicSettings*, mis sisaldab XML-i konfigureerimine Base64-kodeeringuga vormingus. XML-i lugemiseks peate dekodeerida.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

[Eemalda-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) cmdlet-käsk saab eemaldada diagnostika laiend VM.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Luba diagnostika laiend, kui klassikaline juurutamise mudeli kasutamine

Cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) abil saate diagnostika laiendamiseks klõpsake klassikaline juurutamise mudeli kaudu loodavate VM. Järgmises näites on kujutatud uus VM läbi klassikaline juurutamise mudeli lubatud diagnostika laiendiga loomise kohta.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Diagnostika laiend olemasoleva VM, klassikaline juurutamise mudeli kaudu loodud lubamiseks esmalt kasutada [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) cmdlet VM konfiguratsiooni hankimine. Seejärel värskendage VM konfiguratsiooni kaasata diagnostika laiend cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) abil. Lõpuks rakendada värskendatud konfiguratsiooni VM [Värskendus-AzureVM](https://msdn.microsoft.com/library/mt589121.aspx)abil.

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Valimi diagnostika konfigureerimine

Järgmine XML-i saab kasutada diagnostika avaliku konfiguratsiooni ülaltoodud skriptide abil. Selle konfiguratsiooni valimi edastame erinevate jõudluse hinnale diagnostika salvestusruumi konto koos vigade rakenduse, Turve ja sündmuselogide Windows system kanalid ja vigu: diagnostika taristu logid.

Konfiguratsiooni tuleb värskendada on järgmised:

- Atribuut *ResourceIdkasutamisel* **mõõdikute** elemendi vaja värskendada VM ressursi ID-ga.
    - Ressursi ID-d saab ehitada, kasutades järgmist mustrit: "/ tellimuste / {*Tellimuse ID tellimuse koos VM*} /resourceGroups/ {*resourcegroup nimi VM*} / {*VM nime*} providers/Microsoft.Compute/virtualMachines/".
    - Näiteks kui tellimuse ID, kus töötab VM tellimuse **11111111-1111-1111-1111-111111111111**, ressursirühma rühma ressursinimi on **MyResourceGroup**, ja VM nimi on **MyWindowsVM**, siis oleks *ResourceIdkasutamisel* väärtus:

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Lisateavet selle kohta, kuidas luuakse mõõdikute põhjal jõudluse hinnale ja mõõdikute konfiguratsiooni, leiate [Azure'i diagnostika mõõdikute tabeli salvestusruumi](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage).

- **StorageAccount** elemendi peab värskendatakse diagnostika salvestusruumi konto nimi.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Järgmised sammud
- Kasutamise võimalus Azure'i diagnostika- ja teiste meetodite abil seotud probleemide tõrkeotsinguks täiendavad juhised leiate teemast [Azure pilveteenustega ja Virtuaalmasinates diagnostika lubamine](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Diagnostika konfiguratsioone skeem](https://msdn.microsoft.com/library/azure/mt634524.aspx) selgitab erinevate XML-i konfiguratsioone suvandite diagnostika pikendamise eest.
