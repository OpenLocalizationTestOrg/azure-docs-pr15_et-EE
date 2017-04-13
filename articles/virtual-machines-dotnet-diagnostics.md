<properties
    pageTitle="Kuidas kasutada diagnostika Azure'i virtuaalmasinates | Microsoft Azure'i"
    description="Azure'i diagnostika abil andmete kogumine Azure'i Virtuaalmasinates silumine ja jõudluse, jälgimine, liikluse analüüsi ja muu jaoks."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Azure'i virtuaalmasinates diagnostika lubamine

Teemast [Azure diagnostika ülevaade](azure-diagnostics.md) Azure'i diagnostika taust.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Kuidas lubada diagnostika Virtual kohapeal

See kõndida läbi kirjeldatakse, kuidas arengu arvutist diagnostika installimiseks on Azure virtuaalse masina kaugühenduse teel. Saate teada, kuidas rakendada rakendus, mis töötab selle Azure virtuaalse masina ja eraldab telemeetria andmed, kasutades .net-i [EventSource klassi][]. Azure'i diagnostika kasutatakse telemeetria koguda ja salvestada selle konto Azure salvestusruumi.

### <a name="pre-requisites"></a>Eeltingimused
See kõndida läbi eeldab, et teil on Azure tellimus ja kasutate Azure SDK Visual Studio 2013. Kui teil on Azure'i tellimus, te saate registreeruda [Tasuta prooviversioon][]. Veenduge, et [installida ja konfigureerida Azure PowerShelli versiooni 0.8.7 või uuem versioon][].

### <a name="step-1-create-a-virtual-machine"></a>Samm 1: Luua virtuaalse masina
1.  Käivitage arvutis arengu, Visual Studio 2013.
2.  Visual Studio **Server Explorer** laiendamine **Azure**, paremklõpsake **Virtuaalmasinates** siis valige **virtuaalse masina loomine**.
3.  Valige dialoogiboksis **Valige tellimuse** Azure tellimuse ja klõpsake nuppu **edasi**.
4.  Valige dialoogiboksis **Valige virtuaalse masina pilt** **Windows Server 2012 R2 andmekeskuses, November 2014** ja klõpsake nuppu **edasi**.
5.  Määrake **Virtuaalse masina põhisätted**virtuaalse masina nime "wadexample". Määrake oma administraatori kasutajanime ja parooli ja klõpsake nuppu **edasi**.
6.  **Pilveteenuse Teenusesätted** dialoogiboksis luua uue pilveteenus nimega "wadexampleVM". Looge uus salvestusruumi konto nimega "wadexample" ja klõpsake nuppu **edasi**.
7.  Klõpsake nuppu **Loo**.

### <a name="step-2-create-your-application"></a>Samm 2: Looge rakenduse
1.  Käivitage arvutis arengu Visual Studio 2013.
2.  Looge uus Visual C# konsooli rakendus, mis on suunatud .NET Framework 4.5. Projekti "WadExampleVM" nime.
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  Asendage Program.cs sisu järgmine kood. Klassi **SampleEventSourceWriter** rakendab neli logimine: **SendEnums**, **MessageMethod**, **SetOther** ja **HighFreq**. Esimese parameetrina WriteEvent meetodiga määratleb vastava sündmuse ID-d. Ava_moodul rakendab silmuses, mis nõuab logimine meetodi rakendatud **SampleEventSourceWriter** tunni iga 10 sekundi järel.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
        {
        sealed class SampleEventSourceWriter : EventSource
        {
            public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
            public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
            public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
            public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
            public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

        }

        enum MyColor
        {
            Red,
            Blue,
            Green
        }

        [Flags]
        enum MyFlags
        {
            Flag1 = 1,
            Flag2 = 2,
            Flag3 = 4
        }

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }

        }
        }
        }


4.  Salvestage fail ja valige **Lahenduse luua** **koostada** menüü koostamiseks oma kood.


