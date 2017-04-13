<properties
    pageTitle="Azure'i diagnostika (.NET) kasutamine pilveteenustega | Microsoft Azure'i"
    description="Azure'i diagnostika abil andmete kogumine silumine, jõudluse, jälgimine, liikluse analüüsi ja Lisateavet Azure pilveteenustega."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure'i diagnostika Azure Cloud Services lubamine

Teemast [Azure diagnostika ülevaade](../azure-diagnostics.md) Azure'i diagnostika taust.


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Kuidas lubada diagnostika töötaja roll

See kõndida läbi kirjeldatakse, kuidas rakendada Azure töötaja rolli, mis eraldab telemeetria andmed, kasutades .NET EventSource klass. Azure'i diagnostika abil koguda andmeid koguda ja salvestada selle konto Azure salvestusruumi. Töötaja roll loomisel Visual Studio võimaldab automaatselt diagnostika 1.0 osana lahendus Azure'i SDK-d .NET 2.4 ja varasemate versioonidega. Järgmised juhised kirjeldavad töötaja roll, keelamise diagnostika 1.0 lahenduse juurutamisel diagnostika 1.2 või 1.3 teie töötaja rolli loomise protsess.

### <a name="pre-requisites"></a>Eeltingimused
Selles artiklis eeldatakse, et teil on Azure tellimus ja kasutate Azure SDK Visual Studio 2013. Kui teil on Azure'i tellimus, te saate registreeruda [Tasuta prooviversioon][]. Veenduge, et [installida ja konfigureerida Azure PowerShelli versiooni 0.8.7 või uuem versioon][].

### <a name="step-1-create-a-worker-role"></a>Samm 1: Loo töötaja roll
1.  Käivitage **Visual Studio 2013**.
2.  Looge uus **Azure'i pilveteenuses** projekt **Cloud** malli selle sihtkohtade .NET Framework 4.5.  Projekti "WadExample" nimi ja klõpsake nuppu Ok.
3.  Valige **Töötaja roll** ja klõpsake nuppu Ok. Projekti luuakse.
4.  **Solution Exploreris**, topeltklõpsake faili **WorkerRole1** atribuudid.
5.  **Konfiguratsiooni** vahekaardil tühjendada vastavaid ruute **Diagnostika lubamine** , keelamine diagnostika 1.0 (Azure'i SDK 2.4 ja eariler).
6.  Luua oma lahenduse kinnitamaks, et teil on tõrkeid.

### <a name="step-2-instrument-your-code"></a>Samm 2: Seadme oma kood
Asendage WorkerRole.cs sisu järgmine kood. Klassi SampleEventSourceWriter, päritud [EventSource klassi][]rakendab neli logimine: **SendEnums**, **MessageMethod**, **SetOther** ja **HighFreq**. Esimese parameetrina **WriteEvent** meetodiga määratleb vastava sündmuse ID-d. Ava_moodul rakendab silmuses, mis nõuab logimine meetodi rakendatud **SampleEventSourceWriter** tunni iga 10 sekundi järel.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
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

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

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

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Samm 3: Juurutada oma töötaja rolli
1.  Oma töötaja rolli Azure Visual Studio juurutada, valides **WadExample** projekti Solution Exploreris siis **Avalda** **koostada** menüüst.
2.  Valige oma tellimus.
3.  Dialoogiboksis **Microsoft Azure'i avaldamine sätted** valige **Loo uus …**.
4.  Sisestage dialoogi **loomine pilveteenuses ja salvestusruumi konto** **nimi** (nt "WadExample") ja valige piirkond või osaleja rühma.
5.  Seadke **lavastus** **keskkonnas** .
6.  Muuta muid **sätteid** vastavalt vajadusele ja klõpsake nuppu **Avalda**.
7.  Pärast lõpetamist juurutamise veenduge Azure klassikaline portaalis, et oma pilveteenuses on olekus **töötab** .

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Samm 4: Looge oma diagnostika konfiguratsioonifail ja install laiendamine
1.  Laadige avaliku konfiguratsiooni faili skeemi määratluse järgmist PowerShelli käsu:
2.
        (Get-AzureServiceAvailableExtension - ExtensionName 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Välja faili-kodeerimine utf8 - FilePath "WadConfig.xsd"

2.  XML-faili lisamine **WorkerRole1** projekti **WorkerRole1** projekti paremklõpsake ja valige **Lisa** -> **Uue üksuse...**  ->  **Visual C# üksuste** -> **andmete** -> **XML-fail**. "WadExample.xml" faili nimi.

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Juhis 5: Installi diagnostika töötaja kohta
PowerShelli cmdlet-käskude haldamise diagnostika veebis või töötaja rollile on: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension ja Eemalda-AzureServiceDiagnosticsExtension.

1.  Avage Azure'i PowerShelli.
2.  Käivitada skripti diagnostika installimiseks oma töötaja rolli (Asendage *StorageAccountKey* salvestusruumi konto võti wadexample salvestusruumi konto jaoks):

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Samm 6: Telemeetria andmete ülevaade
Visual Studio **Server Explorer** liikuge wadexample salvestusruumi konto. Pärast pilve teenus peab töötama umbes 5 minutit näete **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** ja **WADSetOtherTable**tabelid. Topeltklõpsake mõnda kogutud telemeetria vaadata tabelite.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Otsingukonfiguratsiooni faili skeemi

Konfiguratsioonifail diagnostika määratleb lähtestamine diagnostika konfiguratsioonisätted diagnostika agent käivitamisel kasutatavad väärtused. Vt [uusima skeemi viide](https://msdn.microsoft.com/library/azure/mt634524.aspx) kehtivad väärtused ja näited.

## <a name="troubleshooting"></a>Tõrkeotsing

Kui teil on probleeme, leiate abi levinumate probleemide [Tõrkeotsingu Azure'i diagnostika](../azure-diagnostics-troubleshooting.md) .

## <a name="next-steps"></a>Järgmised sammud
[Seotud virtuaalse masina loendi leiate teemast Azure diagnostika artikleid](azure-diagnostics.md#cloud-services) muuta andmete kogute, tõrkeotsing või Lisateavet diagnostika üldiselt.


[Klassi EventSource]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Tasuta prooviversioon]: http://azure.microsoft.com/pricing/free-trial/
[Installima ja konfigureerima Azure PowerShelli versiooni 0.8.7 või uuem versioon]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
