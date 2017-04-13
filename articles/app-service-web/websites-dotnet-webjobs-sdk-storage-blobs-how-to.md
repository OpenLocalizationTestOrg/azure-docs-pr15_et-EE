<properties 
    pageTitle="Kuidas kasutada WebJobs SDK Azure'i bloobimälu" 
    description="Saate teada, kuidas kasutada Azure'i bloobimälu WebJobs SDK. Käivitab uue bloobimälu kuvatakse ümbris protsessi ja käsitlemiseks "mürki plekid"." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Kuidas kasutada WebJobs SDK Azure'i bloobimälu

## <a name="overview"></a>Ülevaade

Sellest juhendist leiate C# koodi näidised, mis näitab, kuidas käivitamiseks kui ka Azure'i bloobimälu on loodud või värskendatud. Proovi kood kasutada [WebJobs SDK](websites-dotnet-webjobs-sdk.md) versioon 1.x.

Jaoks koodinäiteid, mis näitab, kuidas luua plekid leiate [Azure'i järjekorda salvestusruumi WebJobs SDK kasutamise kohta](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 
        
Juhend eeldab, et teate, [Kuidas luua WebJob projekti Visual Studios koos ühendusstringi, mis osutage salvestusruumi konto](websites-dotnet-webjobs-sdk-get-started.md) või [mitme salvestusruumi](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs)kontod.

## <a id="trigger"></a>Kuidas, mis käivitab funktsiooni lisamine bloobimälu on loodud või värskendatud

Selles jaotises kirjeldatakse, kuidas kasutada funktsiooni `BlobTrigger` atribuut. 

> [AZURE.NOTE] WebJobs SDK skannib logifailid vaadata uusi või muudetud plekid. Seda toimingut ei ole reaalajas; funktsiooni võib saada käivitatud kuni minuti või enam bloobimälu selle loomise järel. Lisaks [salvestusruumi logid luuakse "parima"](https://msdn.microsoft.com/library/azure/hh343262.aspx) alusel; ei ole kindel, et kõik sündmused on kaetud. Teatud tingimustel võib vastamata logid. Kui bloobimälu päästikute kiirus ja usaldusväärsus piirangud pole aktsepteeritav rakenduse, soovitatav meetod on luua järjekorda sõnumi loomisel kuvatakse bloobimälu, ja kasutage selle asemel atribuuti [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) on `BlobTrigger` funktsiooni, mis on bloobimälu töötleb atribuut.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Ühe kohatäite laiendiga bloobimälu nimi  

Järgmine kood näidis kopeerib teksti plekid *Sisestuskeel* ümbrises-ümbrisest *väljundi* kuvatavate:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Atribuut ehitaja võtab päringustringi parameetri, mis määrab container nimi ja bloobimälu nime kohatäide. Selles näites kui bloobimälu, mis on nimega *Blob1.txt* luuakse *Sisestuskeel* ümbris, funktsioon loob bloobimälu, mis on nimega *Blob1.txt* *väljundi* ümbrises. 

Saate määrata nime mustri kohatäitega bloobimälu nimi, nagu on näidatud järgmises näites kood:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Järgmine kood kopeeritakse ainult plekid, mis on nimed, alustades "algse-". Näiteks kopeeritakse *Algne-Blob1.txt* *Sisestuskeel* ümbrises *Kopeeri-Blob1.txt* *väljundi* ümbrises.

Kui teil on vaja bloobimälu nimed, mis on looksulud nimi nimi mustri määramiseks topeltklõpsake soovitud looksulud. Näiteks, kui soovite leida plekid *piltide* ümbrises, mis on nimed umbes järgmine:

        {20140101}-soundfile.mp3

Kasutage seda oma mustri:

        images/{{20140101}}-{name}

Näites oleks *nime* kohatäide väärtus *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Eraldi bloobimälu nimi ja laiend kohatäidete

Järgmine kood näidis muutub faililaiendit selle valiku puhul kopeeritakse *Sisestuskeel* ümbrises-ümbrisest *väljundi* kuvatavate plekid. Koodi logib *Sisestuskeel* bloobimälu pikendamise ja määrab *väljundi* bloobimälu pikendamine *.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Tüübid, mida võite siduda plekid

Saate kasutada funktsiooni `BlobTrigger` atribuut järgmist tüüpi:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Muud tüüpi deserialized [ICloudBlobStreamBinder](#icbsb) järgi 

Kui soovite töötada Azure storage konto, saate lisada mõne `CloudStorageAccount` meetod allkirja-parameeter.

Näiteid leiate [bloobimälu sidumine kood azure-webjobs-sdk hoidla GitHub.com kohta](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Saada teksti bloobimälu sisu siduv string

Kui tekst plekid eeldatakse, `BlobTrigger` saab rakendada mõne `string` parameeter. Järgmine kood näidis seob teksti bloobimälu abil soovitud `string` parameeter nimega `logMessage`. Funktsioon kasutab selle parameetri kirjutamiseks sisu on bloobimälu WebJobs SDK armatuurlaud. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Saada seeriasertide bloobimälu sisu ICloudBlobStreamBinder abil

Järgmine kood näidis kasutab klassi, mis rakendab `ICloudBlobStreamBinder` lubamiseks on `BlobTrigger` atribuut sidumiseks mõne bloobimälu abil soovitud `WebImage` tüüp.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

Funktsiooni `WebImage` Köitmine kood on esitatud on `WebImageBinder` klassi, mis tuleneb `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Saada bloobimälu tee sünergilise bloobimälu

Container nime ja bloobimälu bloobimälu, mis on käivitanud funktsiooni nime, võite kaasata on `blobTrigger` stringi parameeter funktsioon signatuur.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Mürki plekid reageerimine

Kui soovitud `BlobTrigger` funktsioon nurjub, SDK nõuab see uuesti, juhuks, kui selle põhjuseks ajutine tõrge. Kui sisu on bloobimälu põhjuseks on tõrge, nurjub funktsiooni iga kord, kui püüab selle bloobimälu töötlemine. Vaikimisi SDK nõuab funktsiooni kuni 5 korda antud bloobimälu. Viienda proovige nurjumisel lisab SDK sõnumi nimega *webjobs-blobtrigger-mürki*järjekorda.

Korduskatsed maksimumarv on konfigureeritav. Sama [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) säte kasutatakse mürki bloobimälu töötlemine ja mürki järjekorda sõnumi töötlemine. 

Mürki plekid järjekorda sõnum on JSON-objekti, mis sisaldab järgmised atribuudid:

* FunctionId (vormingus *{WebJob nimi}*. Funktsioonid. *{Funktsiooni nime}*, nt: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" või "PageBlob")
* Ümbrisenimi
* BlobName
* ETag (bloobimälu versiooni identifikaator, nt: "0x8D1DC6E70A277EF")

Järgmine kood valimis on `CopyBlob` funktsioon on põhjustab see ei õnnestu iga kord, kui seda nimetatakse tähist. Pärast SDK nõuab korduskatsed maksimumarv, sõnumi, luuakse mürki bloobimälu kuhjuda ja see sõnum töötleb selle `LogPoisonBlob` funktsioon. 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

SDK deserializes automaatselt JSON sõnum. Siin on selle `PoisonBlobMessage` klassi: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Bloobimälu küsitlused algoritmi

WebJobs SDK skannib kõik ümbriste määratud `BlobTrigger` atribuute rakenduse käivitamine. Suure salvestusruumi konto kontrollimine võib võtta aega, nii, et see võib olla aega enne uue plekid ei leita ja `BlobTrigger` funktsioonid on täidetud.

Rakenduse käivitamine pärast uusi või muudetud plekid tuvastamiseks SDK loeb perioodiliselt bloobimälu salvestusruumi logid. Bloobimälu logid on puhverdatud ja ainult saada füüsilise kirjutatud iga 10 minuti järel või nii, et nii võib olla märgatavat viivituse pärast soovitud bloobimälu on loodud või värskendatud enne seda `BlobTrigger` funktsioon aktiveeritakse. 

Erandiks plekid abil loodud on selle `Blob` atribuut. Kui WebJobs SDK loob uue bloobimälu, see edastab uue bloobimälu kohe mis tahes sobitamine `BlobTrigger` funktsioonid. Seetõttu kui teil on ahelas bloobimälu sisendi ja väljundi, SDK saab töödelda neid tõhus. Kui soovite madal latentsus töötab teie bloobimälu plekid, mis on loodud või värskendatud muul viisil töötlemise funktsioone, soovitame kasutada, kuid `QueueTrigger` mitte `BlobTrigger`.

### <a id="receipts"></a>Bloobimälu lugemisteatised

WebJobs SDK tagab, et pole `BlobTrigger` funktsiooni saab kutsuda rohkem kui üks kord sama uus või värskendatud bloobimälu jaoks. See, et see säilitades *bloobimälu lugemisteatised* selleks, et kindlaks teha, kui antud bloobimälu versioon on tehtud.

Bloobimälu lugemisteatised on talletatud ümbris nimega *azure-webjobs-hosts* Azure storage kontole määratud AzureWebJobsStorage ühendusstring. Bloobimälu kviitungit on järgmine teave.

* Funktsioon, mis on nõudnud bloobimälu ("*{WebJob nimi}*. Funktsioonid. *{Funktsiooni nime}*", näiteks:"WebJob1.Functions.CopyBlob")
* Container nimi
* Bloobimälu tüüp ("BlockBlob" või "PageBlob")
* Bloobimälu nimi
* Funktsiooni ETag (bloobimälu versiooni identifikaator, nt: "0x8D1DC6E70A277EF")

Kui soovite mõne bloobimälu ümbertöötamine, saate käsitsi bloobimälu teatis jaoks selle bloobimälu *azure-webjobs-hosts* -ümbrisest kustutada.

## <a id="queues"></a>Seotud järjekorrad artikli teemad

Lisateavet reageerimine bloobimälu töötlemine käivitab järjekorda sõnumi või WebJobs SDK töötlemise, stsenaariumid, pole teatud Bloobivahemälu kohta leiate [Azure'i järjekorda salvestusruumi WebJobs SDK kasutamise kohta](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Seotud teemad selles artiklis on järgmised:

* Asünkroonse funktsioonid
* Mitmes eksemplaris
* Graatsiline sulgemine
* Kasutage funktsiooni kehas WebJobs SDK atribuudid
* Saate seada SDK ühendusstringi kood.
* Määrata väärtused WebJobs SDK ehitaja parameetrite kood
* Konfigureerimine `MaxDequeueCount` mürki bloobimälu käsitsemiseks.
* Funktsiooni käsitsi käivitamine
* Logide kirjutamine

## <a id="nextsteps"></a>Järgmised sammud

Sellest juhendist on andnud koodinäiteid, reageerimine tavastsenaariumid töötamine Azure plekid kuvavate. Azure'i WebJobs ja WebJobs SDK kasutamise kohta leiate lisateavet teemast [Azure WebJobs Soovitatavad ressursid](http://go.microsoft.com/fwlink/?linkid=390226).
 
