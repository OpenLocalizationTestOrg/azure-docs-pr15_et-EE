<properties
    pageTitle="Alustamine bloobimälu salvestusruumi ja Visual Studio ühendatud teenused (WebJob projektid) | Microsoft Azure'i"
    description="Kuidas alustada bloobimälu kasutamine WebJob projekti pärast ühenduse loomine mõne Azure storage Visual Studio abil ühendatud teenused."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Azure'i bloobimälu alustamine salvestusruumi ja Visual Studio ühendatud teenused (WebJob projektid)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Ülevaade

Selles artiklis antakse C# koodi näidised, mis näitab, kuidas käivitamiseks kui ka Azure'i bloobimälu on loodud või värskendatud. Proovi kood kasutada [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) versioon 1.x. Salvestusruumi konto lisamisel Visual Studio **Ühendatud teenuste lisamine** dialoogiboksi kaudu WebJob projekti asjakohane Azure'i salvestusruumi Nugeti pakett on installitud, lisatakse vastavad .net-i viited projekti ja ühendusstringi salvestusruumi konto on värskendatud App.config faili.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Kuidas, mis käivitab funktsiooni lisamine bloobimälu on loodud või värskendatud

Selles jaotises kirjeldatakse **BlobTrigger** atribuut kasutamise kohta.

 **Märkus:** WebJobs SDK skannib logifailid vaadata uusi või muudetud plekid. See toiming on potentsiaalselt aeglane; funktsiooni võib saada käivitatud kuni minuti või enam bloobimälu selle loomise järel.  Kui teie rakendus peab töötlemine plekid kohe, on soovitatav meetod järjekorda sõnumi loomisel kuvatakse bloobimälu, ja kasutage [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) atribuuti **BlobTrigger** atribuuti funktsioon, mis on bloobimälu töötleb loomine.

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

## <a name="types-that-you-can-bind-to-blobs"></a>Tüübid, mida võite siduda plekid

Saate kasutada atribuuti **BlobTrigger** järgmist tüüpi:

* **string**
* **TextWriterit**
* **Voo**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Muud tüüpi deserialized [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder) järgi

Kui soovite töötada otse Azure storage konto, saate ka **CloudStorageAccount** parameetri meetod signatuuri lisada.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Saada teksti bloobimälu sisu siduv string

Kui tekst plekid oodatakse, saab **päringustringi** parameetri **BlobTrigger** rakendada. Järgmine kood näidis seob teksti bloobimälu **päringustringi** parameetri, nimega **logMessage**. Funktsioon kasutab selle parameetri kirjutamiseks sisu on bloobimälu WebJobs SDK armatuurlaud.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Saada seeriasertide bloobimälu sisu ICloudBlobStreamBinder abil

Järgmine kood näidis kasutab ainekursuse, mis rakendab **ICloudBlobStreamBinder** lubamiseks **BlobTrigger** atribuut on bloobimälu sidumiseks **WebImage** tüüp.

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

**WebImage** sidumine kood on esitatud **WebImageBinder** tunni, mis tuleneb **ICloudBlobStreamBinder**.

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

## <a name="how-to-handle-poison-blobs"></a>Mürki plekid reageerimine

Kui **BlobTrigger** funktsioon nurjub, SDK nõuab see uuesti, juhuks, kui selle põhjuseks ajutine tõrge. Kui sisu on bloobimälu põhjuseks on tõrge, nurjub funktsiooni iga kord, kui püüab selle bloobimälu töötlemine. Vaikimisi SDK nõuab funktsiooni kuni 5 korda antud bloobimälu. Viienda proovige nurjumisel lisab SDK sõnumi nimega *webjobs-blobtrigger-mürki*järjekorda.

Korduskatsed maksimumarv on konfigureeritav. Sama [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) säte kasutatakse mürki bloobimälu töötlemine ja mürki järjekorda sõnumi töötlemine.

Mürki plekid järjekorda sõnum on JSON-objekti, mis sisaldab järgmised atribuudid:

