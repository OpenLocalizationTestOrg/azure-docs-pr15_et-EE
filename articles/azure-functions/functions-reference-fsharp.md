<properties
    pageTitle="Azure'i funktsioonide F # tootearendusmaterjal | Microsoft Azure'i"
    description="Mõista, kuidas töötada Azure'i funktsioonide abil F #."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure'i funktsioone, funktsioonide, sündmuse töötlemiseks webhooks, dünaamiline Arvuta, serverless arhitektuur, F#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Azure'i funktsioonide F # Tootearendusmaterjal

> [AZURE.SELECTOR]
- [C# skript](../articles/azure-functions/functions-reference-csharp.md)
- [F # skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F # Azure'i funktsioonid on lahendus hõlpsalt töötab väikeste osade koodi või "funktsioone," pilveteenuses. Andmevahetus sisse oma F # funktsioon kaudu funktsiooni argumendid. Argumentide nimed on määratud `function.json`, ja seal on eelmääratletud nimede funktsioon puuraidur ja tühistamise sõned, näiteks juurdepääs.

Selles artiklis eeldatakse, et olete juba lugenud [Azure'i funktsioonide tootearendusmaterjal](functions-reference.md).

## <a name="how-fsx-works"></a>.Fsx tööpõhimõte

Mõne `.fsx` fail on F # skripti. Seda saab vaadelda kui F # projekti, mis on toodud ühe faili. Fail sisaldab koodi programmi (sel juhul oma Azure'i funktsioon) ja suuniseid haldamise sõltuvused.

Kui kasutate mõnda `.fsx` Azure funktsioonile tavaliselt nõutav assemblereid kaasatakse automaatselt teile, saate keskenduda funktsioon asemel "trafarett" kood.

## <a name="binding-to-arguments"></a>Argumendid sidumine

Iga sidumine toetab mõni kogumi argumendid, mis üksikasjalik [Azure'i funktsioonide päästikute ja sidumiste tootearendusmaterjal](functions-triggers-bindings.md). Üks argument sidumiste, mis toetab bloobimälu päästik on näiteks POCO, mida saab väljendada F # kirje. Näiteks:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

F # Azure'i funktsioon võtab üks või mitu argumenti. Kui räägitakse Azure'i funktsioonide argumendid, saame viidata _Sisestuskeel_ argumendid ja _väljund_ argumendid. Sisestuskeel argument on täpselt nagu see kõlab: F # Azure'i funktsiooni sisestada. _Väljundi_ argument on muutlik andmete või `byref<>` argument, mis toimib nii edasi andmete tagasi _välja_ oma funktsiooni.

Eeltoodud näites `blob` on Sisestuskeel argument ja `output` on väljundi argument. Teade, et kasutasime `byref<>` jaoks `output` (ei ole vaja lisamiseks soovitud `[<Out>]` marginaali). Abil soovitud `byref<>` tüüp võimaldab oma funktsioon, mis kirje või objekti, mis viitab argumendi muutmiseks.

Kirje F # kasutamisel on Sisestuskeel tüübiks kirje määratlust tuleb märkida `[<CLIMutable>]` Azure funktsioonide raames seadmiseks väljad õigesti enne läbides kirje oma funktsiooni võimaldamiseks. Kaitstud, `[<CLIMutable>]` kehtestajate kirje atribuutide genereerib. Näiteks:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

F # klassi saate kasutada ka mõlema sisse ja välja argumendid. Klassi, atribuudid tuleb tavaliselt getters- ja kehtestajate. Näiteks:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Logimine

Logige oma [streaming logid](../app-service-web/web-sites-streaming-logs-and-console.md) F # väljundi, võtma teie funktsiooni argumendi tüüp `TraceWriter`. Vormindusühtsuse, soovitame seda argumenti nimega `log`. Näiteks:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asünkroonse

Funktsiooni `async` töövoogu saab kasutada, kuid tulemi tagastamiseks vajab on `Task`. Seda saab teha `Async.StartAsTask`, näiteks:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Luba tühistamine

Kui teie funktsioon peab käsitlema sulgumist nõtkelt, saate seda on [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument. See saab kombineerida `async`, näiteks:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Importimise nimeruumid

Saate avada nimeruumid tavapärasel viisil:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Järgmised nimeruumid avatakse automaatselt:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Välise assemblereid viitamine

Samuti framework komplekti viited lisada soovitud `#r "AssemblyName"` direktiivi.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
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
* `Microsoft.AspNEt.WebHooks.Common`.

Kui teil on vaja viidata privaatne komplekti, saate üles laadida komplekti faili lisamine `bin` funktsioon ja viide selle abil faili nimi (nt kausta  `#r "MyAssembly.dll"`). Teavet selle kohta, kuidas funktsioon kausta failide üleslaadimine, lugege järgmist jaotist pakettide.

## <a name="editor-prelude"></a>Redaktori Prelude

Redaktori, mis toetab F # koostaja teenuste ei ole nimeruumid ja assemblereid, mis automaatselt lisab Azure'i funktsioonid. Võib olla kasulik kaasata prelude, mis aitab leida assemblereid, kasutate redaktori ning nimeruumid konkreetselt avamiseks. Näiteks:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Kui Azure funktsioonid aktiveeritakse teie kood, töötleb koos allika `COMPILED` määratletud nii, et editor prelude ignoreeritakse.

## <a name="package-management"></a>Paketi haldus

Nugeti pakettide F # funktsiooni kasutamiseks lisada mõne `project.json` salvestada selle funktsiooni kausta funktsioon rakenduse failisüsteemis. Siin on näide `project.json` faili, mis lisab Nugeti pakett viide `Microsoft.ProjectOxford.Face` versiooni 1.1.0:

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

Toetatakse ainult .NET Frameworki 4.6, seega veenduge, et teie `project.json` faili saate määrata `net46` nagu siin näidatud.

Kui laadite mõne `project.json` faili, runtime saab paketid ja lisab automaatselt paketi assemblereid viited. Te ei ole vaja lisada `#r "AssemblyName"` suuniseid. Lisage lihtsalt nõutav `open` lauseid oma `.fsx` faili.

Soovi korral võite luua automaatselt viited assemblereid oma redaktor Sissejuhatus, parandada teenustega F # kompileerida oma redaktor.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Kuidas lisada mõne `project.json` faili oma Azure'i funktsioon

1. Alustage kontrollite rakenduse funktsioon töötab, mida saate teha, avades oma funktsioon Azure'i portaalis. See ka annab juurdepääsu streaming logid kus paketi installi väljund kuvatakse.

2. Üles laadida soovitud `project.json` faili, kasutage ühte kirjeldatud [funktsioon rakenduse failide värskendamine](functions-reference.md#fileupdate). Kui kasutate [Pidev juurutamise Azure'i funktsioonide](functions-continuous-deployment.md), saate lisada mõne `project.json` oma lavastus haru selleks, et seda katsetada enne selle lisamist oma juurutamise haru faili.

3. Pärast selle `project.json` fail on lisatud, kuvatakse väljund sarnaneb teie funktsiooni järgmises näites on streaming log:

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

Mõne keskkonnas kui ka rakenduse väärtuse seadmine saamiseks kasutage `System.Environment.GetEnvironmentVariable`, näiteks:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Diagrammide korduvkasutamine .fsx kood

Saate kasutada muude koodi `.fsx` abil failide lisamine `#load` direktiivi. Näiteks:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Teed pakub soovitud `#load` direktiiv on asukoha suhtes oma `.fsx` faili.

* `#load "logger.fsx"`laadib funktsioon kaustas faili.

* `#load "package\logger.fsx"`laadib fail, mis asub selle `package` kausta funktsioon kausta.

* `#load "..\shared\mylogger.fsx"`laadib fail, mis asub selle `shared` kausta funktsioon kausta, mis on samal tasemel otse `wwwroot`.

Funktsiooni `#load` direktiivi töötab ainult `.fsx` (F # skript) faile, mitte `.fs` failid.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet järgmistest teemadest:

* [F # juhend](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Azure'i funktsioonide tootearendusmaterjal](functions-reference.md)
* [Azure'i funktsioonide C# tootearendusmaterjal](functions-reference-csharp.md)
* [Azure'i funktsioonide NodeJS tootearendusmaterjal](functions-reference-node.md)
* [Azure'i funktsioonide päästikute ja seosed](functions-triggers-bindings.md)
* [Azure'i funktsioonide testimine](functions-test-a-function.md)
* [Azure'i skaleerimist funktsioonid](functions-scale.md)