### <a name="step-3-deploy-your-application"></a>Samm 3: Juurutada rakendust
1.  Paremklõpsake **Solution** Exploreris **WadExampleVM** projekti ja valige **File Exploreris kausta avamine**.
2.  Liikuge kausta, *bin\Debug* ja kopeerige kõik failid (WadExampleVM.*)
3.  **Server** Explorer virtuaalse masina paremklõpsake ja valige **Kaugtöölaua ühendus kasutamise**.
4.  Pärast ühendatud VM looge kaust nimega WadExampleVM ja kleebi rakenduse failid kausta.
5.  Käivitage rakendus WadExampleVM.exe. Peaksite nägema tühja konsooli aken.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Samm 4: Looge oma konfiguratsioon diagnostika ja installige laiendamine
1.  Avaliku konfiguratsiooni faili skeemi määratluse arengu arvutisse allalaadimiseks täites PowerShelli käsk:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Avage Visual Studios, kas teil on juba avatud või Visual Studio näiteks projekti alustamine pole avatud projektide uut XML-faili. Visual Studio, valige käsk **Lisa** -> **Uue üksuse...**  ->  **Visual C# üksuste** -> **andmete** -> **XML-fail**. "WadExample.xml" faili nimi
3.  Konfiguratsioonifail on WadConfig.xsd seostada. Veenduge, et WadExample.xml Editori aknas on aktiivse akna. Vajutage klahvi **F4** , et avada aken **Atribuudid** . Klõpsake aknas **Atribuudid** atribuudi **skeemid** . Klõpsake **…** atribuudi **skeemid** . Klõpsake soovitud **Lisa...** nupp ja liikuge asukohta, kuhu salvestasite XSD-faili ja valige fail WadConfig.xsd. Klõpsake nuppu **OK**.
4.  Asendage WadExample.xml konfiguratsiooni faili sisu järgmine XML-i ja salvestage fail. Konfiguratsioonifail määratleb paari jõudluse hinnale kogumiseks: üks CPU kasutuse ja teine mälu kasutamine. Seejärel konfiguratsiooni määratleb vastav meetodite SampleEventSourceWriter tunni nelja sündmused.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Juhis 5: Eemalt installida diagnostika sisse oma Azure virtuaalse masina
PowerShelli cmdlet-käskude haldamise VM diagnostika on: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension ja Eemalda-AzureVMDiagnosticsExtension.

1.  Avage oma arvutis arendaja Azure PowerShelli.
2.  Käivitada skripti eemalt installida diagnostika oma VM (Asenda *StorageAccountKey* salvestusruumi konto võti wadexamplevm salvestusruumi konto jaoks):

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Samm 6: Telemeetria andmete ülevaade
Visual Studio **Server Explorer** liikuge wadexample salvestusruumi konto. Pärast VM peab töötama umbes 5 minutit peaksite nägema **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** ja **WADSetOtherTable**tabelid. Topeltklõpsake mõnda kogutud telemeetria vaadata tabelite.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Otsingukonfiguratsiooni faili skeemi

Konfiguratsioonifail diagnostika määratleb lähtestamine diagnostika konfiguratsioonisätted diagnostika agent käivitamisel kasutatavad väärtused. Vt [uusima skeemi viide](https://msdn.microsoft.com/library/azure/mt634524.aspx) kehtivad väärtused ja näited.

## <a name="troubleshooting"></a>Tõrkeotsing

Lisateavet leiate [Azure'i diagnostika tõrkeotsing](azure-diagnostics-troubleshooting.md) .


## <a name="next-steps"></a>Järgmised sammud
[Seotud virtuaalse masina loendi leiate teemast Azure diagnostika artikleid](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) muuta andmete kogute, tõrkeotsing või Lisateavet diagnostika üldiselt.


[Klassi EventSource]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Tasuta prooviversioon]: http://azure.microsoft.com/pricing/free-trial/
[Installima ja konfigureerima Azure PowerShelli versiooni 0.8.7 või uuem versioon]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
