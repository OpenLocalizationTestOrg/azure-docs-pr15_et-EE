<properties
    pageTitle="Azure'i funktsioonide tootearendusmaterjal | Microsoft Azure'i"
    description="Mõista, kuidas töötada Azure'i funktsioonide abil C#."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure'i funktsioone, funktsioonide, event töötlus, webhooks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure'i funktsioonide C# tootearendusmaterjal

> [AZURE.SELECTOR]
- [C# skript](../articles/azure-functions/functions-reference-csharp.md)
- [F # skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
Azure'i funktsioonide C# kogemus põhineb Azure'i WebJobs SDK. Andmete voo teie C# funktsiooni argumendid meetodi kaudu. Argumentide nimed on määratud `function.json`, ja seal on eelmääratletud nimede funktsioon puuraidur ja tühistamise sõned, näiteks juurdepääs.

Selles artiklis eeldatakse, et olete juba lugenud [Azure'i funktsioonide tootearendusmaterjal](functions-reference.md).

## <a name="how-csx-works"></a>.Csx tööpõhimõte

Funktsiooni `.csx` vorming võimaldab vähem "trafarett" kirjutamine ja keskenduda kirjutamine lihtsalt C# funktsiooni. Azure'i funktsioone, saate just kaasata kõik komplekti viited ja nimeruumid teil on vaja üles üleval, nagu tavaliselt ja asemel mähkimine kõik nimeruum ja tunni, saate lihtsalt määratleda oma `Run` meetod. Kui soovite kaasata kõik tunnid, näiteks POCO objektide määratlemiseks võib sisaldada ainekursuse sama faili sees.

## <a name="binding-to-arguments"></a>Argumendid sidumine

Erinevate sidumiste on seotud C# funktsiooni kaudu soovitud `name` atribuudi *function.json* konfiguratsiooni. Iga sidumine on eraldi toetatud andmetüübid, mis on kohta sidumine; Näiteks saate bloobimälu päästik toetada stringi, on POCO või mitut muud tüüpi. Saate kasutada tüüp, mis sobib kõige paremini teie vajadustele. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Logimine

Logige oma streaming logid C# väljundi, saate lisada mõne `TraceWriter` tipitud argument. Soovitame, et te nimi on `log`. Soovitame vältida `Console.Write` Azure funktsioone.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asünkroonse

Funktsiooni asünkroonne, kasutage funktsiooni `async` märksõna ja saatja on `Task` objekti.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Luba tühistamine

Teatud juhtudel võib olla toiminguid, mis on tundliku suletakse. Samal ajal, kui see on alati kõige paremini kirjutada koodi, mis saavad hakkama krahh, kui soovite graatsiline sulgumist käsitlema taotleb, määratlege soovitud [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) tipitud argument.  A `CancellationToken` antakse käivitab host sulgumist. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Importimise nimeruumid

Kui soovite importida nimeruumid, saate teha nii nagu tavaliselt, kus on `using` klausel.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Järgmised nimeruumid imporditakse automaatselt ja seetõttu on valikulised.

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Välise assemblereid viitamine

Raamistiku assemblereid, lisage viited, kasutades funktsiooni `#r "AssemblyName"` direktiivi.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Järgmised assemblereid lisatakse automaatselt majutuskeskkond Azure funktsioone:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Lisaks on järgmine assemblereid teisiti ääretule ja võib osutada simplename (nt `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Kui teil on vaja viidata privaatne komplekti, saate üles laadida komplekti faili lisamine `bin` funktsioon ja viide selle abil faili nimi (nt kausta  `#r "MyAssembly.dll"`). Teavet selle kohta, kuidas funktsioon kausta failide üleslaadimine, lugege järgmist jaotist pakettide.

## <a name="package-management"></a>Paketi haldus

Funktsiooni C# Nugeti pakettide kasutamiseks üles *project.json* faili selle funktsiooni kausta funktsioon rakenduse failisüsteemis. Siin on näide *project.json* faili, mis lisab Microsoft.ProjectOxford.Face versiooni 1.1.0 viide:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Toetatakse ainult .NET Frameworki 4.6, seega veenduge, et *project.json* faili saate määrata `net46` nagu siin näidatud.

Kui laadite faili *project.json* , runtime saab paketid ja lisab automaatselt paketi assemblereid viited. Te ei ole vaja lisada `#r "AssemblyName"` suuniseid. Lisage lihtsalt nõutav `using` laused *run.csx* faili määratletud Nugeti pakette kasutama.


### <a name="how-to-upload-a-projectjson-file"></a>Kuidas project.json faili üleslaadimine

1. Alustage kontrollite rakenduse funktsioon töötab, mida saate teha, avades oma funktsioon Azure'i portaalis. 

    See ka annab juurdepääsu streaming logid kus paketi installi väljund kuvatakse. 

2. Project.json faili üleslaadimiseks kasutada ühel [Azure'i funktsioonide arendaja viide teema](functions-reference.md#fileupdate) **värskendamine funktsioon rakenduse failid** jaotises kirjeldatud viisil. 

3. Pärast *project.json* fail on üles laaditud, näete, nagu järgmises näites oma funktsiooni väljundi kasutaja streaming log:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261 
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189 
2016-04-04T19:02:57.189 
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Keskkonna muutujad

Mõne keskkonnas kui ka rakenduse väärtuse seadmine saamiseks kasutage `System.Environment.GetEnvironmentVariable`, nagu on näidatud järgmises näites kood:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Diagrammide korduvkasutamine .csx kood

Saate kasutada tunnid ja muude *.csx* failide *run.csx* failis meetodite. Selleks, et kasutada `#load` suuniseid *run.csx* faili, nagu on näidatud järgmises näites.

Näide *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Näide *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Suhteline tee, mille saate kasutada funktsiooni `#load` direktiivi:

* `#load "mylogger.csx"`laadib funktsioon kaustas faili.

* `#load "loadedfiles\mylogger.csx"`laadib faili kaustas funktsioon kausta.

* `#load "..\shared\mylogger.csx"`laadib fail asub kaustas funktsioon kausta, mis on samal tasemel otse *wwwroot*.
 
Funktsiooni `#load` direktiiv töötab *.csx* (C# skript) failidega, mitte *.cs* failidega. 

## <a name="next-steps"></a>Järgmised sammud

Lisateavet järgmistest teemadest:

* [Azure'i funktsioonide tootearendusmaterjal](functions-reference.md)
* [Azure'i funktsioonide F # tootearendusmaterjal](functions-reference-fsharp.md)
* [Azure'i funktsioonide NodeJS tootearendusmaterjal](functions-reference-node.md)
* [Azure'i funktsioonide päästikute ja seosed](functions-triggers-bindings.md)

