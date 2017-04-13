<properties
    pageTitle="Streaming Azure'i diagnostika andmed sündmuse jaoturi kaudu saadaolevad tee | Microsoft Azure'i"
    description="Illustreerib, kuidas konfigureerida Azure diagnostika sündmuse jaoturi lõpuni, sealhulgas juhised levinud stsenaariumi."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Azure'i diagnostika andmete kuum tee streaming sündmuse jaoturi abil

Azure'i diagnostika pakub paindlik võimalust koguda mõõdikute ja logid cloud services virtuaalmasinates (VMs) ja Azure Storage tulemused üle. Alates ajavahemiku märts 2016 (SDK 2.9), saate valamu diagnostika täielikult kohandatud andmeallikatega ja kuum tee andmete edastamiseks sekundites [Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/)abil.

Toetatud andmetüübid on näiteks järgmised:

- Sündmuse jälgimine for Windows (ETW) sündmused
- Jõudluse hinnale
- Windowsi sündmuste logid
- Logid
- Azure'i diagnostika taristu logid

Selles artiklis kirjeldatakse, kuidas seadistada Azure diagnostika sündmuse jaoturi kaudu lõpuni. Juhised on mõeldud ka levinud järgmistel juhtudel.

- Kuidas kohandada logid ja saada sinked sündmuse jaoturi mõõdikud
- Kuidas muuta konfiguratsioone iga keskkonnas
- Kuidas vaadata jaoturi sündmuse voo andmed
- Ühenduse tõrkeotsing  

## <a name="prerequisites"></a>Eeltingimused

Sündmuse jaoturi hukku sisse Azure'i diagnostika toetatakse pilveteenustega, VMs, virtuaalse masina skaala komplektid ja teenuse struktuuri alates Azure'i SDK 2.9 ja vastavate Azure'i tööriistad Visual Studio.