* FunctionId (vormingus *{WebJob nimi}*. Funktsioonid. *{Funktsiooni nime}*, nt: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" või "PageBlob")
* Ümbrisenimi
* BlobName
* ETag (bloobimälu versiooni identifikaator, nt: "0x8D1DC6E70A277EF")

Järgmine kood näidis funktsiooni **CopyBlob** sisaldab koodi, mis põhjustab see ei õnnestu iga kord, kui seda nimetatakse. Pärast SDK eeldab see korduskatsed maksimumarv, sõnumi, luuakse mürki bloobimälu kuhjuda ja see sõnum töötleb **LogPoisonBlob** funktsioon.

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

SDK deserializes automaatselt JSON sõnum. Siin on **PoisonBlobMessage** klassi.

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Bloobimälu küsitlused algoritmi

WebJobs SDK skannib kõik ümbriste määratud rakenduse käivitamine **BlobTrigger** atribuute. Suure salvestusruumi konto kontrollimine võib võtta aega, nii, et see võib olla aega enne uue plekid ei leita ja **BlobTrigger** funktsioonid on täidetud.

Rakenduse käivitamine pärast uusi või muudetud plekid tuvastamiseks SDK loeb perioodiliselt bloobimälu salvestusruumi logid. Bloobimälu logid on puhverdatud ja ainult saada füüsilise kirjutatud iga 10 minuti järel või nii, et võib olla märgatavat viivituse pärast soovitud bloobimälu on loodud või värskendatud enne seda **BlobTrigger** funktsioon aktiveeritakse.

Erandiks plekid **bloobimälu** atribuut abil loodud on. Kui WebJobs SDK loob uue bloobimälu, edastab selle mis tahes kattuvad **BlobTrigger** funktsioonide kohe uue bloobimälu. Seetõttu kui teil on ahelas bloobimälu sisendi ja väljundi, SDK saab töödelda neid tõhus. Kuid kui soovite madal latentsus töötab teie bloobimälu töötlemise funktsioonid plekid, mis on loodud või värskendatud muul viisil, soovitame **QueueTrigger** , mitte **BlobTrigger**.

### <a name="blob-receipts"></a>Bloobimälu lugemisteatised

WebJobs SDK tagab, et pole **BlobTrigger** funktsiooni saab ehk rohkem kui üks kord sama uus või värskendatud bloobimälu. See, et see säilitades *bloobimälu lugemisteatised* selleks, et kindlaks teha, kui antud bloobimälu versioon on tehtud.

Bloobimälu lugemisteatised on talletatud ümbris nimega *azure-webjobs-hosts* Azure storage kontole määratud AzureWebJobsStorage ühendusstring. Bloobimälu kviitungit on järgmine teave.

* Funktsioon, mis on nõudnud bloobimälu ("*{WebJob nimi}*. Funktsioonid. *{Funktsiooni nime}*", näiteks:"WebJob1.Functions.CopyBlob")
* Container nimi
* Bloobimälu tüüp ("BlockBlob" või "PageBlob")
* Bloobimälu nimi
* Funktsiooni ETag (bloobimälu versiooni identifikaator, nt: "0x8D1DC6E70A277EF")

Kui soovite mõne bloobimälu ümbertöötamine, saate käsitsi bloobimälu teatis jaoks selle bloobimälu *azure-webjobs-hosts* -ümbrisest kustutada.

## <a name="related-topics-covered-by-the-queues-article"></a>Seotud järjekorrad artikli teemad

Lisateavet reageerimine bloobimälu töötlemine käivitab järjekorda sõnumi või WebJobs SDK töötlemise, stsenaariumid, pole teatud Bloobivahemälu kohta leiate [Azure'i järjekorda salvestusruumi WebJobs SDK kasutamise kohta](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Seotud teemad selles artiklis on järgmised:

* Asünkroonse funktsioonid
* Mitmes eksemplaris
* Graatsiline sulgemine
* Kasutage funktsiooni kehas WebJobs SDK atribuudid
* Saate seada SDK ühendusstringi kood.
* Määrata väärtused WebJobs SDK ehitaja parameetrite kood
* **MaxDequeueCount** konfigureerimine mürki bloobimälu töötlemine.
* Funktsiooni käsitsi käivitamine
* Logide kirjutamine

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis on andnud koodinäiteid, reageerimine tavastsenaariumid töötamine Azure plekid kuvavate. Azure'i WebJobs ja WebJobs SDK kasutamise kohta leiate lisateavet teemast [Azure WebJobs dokumentatsiooni ressursid](http://go.microsoft.com/fwlink/?linkid=390226).