- Azure'i diagnostika laiend 1,6 ([Azure'i SDK .NET 2.9 või uuem](https://azure.microsoft.com/downloads/) eesmärgid see vaikimisi)
- [Visual Studio 2013 või uuem versioon](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Azure'i diagnostika olemasolevaid konfiguratsioone rakenduses, kasutades *.wadcfgx* faili ja ühte järgmistest:
    - Visual Studio: [Azure'i pilveteenustega ja Virtuaalmasinates diagnostika konfigureerimine](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Windows PowerShelli: [lubamine diagnostika Azure'i Cloud Services PowerShelli abil](../cloud-services/cloud-services-diagnostics-powershell.md)
- Sündmuse jaoturi nimeruum ette valmistatud kohta artiklis, [sündmus jaoturi kasutamise alustamine](./event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Azure'i diagnostika ühenduse sündmuse jaoturi valamu

Azure'i diagnostika alati valamud logid ja mõõdikute, vaikimisi Azure Storage konto. Rakendus võib lisaks valamu sündmuse jaoturi, lisades uue jaotise **valamud** **WadCfg** elemendi *.wadcfgx* faili **PublicConfig** osas. Visual Studio *.wadcfgx* fail on salvestatud järgmises asukohas: **Pilveteenuste teenuse Project** > **rollid** >  **(RoleName)** > **diagnostics.wadcfgx** faili.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

Selles näites on seatud sündmuse jaoturi URL-i sündmuse jaoturi täielikult kvalifitseeritud nimeruum: sündmuse jaoturi nimeruum + "/" + sündmuse jaoturi nimi.  

Sündmuse jaoturi URL-i kuvatakse [Azure portaali](http://go.microsoft.com/fwlink/?LinkID=213885) armatuurlaual jaoturiga sündmus.  

**Valamu** nime saate määrata mis tahes kehtiv stringi, kui sama väärtuse kasutatakse korduvalt kogu config faili.

> [AZURE.NOTE]  Täiendavad valamud, nt *applicationInsights* konfigureerinud selle jaotise võib. Azure'i diagnostika võimaldab määratleda, kui iga valamu ka jaotise **PrivateConfig** ühe või mitme valamud.  

Sündmuse jaoturi valamu peate ka deklareeritud ja määratletud *.wadcfgx* config faili **PrivateConfig** osas.

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

Funktsiooni `SharedAccessKeyName` väärtus peab vastama ühiskasutusse Accessi allkirja (SAS) võti ja poliitika, mis on määratletud **Sündmuse jaoturi** nimeruumi. Liikuge jaoturi sündmuse armatuurlaua [Azure portaali](https://manage.windowsazure.com), klõpsake vahekaarti **konfigureerimine** ja häälestamine nimega poliitika (nt "SendRule"), mis sisaldab *saatmise* õigused. **StorageAccount** on ka deklareeritud **PrivateConfig**. Ei ole vaja siin väärtusi muuta, kui need töötavad. Selles näites me väärtused tühjaks jätta, mis on märk, et järgmise etapi varade määrab väärtused. Näiteks konfiguratsioonifail *ServiceConfiguration.Cloud.cscfg* keskkond, mis määrab keskkond, mis vastavad nimed ja võtmed.  

> [AZURE.WARNING] Sündmuse jaoturi SAS võti talletatakse *.wadcfgx* failis lihttekstina. Sageli see võti on sisse möllitud lähtekoodi jälgimine või on saadaval, kui vara 's koostamine nii, et peaks seda vastavalt vajadusele kaitsta. Soovitame kasutada SAS peamine õigustega *saatmine ainult* nii, et pahatahtlik kasutaja kirjutamise keskuses sündmus, kuid ei saa kuulata seda või seda hallata.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Azure'i diagnostika logid ja mõõdikute valamu koos sündmuse jaoturi konfigureerimine

Nagu varem, vaike- ja kohandatud diagnostika andmed, st mõõdikute ja logid, on automaatselt sinked Azure mäluruumi konfigureeritud intervalliga. Sündmuse jaoturi ja mis tahes täiendavaid valamu, saate määrata mis tahes root või lehtede sõlm hierarhia olema sinked sündmuse jaoturi abil. See hõlmab ETW sündmusi, jõudluse hinnale, Windows sündmuselogide ja logid.   

See on oluline silmas pidada, mitu andmepunkti tuleks üle tegelikult sündmuse jaoturi. Tavaliselt arendajatel üle latentsusajaga-tee andmed, mis tuleb consumed ja tõlgendada kiiresti. Et jälgimine teatiste või autoscale reeglite näited. Arendaja võib ka konfigureerimine mõne alternatiivse analüüsi poe või otsige poest – näiteks Azure'i voo Analytics, Elasticsearch, kohandatud süsteem või teised lemmik süsteem.

Järgnevalt mõned näide konfiguratsiooni.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

Järgmises näites, rakendatakse valamu vanema **PerformanceCounters** sõlme hierarhia, mis tähendab, et kõik lapse **PerformanceCounters** saadetakse sündmuse jaoturi.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

Eelmises näites, rakendatakse ainult kolme hinnale valamu: **Taotlused järjekorda**, **Tagasi lükata**ja **% protsessori aeg**.  

Järgmises näites on kujutatud, kuidas saate arendaja piirata saadetud andmete kriitiline mõõdikute, mida kasutatakse selle teenuse seisund.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

Selles näites valamu rakendatakse logid ja on filtreeritud ainult taseme Jälita viga.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Juurutada ja värskendada pilveteenustega rakendus ja diagnostika config

Visual Studio pakub lihtsaim tee sündmuse jaoturi valamu konfiguratsioon ja rakenduse juurutamine. Vaadata ja redigeerida faili, Visual Studios *.wadcfgx* faili avada, redigeerida ja salvestada. Tee on **Pilvepõhise teenuse Project** > **rollid** > **(RoleName)** > **diagnostics.wadcfgx**.  

Sel hetkel kõik kasutuselevõtu ja juurutamise värskendada toimingud Visual Studio Visual Studio Team süsteem ja kõik käsud või skriptide, mis põhinevad MSBuild ja kasutage soovitud **t: avaldamine** target pakendamine *.wadcfgx* kaasamine. Lisaks kasutuselevõttu ja värskenduste juurutada faili Azure'i vajaliku Azure'i diagnostika agent laienduse abil oma VMs.

Pärast juurutamist rakendus ja Azure diagnostika konfiguratsiooni, näete kohe tegevuse sündmuse jaoturi armatuurlaual. See näitab, kas olete valmis liikumiseks vaatamise-tee andmeid kuulajale kliendi või analüüsi tööriist oma valik.  

Sündmuse jaoturi armatuurlaua kuvatakse järgmisel joonisel terve saatmist diagnostika andmed sündmuse jaoturi alates millalgi pärast 11: 00. Siis rakendus on juurutatud värskendatud *.wadcfgx* faili ja valamu on õigesti konfigureeritud.

![][0]  

> [AZURE.NOTE] Kui teete Värskenduste Azure diagnostika config faili (.wadcfgx), on soovitatav push Visual Studio avaldamine või Windows PowerShelli skripti abil kogu rakenduse kui ka konfiguratsiooni värskendusi.  

## <a name="view-hot-path-data"></a>Andmete kuvamine-tee

Nagu varem, on palju kasutamine juhtumeid kuulata ja sündmuse jaoturi andmete töötlemiseks.

Ühe lihtsa lähenemine on väikese katse konsooli rakendus sündmuse jaoturi kuulata ja printida väljundi voo loomine. Saate lisada järgmine kood, mida on kirjeldatud täpsemalt [Alustamine sündmuse jaoturi](./event-hubs-csharp-ephcs-getstarted.md)), konsooli rakendus.  

Pange tähele, et konsooli rakendus peab sisaldama [sündmuse protsessor Host Nugeti pakett](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Ärge unustage nurksulgudesse **põhi** funktsiooni väärtuste asendamine väärtustega oma ressursid.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Sündmuse jaoturi valamu tõrkeotsing

- Sündmuse jaoturi ei kuvata sissetulevate ja väljaminevate sündmuse tegevuse ootuspäraselt.

    Kontrollige, et teie sündmuse jaoturi on ette valmistatud. Kõigi ühenduse teave jaotises **PrivateConfig** *.wadcfgx* peab vastama teie ressursside nagu näha portaalis väärtused. Veenduge, et teil on portaalis SAS poliitika määratletud ("SendRule" näites) ja, et *saada* on lubatud.  

- Pärast värskendust, sündmuse jaoturi enam näitab sissetulevate ja väljaminevate sündmus.

    Esmalt veenduge, et sündmuse jaoturi ja konfiguratsiooni teave on õige, nagu on selgitatud varem. Mõnikord on **PrivateConfig** lähtestada juurutamise värskendust. Soovitatav lahendus on muuta kõik *.wadcfgx* projekti ja seejärel vajutage täieliku rakenduse värskendus. Kui see pole võimalik, veenduge, et diagnostika värskenduse sunnib täieliku **PrivateConfig** , mis sisaldab SAS võti.  

- Olen proovinud soovitusi ja sündmuse jaoturi ikka ei tööta.

    Proovige otsida Azure Storage tabeli, mis sisaldab logid ja tõrgete Azure'i diagnostika ise: **WADDiagnosticInfrastructureLogsTable**. Üheks võimaluseks on kasutada näiteks [Azure'i salvestusruumi Explorer](http://www.storageexplorer.com) tööriist selle salvestusruumi kontoga ühendust luua, vaadata selle tabeli ja lisamine päringu ajatempli viimase 24 tunni jooksul. Saate CSV-faili eksportida ja avage see rakendus nagu Microsoft Exceli tööriist. Exceli abil on lihtne otsida helistaja-kaardi stringid, nagu **EventHubs**, mis viga on teatatud kuvamiseks.  

## <a name="next-steps"></a>Järgmised sammud

• [Lisateavet sündmuse jaoturi kohta](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Lisa: Azure'i täieliku diagnostika konfiguratsiooni faili (.wadcfgx) näide

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Selles näites komplementaarse *ServiceConfiguration.Cloud.cscfg* näeb välja järgmine.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
