<properties
    pageTitle="Suurte andmekogumite kasutades andmete Factory ja töölehte töötlemine | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse töötlemine andmed on Azure andmete Factory teel paralleelne töötlemine videovõimaluste kontrollimine Azure'i paketi abil."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Protsess suurte andmekomplektide andmete Factory ja paketi abil
Selles artiklis kirjeldatakse arhitektuur valimi lahenduse, mis teisaldab ja töötleb suurte andmekomplektide automaatse ja ajastatud viisil. See sisaldab ka mõne lõpuni kiirtutvustus rakendada Azure'i andmed Factory ja Azure'i paketi abil. 

Selles artiklis on pikem kui meie tüüpilised artikkel, kuna see sisaldab kogu valimi lahenduse lühiülevaade. Kui teil on uus paketi ja andmete Factory, nende teenuste kohta leiate teavet teemast ja kuidas need koos töötada. Kui soovite teada midagi teenused on kujundamisel/architecting lahenduse, saate võivad keskenduda ainult artikli [jaotisest arhitektuur](#architecture-of-sample-solution) ja kui teil on tekkinud prototüüp või lahenduse, võite proovida [kiirtutvustus](#implementation-of-sample-solution)üksikasjalikke juhiseid. Kutsume oma kommentaare selle sisu ja kuidas seda kasutada.

Esmalt vaatame, kuidas andmeid Factory ja paketi aitab töötlemise suurte andmekogumite pilveteenuses.     

## <a name="why-azure-batch"></a>Miks Azure'i paketi?
Azure'i paketi võimaldab teil käivitada rakendusi suuremahuliste paralleelselt ja suure jõudlusega arvutuste (HPC) tõhus pilveteenuses. See on platform teenus, mis koosolekut kavandava Arvuta mahukat tööd käitamist virtuaalmasinates hallatavate kogumi ja saab automaatselt skaala arvutada ressursse, mis teie töö vajadustele.

Teenusega paketi määratleda Azure Arvuta ressursse, mis teie rakendusi käivitada, samal ajal ja ulatuses. Nõudmisel või ajastatud käivitada töökohtade ning te ei pea käsitsi, konfigureerimiseks ja loomine ja haldamine on HPC kobar, üksikute virtuaalmasinates, virtuaalse võrgu või keerukas töö tööülesande ajastamise taristu.

Kui te ei ole Azure'i paketi tuttav, nagu see aitab mõistmine selles artiklis kirjeldatud lahendus arhitektuur/rakendamine, lugege järgmisi artikleid.   

- [Azure'i paketi põhialused](../batch/batch-technical-overview.md)
- [Paketi funktsioonide ülevaade](../batch/batch-api-basics.md)

(valikuline) Azure'i paketi kohta leiate lisateavet teemast [õppeteema Azure'i paketi jaoks](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Miks Azure'i andmed Factory?
Andmete Factory on pilvepõhist andmete integreerimine teenus, mis orchestrates ja automatiseerib liikumine ja andmete teisendus. Andmete Factory teenuse kasutamisel saate luua hallatava torustikud andmete teisaldamiseks asutusesisesest ja Pilveteenusest andmete poed tsentraliseeritud andmed salvestada (nt: Azure'i bloobimälu), ja protsessi transformatsiooni lisamiseks andmete kasutamine Windows Azure Hdinsightiga ja Azure masina õ. Samuti saate ajastada andmete torujuhtmete käivitada ajastatud viisil (tunni, iga päev, iga nädal, jne) ja jälgida ja hallata neid ühe pilguga probleemide tuvastamine ja tegutsema. 

Kui te ei ole tuttav Azure'i andmed Factory, nagu see aitab mõistmine selles artiklis kirjeldatud lahendus arhitektuur/rakendamine, lugege järgmisi artikleid.  

- [Azure'i andmed Factory kasutuselevõtt](data-factory-introduction.md)
- [Oma esimese andmete müügivõimaluste koostamine](data-factory-build-your-first-pipeline.md)   

(valikuline) Azure'i andmete hankimise kohta leiate lisateavet teemast [õppeteema Azure'i andmed Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Andmete Factory ja paketi koos
Andmete Factory sisaldab sisseehitatud tegevusi, nagu Kopeeri tegevuse Teisalda andmete allikas andmesalve sihtkoha andmed salvestada ja Azure Hadoopi kogumite (Hdinsightiga) kasutamine andmete töötlemise tegevuste taru. Vaadake [Tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) toetatud teisendus tegevuste loend. 

See võimaldab teil luua kohandatud .NET tegevuste liikuda või töödelda oma loogika ja käivitage need tegevused on Windows Azure Hdinsightiga kobar või Azure'i paketi kogumi VMs. Azure'i paketi kasutamisel saate konfigureerida pool, et mastaabi automaatselt (lisamine või eemaldamine põhineb töökoormus VMs) esitate valemi.     

## <a name="architecture-of-sample-solution"></a>Valimi lahenduse arhitektuur
Selles artiklis kirjeldatud arhitektuur on lihtne lahenduse, on oluline keerukate stsenaariumid, nt risk modelleerimine finants, pilditöötluse, visualiseerimine ja genoomse analüüsi. 

Diagramm näitab 1) kuidas andmed Factory orchestrates andmete liikumine ja töötlemiseks ja (2) kuidas Azure'i paketi töötleb andmed paralleelselt viisil. Alla laadida ning printida skeemi lihtsam viidata (11 x 17 sisse. või A3 suurus): [HPC ja andmete korraldamise Azure'i paketi ja andmete Factory abil](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Suuremahuliste töötlemise skeem](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

Järgmisest loendist leiate protsessi põhitoimingud. Lahendus sisaldab koodi ja selgitused luua lahenduse lõpuni.

1.  **Azure'i paketi konfigureerimine kogumi Arvuta sõlmed (VMs)**. Saate määrata sõlmed arv ja iga sõlme suurust.

2.  **Azure'i andmed Factory eksemplari loomine** , mis on konfigureeritud üksusi, mis moodustavad Azure'i bloobimälu, Azure'i paketi Arvuta teenuse, sisend andmed ja tegevused, mida liikumine ja andmete teisendamiseks töövoo/kohaletoimetamisel.

3.   **Loo kohandatud .NET tegevuse andmete Factory tulemas**. Azure'i paketi pool oma kasutaja koodi on.

4.  **Poe suure hulga sisendandmete nimega plekid Azure salvestusruumi**. Andmed on jagatud loogilise sektorid (tavaliselt kellaaja alusel).

5.  **Andmete Factory kopeerib andmed, mida töödeldakse paralleelselt** teisene asukohta.

6.  **Andmete Factory töötab kohandatud tegevuse abil eraldatud paketi kausta**. Andmete Factory saab käivitada üheaegselt tegevusi. Iga tegevuse töötleb sektori andmed. Tulemused salvestatakse Azure salvestusruumi.

7.  **Andmete Factory viib lõplik tulemused kolmanda asukohta**, rakenduse kaudu levitamiseks või muude tööriistadega edasiseks töötlemiseks.

## <a name="implementation-of-sample-solution"></a>Valimi lahenduse rakendamine
Valimi lahendus on teadlikult lihtne ja andmete Factory ja paketi kasutamine koos töödelda andmekomplektide kuvamiseks. Lahendus lihtsalt loendab Sisestuskeel failides, mis on korraldatud aja sarja esinemiskordade otsingutermin ("Microsoft") See väljundid väljundi failide arvu.

**Korda**: kui tuttavaid põhitõdesid Azure, andmete Factory ja paketi ja olete täitnud eeltingimused, mis on loetletud allpool, meie hinnangul see lahendus võtab 1-2 tundi.

### <a name="prerequisites"></a>Eeltingimused

#### <a name="azure-subscription"></a>Azure'i tellimus
Kui teil on Azure'i tellimus, saate luua tasuta prooviversiooni konto vaid paar minutit. Vt [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure storage konto
Kasutage andmete talletamiseks selles õpetuses Azure storage konto. Kui teil pole Azure storage konto, lugege [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account). Valimi lahendus kasutab bloobimälu.

#### <a name="azure-batch-account"></a>Azure'i paketi konto
Looge konto Azure'i paketi [Azure portaali](http://manage.windowsazure.com/). Vt [loomine ja haldamine konto Azure'i paketi](../batch/batch-account-create-portal.md). Pange tähele Azure'i paketi konto nimi ja konto võti. [Uus-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet-käsu abil konto Azure'i paketi loomine. Üksikasjalikud juhised selle cmdlet-käsu abil vt [Alustamine Azure'i paketi PowerShelli cmdlet-käsud](../batch/batch-powershell-cmdlets-get-started.md) .

Valimi lahendus kasutab Azure'i paketi (kaudu on Azure andmete Factory müügivõimaluste) andmete töötlemise paralleelselt viisil aluseks olevate Arvuta sõlmed (hallatavate virtuaalmasinates saidikogum).

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Azure'i paketi kogumi virtuaalmasinates (VM)
Saate luua vähemalt 2 Arvuta sõlmed on **Azure'i paketi kausta** .

1.  [Azure'i portaal](https://portal.azure.com), klõpsake vasakpoolses menüüs nuppu **Sirvi** ja käsku **Paketi kontod**. 
2. Valige konto Azure'i paketi **Paketi konto** tera avamiseks. 
3. Klõpsake paani **kaustu** .
4. Klõpsake **kaustu** labale lisamiseks on tööriistaribal nuppu Lisa.
    1. Sisestage kausta (**Kausta ID**). Pange tähele **ID pool**; peate selle andmete Factory lahenduse loomisel. 
    2. Määrake sätte operatsioonisüsteemi pere **Windows Server 2012 R2** .
    3. Valige soovitud **sõlm hinnakirjad taseme**.
    4. Sisestage **2** väärtusena **Target spetsiaalne** säte.
    5. Sisestage väärtus **2** **Max ülesannete kohta sõlm** sätte.
    6. Klõpsake nuppu **OK** , et luua kausta. 
    
#### <a name="azure-storage-explorer"></a>Azure'i salvestusruumi Explorer   
[Azure'i salvestusruumi Explorer 6 (tool)](https://azurestorageexplorer.codeplex.com/) või [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (alates ClumsyLeaf tarkvara). Saate kasutada nende tööriistade kontrollimise ja muuta oma Azure Storage projektide, sh logid pilve majutatud rakenduste andmed.

1.  Ümbris nimega **mycontainer** privaatse juurdepääsuga (anonüümse juurdepääsu) loomine

2.  Kui kasutate **CloudXplorer**, luua kaustu ja alamkaustu järgmine struktuur:

    ![](./media/data-factory-data-processing-using-batch/image3.png)

    **Inputfolder** ja **outputfolder** on ülataseme **mycontainer,** kaustad ja **inputfolder** on alamkaustad kuupäeva ja kellaaja templite (YYYY-MM-DD-HH).

    Kui kasutate **Azure salvestusruumi Exploreri**järgmise juhise juurde, peate nimedega faile üles laadida: inputfolder/2015-11-16-00/file.txt, inputfolder/2015-11-16-01/file.txt ja jne. Selles etapis tuleb loob automaatselt kaustad.

3.  Luua teksti faili **file.txt** sisuga, mis sisaldab seda märksõna **Microsoft**teie arvutis. Näide: "test kohandatud tegevuse Microsoft test kohandatud tegevuse Microsoft".

4.  Faili üleslaadimine Azure'i bloobimälu Sisestuskeel järgmised kaustad.

    ![](./media/data-factory-data-processing-using-batch/image4.png)

    Kui kasutate **Azure salvestusruumi Explorer**, üles laadida faili **file.txt** **mycontainer**. Klõpsake tööriistaribal soovitud bloobimälu koopia loomiseks klõpsake nuppu **Kopeeri** . Muutke dialoogiboksis **Kopeeri bloobimälu** **sihtkoha bloobimälu nimi** **inputfolder/2015-11-16-00/file.txt.** Korrake seda toimingut loomiseks inputfolder/2015-11-16-01/file.txt inputfolder/2015-11-16-02/file.txt, inputfolder/2015-11-16-03/file.txt, inputfolder/2015-11-16-04/file.txt jne. See toiming automaatselt loob kaustad.

3.  Luua teise ümbris nimega: **customactivitycontainer**. Kohandatud toimingute zip-faili üleslaadimine selles ümbrises.

#### <a name="visual-studio"></a>Visual Studio
Installige Microsoft Visual Studio 2012 või hiljem luua kohandatud paketi tegevuse kasutada andmete Factory lahendus.

### <a name="high-level-steps-to-create-the-solution"></a>Üksikasjalik juhiseid lahenduse loomine

1.  Luua kohandatud tegevuse, mis sisaldab andmete töötlemise loogika.
2.  Azure'i andmed factory, mis kasutab kohandatud tegevuse loomiseks tehke järgmist.

### <a name="create-the-custom-activity"></a>Kohandatud toimingute loomine

Andmete Factory kohandatud tegevuse on selle valimi lahenduse. Valimi lahendus kasutab Azure'i paketi kohandatud tegevuste käivitamiseks. Vaadake [kasutada kohandatud tegevused on Azure andmete Factory teel](data-factory-use-custom-activities.md) põhiteave jaoks kohandatud arendada ja kasutada neid Azure'i andmed Factory torujuhtmetes.

.NET kohandatud tegevuse, mille abil saate luua mõne Azure'i andmed Factory kohaletoimetamisel, peate klassi, mis rakendab selle **IDotNetActivity** kasutajaliidese **.NET Klassiteek** projekti loomise kohta. Kasutajaliidese see on ainult üks viis: **käivitamine**. Siin on allkirja meetodit.

    public IDictionary<string, string> Execute(
                IEnumerable<LinkedService> linkedServices,
                IEnumerable<Dataset> datasets,
                Activity activity,
                IActivityLogger logger)

Meetod on mõned põhilised komponendid, mida on vaja mõista.

-   Meetod on nelja parameetrid:

    1.  **linkedServices**. Mõne sihttüüp lingitud teenuste link sisend andmeallikate loendi (nt: Azure'i bloobimälu) abil andmete factory. Selles näites on ainult üks lingitud teenuse tüüpi Azure Storage sisestus- ja väljundi jaoks kasutada.

    2.  **andmekomplektide**. See on andmekogumite sihttüüp loend. Selle parameetri abil saate asukohad ja skeemid sisend- ja andmekomplektide määratletud.

    3.  **tegevuste**. Selle parameetri tähistab praeguse Arvuta üksus – sel juhul teenuse Azure'i paketi.

    4.  **puuraidur**. Logija saate kirjutada silumine kommentaare pinna nimega "Kasutaja" Logi tulemas.

-   Meetod tagastab sõnastikku, mida saate kasutada kett kohandatud tegevuste koos tulevikus. See funktsioon pole veel juurutatud, nii on tühi sõnastiku tulu meetodit. 

#### <a name="procedure-create-the-custom-activity"></a>Toiming: Luua kohandatud tegevuse

1.  Visual Studio .NET Klassiteek projekti loomine.

    1.  Käivitage **Visual Studio 2012**/**2013/2015**.

    2.  Klõpsake menüüd **fail**, käsku **Uus**ja valige **Project**.

    3.  Laiendage **Mallid**ja valige **Visual C\#**. Ülevaate, saate kasutada C\#, kuid mis tahes keeleks .net-i abil saate luua kohandatud tegevuse.

    4.  Valige loendist paremal projekti tüüpi **Klassiteek** .

    5.  Sisestage **MyDotNetActivity** **nimi**.

    6.  Valige **C:\\ADF** **asukoht**. Looge kaust **ADF** , kui see pole olemas.

    7.  Klõpsake nuppu **OK** , et luua projekt.

2.  Klõpsake nuppu **Tööriistad**, osutage **Nugeti Package Manager**ja klõpsake **Package Manager konsooli**.

3.  **Paketi Manager konsooli**, käivitage järgmine käsk **Microsoft.Azure.Management.DataFactories**importida.

            Install-Package Microsoft.Azure.Management.DataFactories

4.  Projekti importimine **Azure Storage** Nugeti pakett. Peate selle paketi, sest kasutate selle valimi bloobimälu API.

        Install-Package Azure.Storage

5.  Järgmised **abil** suuniseid lisada projekti lähtefaili.

        using System.IO;
        using System.Globalization;
        using System.Diagnostics;
        using System.Linq;

        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Runtime;

        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

6.  Muuta **MyDotNetActivityNS** **nimeruumi** nimi.

        namespace MyDotNetActivityNS

7.  Klassi nime muutmine **MyDotNetActivity** ja tuletamine **IDotNetActivity** kasutajaliidese, nagu allpool näidatud.

        public class MyDotNetActivity : IDotNetActivity

8.  Rakendada (lisa) **Execute** meetodit **IDotNetActivity** kasutajaliidese **MyDotNetActivity** klassi ja kopeerige järgmine proovi kood meetodit. Loogika seda meetodit kasutada selgitus jaotisest [Käivitada meetod](#execute-method) .

        /// <summary>
        /// Execute method is the only method of IDotNetActivity interface you must implement.
        /// In this sample, the method invokes the Calculate method to perform the core logic.  
        /// </summary>
        public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
        {

            // declare types for input and output data stores
            AzureStorageLinkedService inputLinkedService;

            Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
            foreach (LinkedService ls in linkedServices)
                logger.Write("linkedService.Name {0}", ls.Name);

            // using First method instead of Single since we are using the same
            // Azure Storage linked service for input and output.
            inputLinkedService = linkedServices.First(
                linkedService =>
                linkedService.Name ==
                inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
                as AzureStorageLinkedService;

            string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
            string folderPath = GetFolderPath(inputDataset);
            string output = string.Empty; // for use later.

            // create storage client for input. Pass the connection string.
            CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
            CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

            // initialize the continuation token before using it in the do-while loop.
            BlobContinuationToken continuationToken = null;
            do
            {   // get the list of input blobs from the input storage client object.
                BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                         true,
                                         BlobListingDetails.Metadata,
                                         null,
                                         continuationToken,
                                         null,
                                         null);

                // Calculate method returns the number of occurrences of
                // the search term (“Microsoft”) in each blob associated
                // with the data slice.
                //
                // definition of the method is shown in the next step.
                output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

            } while (continuationToken != null);

            // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
            Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

            folderPath = GetFolderPath(outputDataset);

            logger.Write("Writing blob to the folder: {0}", folderPath);

            // create a storage object for the output blob.
            CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
            // write the name of the file.
            Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

            logger.Write("output blob URI: {0}", outputBlobUri.ToString());
            // create a blob and upload the output text.
            CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
            logger.Write("Writing {0} to the output blob", output);
            outputBlob.UploadText(output);

            // The dictionary can be used to chain custom activities together in the future.
            // This feature is not implemented yet, so just return an empty dictionary.
            return new Dictionary<string, string>();
        }


9.  Saate lisada klassi helper järgmistest meetoditest. **Täita** meetod on kasutada järgmised võimalused. Kõige olulisem **Calculate** meetodit isolaatide seni, kuni iga bloobimälu tähist.

        /// <summary>
        /// Gets the folderPath value from the input/output dataset.
        /// </summary>
        private static string GetFolderPath(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FolderPath;
        }

        /// <summary>
        /// Gets the fileName value from the input/output dataset.
        /// </summary>

        private static string GetFileName(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FileName;
        }

        /// <summary>
        /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
        /// and prepares the output text that is written to the output blob.
        /// </summary>

        public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
        {
            string output = string.Empty;
            logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
            foreach (IListBlobItem listBlobItem in Bresult.Results)
            {
                CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
                if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
                {
                    string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                    logger.Write("input blob text: {0}", blobText);
                    string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                    var matchQuery = from word in source
                                     where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                     select word;
                    int wordCount = matchQuery.Count();
                    output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
                }
            }
            return output;
        }


    **GetFolderPath** meetod tagastab tee kausta, mis viitab andmekomplekti ja **GetFileName** meetod tagastab andmekomplekti osutab bloobimälu/faili nimi.

        "name": "InputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "file.txt",
                "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",


    **Calculate** meetod arvutab märksõna **Microsoft** Sisestuskeel failid (plekid kausta) eksemplaride arv. Otsingusõna ("Microsoft") on raske koodiga kood.

10.  Projekti koostamine. Klõpsake menüükäsku **koostamine** ja klõpsake **Lahenduse luua**.

11.  Käivitage **Windows Explorer**ja liikuge **prügikasti\\silumine** või **prügikasti\\vabastage** kausta sõltuvalt koostamine.

12.  Looge zip-fail **MyDotNetActivity.zip** , mis sisaldab kõiki Binaarfailid on selle ** \\prügikasti\\silumine** kausta. Kui soovite kaasata selle MyDotNetActivity. **eelarveprojekti** faili nii, et saate täiendavaid üksikasju, näiteks reanumber lähtekoodi, mis põhjustas probleemi kui tõrge ilmneb.

    ![](./media/data-factory-data-processing-using-batch/image5.png)

13.  Laadige **MyDotNetActivity.zip** nimega on bloobimälu-ümbrisest bloobimälu: **customactivitycontainer** sisse **StorageLinkedService** lingitud **ADFTutorialDataFactory** teenuse Azure'i bloobimälu kasutab. Kui see pole juba olemas, saate luua bloobimälu container **customactivitycontainer** .

#### <a name="execute-method"></a>Käivitada meetod

Sellest jaotisest leiate rohkem üksikasju ja märkmete koodi käivitamine meetodi kohta.

1.  Liikmete iterating Sisestuskeel saidikogumi jaoks on leitud [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) nimeruumi. Iterating bloobimälu saidikogumi jaoks on vaja **BlobContinuationToken** ainekursust abil. Sisuliselt tuleb kasutada teha-ajal tsükkel koos luba nimega süsteem väljumine kursis. Lisateabe saamiseks vaadake, [Kuidas kasutada bloobimälu .net-i kaudu](../storage/storage-dotnet-how-to-use-blobs.md). Tavaline tsükkel on näidatud siin.

        // Initialize the continuation token.
        BlobContinuationToken continuationToken = null;
        do
        {
        // Get the list of input blobs from the input storage client object.
        BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                true,
                                          BlobListingDetails.Metadata,
                                          null,
                                          continuationToken,
                                          null,
                                          null);
        // Return a string derived from parsing each blob.
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

        } while (continuationToken != null);

    Lisateavet [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) meetodit dokumentatsioonist.

2.  Töö määramine plekid kaudu loogiliselt kood läheb maksimaalselt ära-ajal tsükkel. **Käivita** meetod, kas-ajal tsükkel edastamiste plekid loendit meetodiga nimega **Calculate**. Meetod tagastab stringi muutuja nimega **väljund** on tulemus on näidata kõik plekid segmendi kaudu.

    See tagastab esinemiskordade otsingusõna (**Microsoft**) bloobimälu, **Calculate** meetodil on möödas.

        output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);

3.  Kui **Calculate** meetod on töö lõpetanud, tuleb see uue bloobimälu kirjutada. Nii, et iga töödeldud plekid hulk, saate uute bloobimälu kirjutada tulemustega. Uue bloobimälu kirjutamiseks esmalt leida väljundi andmekomplekti.

        // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

4.  Kood kutsub ka helper meetodi: **GetFolderPath** toomiseks kausta tee (salvestusruumi container nimi).

        folderPath = GetFolderPath(outputDataset);

    **GetFolderPath** tekitab mõne AzureBlobDataSet, mis on nimega FolderPath vara andmekomplekti objekti.

        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

        return blobDataset.FolderPath;

5.  Kood kutsub **GetFileName** meetod tuua faili nimi (bloobimälu nimi). Koodi sarnaneb eeltoodud koodi kausta tee.

        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

        return blobDataset.FileName;

6.  Faili nimi on kirjutatud URI objekti. URI ehitaja kasutab **BlobEndpoint** atribuudi nimi container tagastamiseks. Ehitada väljundi bloobimälu URI lisatakse kausta tee ja faili nimi.  

        // Write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

7.  Faili nimi on kirjutatud ja nüüd te saate kirjutada väljundi stringi **Calculate** meetod uue bloobimälu:

        // Create a blob and upload the output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} to the output blob", output);
        outputBlob.UploadText(output);


### <a name="create-the-data-factory"></a>Andmete factory loomine

[Kohandatud toimingute loomine](#create-the-custom-activity) jaotises loodud kohandatud tegevuse ja mõne Azure'i bloobimälu container zip-fail, mille kahendfaile ja Eelarveprojekti faili üles laadida. Selles jaotises saate luua ka Azure'i **andmed factory** **kohaletoimetamisel** , mis kasutab **Kohandatud tegevuse**.

Kohandatud tegevuste Sisestuskeel andmekomplekti tähistab plekid (failid) Sisestuskeel kausta (mycontainer\\inputfolder) bloobimälu sisse. Väljundi andmekomplekti tegevuse tähistab väljundi plekid väljund kausta (mycontainer\\outputfolder) bloobimälu sisse.

Ühe või mitme faili kukutage Sisestuskeel kaustadesse:

    mycontainer -\> inputfolder
        2015-11-16-00
        2015-11-16-01
        2015-11-16-02
        2015-11-16-03
        2015-11-16-04

Näiteks kukutage ühe faili (fail.txt) järgmise sisuga kõik kaustad.

    test custom activity Microsoft test custom activity Microsoft

Iga sisend kausta vastab Azure'i andmed Factory sektorit isegi juhul, kui kaustal on 2 või mitu faili. Kui iga sektorit töötleb tulemas, kohandatud tegevuse itereerib läbi kõik plekid Sisestuskeel kausta sektorit.

Kuvatakse viis väljundi failid, millel on sama sisu. Näiteks väljundfail töötlemise 2015-11-16-00 kausta fail on sisu on järgmine:

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.

Kui te panete mitu faili (fail.txt, file2.txt, file3.txt) sama sisuga Sisestuskeel kausta, näete järgmist sisu väljundfail. Iga kausta (2015-11-16-00 jne) vastab selle valimi sektorit isegi siis, kui kaustal on mitu failid.

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.


Väljundfail on nüüd kolme joonega, üks iga Sisestuskeel faili (bloobimälu) kausta seostatud sektorit (2015-11-16-00).

Tööülesande luuakse iga toimingu käivitada. Selles näites on ainult üks tegevuse tulemas. Tulemas töötleb mõnda sektorit, kohandatud tegevuse töötab Azure'i paketi töödelda sektorit. Kuna seal on viis sektorid (iga sektorit võib olla mitu plekid või fail), on viis loodud Azure'i paketi tööülesanded. Tööülesande käitamisel paketi on tegelikult kohandatud tegevust, mis töötab.

Järgmised kiirtutvustus pakub täiendavaid üksikasju.

#### <a name="step-1-create-the-data-factory"></a>Samm 1: Looge andmete factory

1.  Pärast sisselogimist [Azure'i portaalis](https://portal.azure.com/), tehke järgmist.

    1.  Klõpsake vasakul menüüs **Uus** .

    2.  Klõpsake nuppu **Uus** tera **andmete + Analytics** .

    3.  Klõpsake **Andmete Factory** **andmeanalüüsi** enne.

2.  **Uute andmete factory** tera, sisestage **CustomActivityFactory** nimi. Azure'i andmed factory nimi peab olema globaalselt kordumatu. Kui kuvatakse tõrketeade: **andmete factory nimi "CustomActivityFactory" on pole saadaval**, muuta andmete factory (nt **yournameCustomActivityFactory**) nime ja proovige uuesti luua.

3.  Klõpsake **RESSURSIRÜHMA nimi**ja valige ressursi olemasolevasse rühma või ressursi rühma loomine.

4.  Veenduge, et kasutate õiget tellimus ja piirkond, kuhu soovite andmed factory luua.

5.  Klõpsake nuppu **Loo** **Uus andmete factory** enne.

6.  Näete andmete factory, luuakse **armatuurlaua** Azure portaali.

7.  Pärast andmete factory on loodud, kuvatakse andmete factory leht, mis kuvatakse andmete factory sisu.

 ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Samm 2: Looge lingitud teenused

Lingitud teenuste andmete linkimine või teenused on Azure andmete factory arvutada. Selles etapis tuleb teil link oma **Azure Storage** ja **Azure'i paketi** konto oma andmete factory.

#### <a name="create-azure-storage-linked-service"></a>Azure'i lingitud salvestusteenus loomine

1.  Klõpsake soovitud **autor ja juurutamine** paani **Andmete FACTORY** enne **CustomActivityFactory**jaoks. Näete andmete Factory redaktor.

2.  Klõpsake käsku **uute andmete talletamiseks** ja valige **Azure storage.** Peaksite nägema JSON script Editor Azure Storage lingitud teenuse loomiseks.

    ![](./media/data-factory-data-processing-using-batch/image7.png)

3.  Asendage Azure storage konto ja **konto võti** Accessi võti Azure storage konto nimi **konto nimi** . Hankimine teie salvestusruumi kiirklahv leiate artiklist [vaate, kopeerimine ja uuesti luua salvestusruumi kiirklahvide](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

4.  Klõpsake **Deploy** käsuriba lingitud teenuse kasutuselevõtuks.

    ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Azure'i paketi lingitud teenuse loomine

Selles etapis tuleb teil lingitud teenuse **Azure'i paketi** konto, mida kasutatakse andmete Factory kohandatud tegevuste käivitamiseks loomine.

1.  Klõpsake käsku **Uus arvutada** ja valige **Azure'i paketi.** Peaksite nägema JSON script Editor teenuse lingitud Azure'i paketi loomiseks.

2.  JSON skripti:

    1.  Asendage oma Azure'i paketi konto nimi **konto nimi** .

    2.  Asendage **kiirklahv** kiirklahv Azure'i paketi konto.

    3.  Sisestage **poolName** atribuudi**.** kogumi ID Selle atribuudi saate määrata, kas kausta nime või pool ID-ga.

    4.  Sisestage paketi URI **batchUri** JSON atribuudi. 
    
        > [AZURE.IMPORTANT] **URL-i** kaudu **Azure'i paketi konto blade** on järgmises vormingus: \<account_name\>. \<piirkond\>. batch.azure.com. Klõpsake soovitud JSON **batchUri** atribuudi tuleb teil **eemaldada "account_name."** URL-i. Näide: `"batchUri": "https://eastus.batch.azure.com"`.

        ![](./media/data-factory-data-processing-using-batch/image9.png)

        **PoolName** atribuudi, saate määrata ka pool pool nime asemel ID-d.

        > [AZURE.NOTE] Andmete Factory teenus ei toeta nõudmisel suvandit Azure'i paketi ei Hdinsightiga. Saate kasutada ainult mõne Azure'i andmed factory oma Azure'i paketi kausta.

    5.  Määrake atribuudi **linkedServiceName** **StorageLinkedService** . Eelmises juhises loodud lingitud teenus. See salvestusruumi kasutatakse on koondusalal failide ja logid.

3.  Klõpsake **Deploy** käsuriba lingitud teenuse kasutuselevõtuks.

#### <a name="step-3-create-datasets"></a>Samm 3: Looge andmekomplektide

Selles etapis tuleb teil luua andmekomplektide sisend- ja andmete esitamiseks.

#### <a name="create-input-dataset"></a>Sisestuskeel andmekomplekti loomine

1.  **Redaktori** andmete Factory, klõpsake tööriistaribal nuppu **Uus andmekomplekti** ja klõpsake nuppu **Azure'i bloobimälu** rippmenüüst menüü kaudu.

2.  Järgmised JSON koodilõigu JSON parempoolsel paanil asendamiseks tehke järgmist.

        {
            "name": "InputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
                    "format": {
                        "type": "TextFormat"
                    },
                    "partitionedBy": [
                        {
                            "name": "Year",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "yyyy"
                            }
                        },
                        {
                            "name": "Month",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "MM"
                            }
                        },
                        {
                            "name": "Day",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "dd"
                            }
                        },
                        {
                            "name": "Hour",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "HH"
                            }
                        }
                    ]
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        }


     Saate luua müügivõimaluste hiljem algusaeg ja selle ülevaade: 2015-11-16T00:00:00Z-ja lõppaeg: 2015-11-16T05:00:00Z. See on ajastatud andmete **tunni**, andes nii on 5 sisend sektorid (vahel **00**: 00:00 -\> **05**: 00:00).

     **Sagedus** ja Sisestuskeel andmekomplekti **intervall** on seatud **tund** ja **1**, mis tähendab, et Sisestuskeel sektorit on saadaval tunnis.

     Siin on algusaegade iga sektorit, mis on esindatud ülaltoodud JSON koodilõigu **SliceStart** süsteemi muutuja jaoks.

  	| **Sektorit** | **Algus**          |
  	|-----------|-------------------------|
  	| 1         | 2015-11-16T**00**: 00:00 |
  	| 2         | 2015-11-16T**01**: 00:00 |
  	| 3         | 2015-11-16T**02**: 00:00 |
  	| 4         | 2015-11-16T**03**: 00:00 |
  	| 5         | 2015-11-16T**04**: 00:00 |

     **FolderPath** arvutatakse aasta, kuu, päev ja tund osa sektorit algusaja (**SliceStart**) abil. Seetõttu siit, kuidas mõni Sisestuskeel kaust on vastendatud.

  	| **Sektorit** | **Algus**          | **Sisend kausta**  |
  	|-----------|-------------------------|-------------------|
  	| 1         | 2015-11-16T**00**: 00:00 | 2015-11-16 -**00** |
  	| 2         | 2015-11-16T**01**: 00:00 | 2015-11-16 -**01** |
  	| 3         | 2015-11-16T**02**: 00:00 | 2015-11-16 -**02** |
  	| 4         | 2015-11-16T**03**: 00:00 | 2015-11-16 -**03** |
  	| 5         | 2015-11-16T**04**: 00:00 | 2015-11-16 -**04** |

3.  Luua ja juurutada **InputDataset** tabeli tööriistariba käsku **Deploy** . 

#### <a name="create-output-dataset"></a>Väljundi andmekomplekti loomine

Selles etapis tuleb loomist teise andmehulga tüüpi AzureBlob väljundi andmete esitamiseks.

1.  **Redaktori** andmete Factory, klõpsake tööriistaribal nuppu **Uus andmekomplekti** ja klõpsake nuppu **Azure'i bloobimälu** rippmenüüst menüü kaudu.

2.  Järgmised JSON koodilõigu JSON parempoolsel paanil asendamiseks tehke järgmist.

        {
            "name": "OutputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "fileName": "{slice}.txt",
                    "folderPath": "mycontainer/outputfolder",
                    "partitionedBy": [
                        {
                            "name": "slice",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "yyyy-MM-dd-HH"
                            }
                        }
                    ]
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }


    Väljundi bloobimälu/faili luuakse iga Sisestuskeel sektorit. Siin on, kuidas väljund faili nimega jaoks iga sektorit. Väljund faili luuakse üks väljund kausta: **mycontainer\\outputfolder**.


  	| **Sektorit** | **Algus**          | **Väljundfail**       |
  	|-----------|-------------------------|-----------------------|
  	| 1         | 2015-11-16T**00**: 00:00 | 2015-11-16 -**00. txt** |
  	| 2         | 2015-11-16T**01**: 00:00 | 2015-11-16 -**01. txt** |
  	| 3         | 2015-11-16T**02**: 00:00 | 2015-11-16 -**02. txt** |
  	| 4         | 2015-11-16T**03**: 00:00 | 2015-11-16 -**03. txt** |
  	| 5         | 2015-11-16T**04**: 00:00 | 2015-11-16 -**04. txt** |

     Pidage meeles, et kõik failid Sisestuskeel kaustas (nt: 2015-11-16-00) osast, mille algusaja kuuluvad: 2015-11-16-00. Kui töödeldakse selles sektorit, kohandatud tegevuse kaudu iga faili skannib ja annab tulemiks joonele väljundfail otsingutermin ("Microsoft") esinemiskordade arvu. Kui kausta 2015-11-16-00 on kolm faile, on kolme joonega väljundfail: 2015-11-16-00.txt.

3.  Luua ja juurutada **OutputDataset**tööriistariba käsku **Deploy** .

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a>Samm 4: Loomine ja käivitamine tulemas kohandatud toimingute

Selles juhises saate luua müügivõimaluste ühe tegevuse varem loodud kohandatud tegevuse.

> [AZURE.IMPORTANT] Kui **file.txt** sisestada kaustade bloobimälu ümbrises pole üles laaditud, tehke enne loomist tulemas. **IsPaused** atribuudi väärtuseks false tulemas JSON, et tulemas käivitatakse kohe, kui **Alguskuupäev** on varem.

1.  Andmete Factory Editoris, klõpsake käsku **Uus kohaletoimetamisel** . Kui te ei näe käsku, klõpsake **... (Kolmikpunkt)** kuvamiseks.

2.  Järgmine JSON skript JSON parempoolsel paanil asendamiseks tehke järgmist.

        {
            "name": "PipelineCustom",
            "properties": {
                "description": "Use custom activity",
                "activities": [
                    {
                        "type": "DotNetActivity",
                        "typeProperties": {
                            "assemblyName": "MyDotNetActivity.dll",
                            "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                            "packageLinkedService": "AzureStorageLinkedService",
                            "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                        },
                        "inputs": [
                            {
                                "name": "InputDataset"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "OutputDataset"
                            }
                        ],
                        "policy": {
                            "timeout": "00:30:00",
                            "concurrency": 5,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MyDotNetActivity",
                        "linkedServiceName": "AzureBatchLinkedService"
                    }
                ],
                "start": "2015-11-16T00:00:00Z",
                "end": "2015-11-16T05:00:00Z",
                "isPaused": false
           }
        }

    Võtke arvesse järgmist.

    -   Tulemas on ainult üks tegevuste ja mis on tippige: **DotNetActivity**.

    -   **AssemblyName** on seatud DLL nimi: **MyDotNetActivity.dll**.

    -   **EntryPoint** on seatud **MyDotNetActivityNS.MyDotNetActivity**. See on põhimõtteliselt \<nimeruum\>. \<klassinimi\> koodi.

    -   **PackageLinkedService** on seatud **StorageLinkedService** , mis viitab bloobimälu, mis sisaldab kohandatud tegevuse zip-fail. Kui kasutate muu Azure Storage kontod sisend faili-ja kohandatud toimingute zip-fail, peate looma Azure Storage lingitud mõne muu teenus. Selles artiklis eeldatakse, et kasutate sama Azure Storage konto.

    -   **PackageFile** on seatud **customactivitycontainer/MyDotNetActivity.zip**. See on vormingus: \<containerforthezip\>/\<nameofthezip.zip\>.

    -   Kohandatud tegevuse võtab **InputDataset** sisendina ja **OutputDataset** väljundina.

    -   Kohandatud tegevuse atribuudi **linkedServiceName** osutab selle **AzureBatchLinkedService**, mis ütleb Azure'i andmed Factory, mis kohandatud tegevuse tuleb sisse Azure'i paketi käivitamine.

    -   **Kokkulangevus** säte on oluline. Kui kasutate vaikimisi väärtus, mis on 1, isegi juhul, kui teil on 2 või rohkem arvutada sõlmed Azure'i paketi kogumi, sektorid töödeldakse üksteise järel. Seetõttu ei võtmise paralleelne töötlemine võimalus Azure'i partii eeliseid. Kui seate **kokkulangevus** suurem väärtus, st 2; see tähendab, et kaks viilu (vastab Azure'i paketi kaks tööülesanded) saab töödelda samal ajal, mille puhul VMs Azure'i paketi pool on kasutatud. Seetõttu atribuudi kokkulangevus õigesti.

    -   Vaikimisi on ainult üks tööülesanne (sektorit) täide mis tahes hetkel VM. Põhjus on see, et **maksimum ülesannete kohta VM** on vaikimisi Azure'i paketi kohapeal on 1. Eeltingimused osana loodud on selle atribuudi väärtuseks 2, et kahe andmete Factory sektorid saate VM töötab samal ajal.


    -   **isPaused** atribuudi väärtuseks false vaikimisi. Tulemas käivitatakse kohe selles näites, kuna sektorid alustada varem. Saate seada selle atribuudi true pausi tulemas ja määrama tagasi false taaskäivitama.

    -   Sektorite toodeti tunni, nii, et tulemas on esitatud viis sektorid **algusaeg** ja **lõpuaeg** on viis tundi lahku.

3.  Klõpsake käsku **Deploy** käsuribal juurutamiseks tulemas.

#### <a name="step-5-test-the-pipeline"></a>Juhis 5: Test tulemas

Selles etapis tuleb teil testida tulemas failide lohistamine Sisestuskeel kaustadesse. Alustame testimine tulemas ühte faili, üks sisendväärtuste kausta kohta.

1.  Andmete Factory tera Azure'i portaalis, klõpsake **diagrammi**.

    ![](./media/data-factory-data-processing-using-batch/image10.png)

2.  Diagrammivaate, topeltklõpsake Sisestuskeel andmekomplekti: **InputDataset**.

    ![](./media/data-factory-data-processing-using-batch/image11.png)

3.  Peaksite nägema **InputDataset** abaluuga viis sektorit, mis on valmis. Pange tähele **sektorit ALGUSAEG** ja **LÕPUAEG sektorit** iga sektorit.

    ![](./media/data-factory-data-processing-using-batch/image12.png)

4.  **Diagrammivaate**nüüd klõpsake **OutputDataset**.

5.  Peaksite nägema on viis väljundi sektorid valmis olekus kui ta on juba andnud.

    ![](./media/data-factory-data-processing-using-batch/image13.png)

6.  Azure'i portaali abil saate kuvada **sektorid** seotud **ülesanded** ja milline oli iga sektorit VM. Vaadake teemat [andmete Factory ja paketi integreerimise](#data-factory-and-batch-integration) jaotisest. 

7.  Peaksite nägema väljundi failid **outputfolder** , **mycontainer** Azure'i bloobimälus.

    ![](./media/data-factory-data-processing-using-batch/image15.png)

    Peaksite nägema viis väljund faili, üks iga Sisestuskeel sektorit. Iga väljundfail peaks olema umbes järgmine väljund sisu:

        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.

    Järgmine diagramm näitab, kuidas andmeid Factory sektorid kaart ülesannete Azure'i paketi. Selles näites on on ainult üks Käivita.

    ![](./media/data-factory-data-processing-using-batch/image16.png)

8.  Nüüd Proovime nüüd mitme kausta failiga. Failide loomine: **file2.txt**, **file3.txt**, **file4.txt**ja **file5.txt** nagu file.txt kaustas sama sisuga: **2015-11-06-01**.

9.  Väljundi kausta **kustutamine** väljundfail: **2015-11-16-01.txt**.

10. Nüüd **OutputDataset** tera, paremklõpsake sektorit **sektorit ALGUSKELLAAEG** määratud **11/16/2015 01:00:00 AM**, ja klõpsake nuppu **Käivita** Käivita uuesti/elektroonikaromude-process sektorit. Nüüd on sektorit viis faili asemel ühte faili.

    ![](./media/data-factory-data-processing-using-batch/image17.png)

11. Pärast sektorit töötab ja selle olek on **valmis**, kontrollige **outputfolder** , **mycontainer** bloobimälus sisu väljundfail selle sektori (**2015-11-16-01.txt**) jaoks. Tuleks rida iga faili sellest osast.

        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.


> [AZURE.NOTE] Kui te ei kustutanud selle väljundi faili 2015-11-16-01.txt enne viis Sisestuskeel failidega, kuvatakse ühe rea eelmine sektorit käitatav ja viis praeguse sektorit Käivita ridu. Vaikimisi lisatakse sisu väljund faili, kui see on juba olemas.

#### <a name="data-factory-and-batch-integration"></a>Andmete Factory ja paketi integreerimine
Andmete Factory teenus loob töö Azure'i paketi nimi: **adf-poolname:job-xxx**. 

![Azure'i andmed Factory - paketi tööde haldamine](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Tööülesande töö luuakse iga tegevuste käivitamine, mõnda sektorit. Kui loendis on 10 sektorid töötlemiseks valmis, luuakse 10 ülesandeid töö. Teil võib olla mitu sektorit töötab samal ajal, kui teil on mitu Arvuta sõlmed kogumi. Kui ülempiir ülesannete kohta MSDN on seatud > 1, võib olla mitu sama Arvuta töötavate sektorit.

Selles näites on viis sektorit, seega viis tööülesanded Azure'i paketi. Koos **kokkulangevus** seatud **5** tulemas JSON Azure'i andmed Factory ja **maksimum ülesannete kohta VM** seadmine **2** Azure'i paketi pargis **2** VMs tööülesannete käivitatakse kiiresti (märkige algus- ja lõpuaegade ülesannete jaoks).

Portaali abil saate vaadata pakett-töö ja oma ülesandeid, mis on seotud **sektorid** ning milline oli iga sektorit VM. 

![Azure'i andmed Factory - paketi tööülesanded](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Silumine tulemas

Silumine koosneb mõne põhimeetodist.

1.  Kui Sisestuskeel sektorit on seatud **valmis**, kinnitada, et Sisestuskeel kausta ülesehitust oleks õige ja file.txt olemas Sisestuskeel kaustadesse.

    ![](./media/data-factory-data-processing-using-batch/image3.png)

2.  Kasutage oma kohandatud tegevuste **käivitamine** meetod **IActivityLogger** objekti Logiteave, mis aitab teil probleemide tõrkeotsing. Loginud sõnumid kuvataks kasutaja\_väärtust 0. logifaili.

    Klõpsake **OutputDataset** labale sektorit sektorit **Andmete sektorit** höövlitera kuvamiseks. Näete **tegevuse töötab** sektorit. Peaksite nägema ühe tegevuste käivitamine sektorit. Kui klõpsate nuppu **Käivita** käsuriba, saate alustada muu tegevuse käivitamine sama sektorit.

    Kui klõpsate tegevuste käivitamine, näete **Tegevuste käivitamine üksikasjad** tera logifailid loendiga. Näete sisse loginud sõnumid kuvatakse **kasutaja\_väärtust 0. log** fail. Tõrke ilmnemisel kuvatakse kolm tegevuse käivitatakse, kuna proovi uuesti arv väärtuseks 3 müügivõimaluste/tegevuse JSON. Kui klõpsate tegevuste käivitamine, näete logi faile, mis võimaldab teil läbi vaadata tõrkeotsing tõrge.

    ![](./media/data-factory-data-processing-using-batch/image18.png)

    Klõpsake loendis logifailid **kasutaja-0.log**. Tulemuste **IActivityLogger.Write** meetodil on paremal.

    ![](./media/data-factory-data-processing-using-batch/image19.png)

    Märkige ruut süsteemi-0.log mis tahes süsteemi tõrketeated ja erandid.

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module

        Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully

3.  Lisage **Eelarveprojekti** faili zip-fail nii, et tõrke üksikasjade on teavet, nt **kõne virnas** tõrke ilmnemisel.

4.  Kõik failid zip-fail kohandatud toimingute jaoks peavad olema **kõrgeima taseme** pole alamkaustad.

    ![](./media/data-factory-data-processing-using-batch/image20.png)

5.  Veenduge, et **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) ja **packageLinkedService** (peaks käsk Azure'i bloobimälu, mis sisaldab zip-fail) seatud õige väärtused.

6.  Kui soovite pärast kindlaksmääratud tõrke töötama ümber sektorit, paremklõpsake **OutputDataset** tera sektorit ja klõpsake nuppu **Käivita**.

    ![](./media/data-factory-data-processing-using-batch/image21.png)

    > [AZURE.NOTE] 
    > Näete oma Azure'i bloobimälu nimega **container** : **adfjobs**. See ümbris ei kustutata automaatselt, kuid võite turvaline selle kustutada, kui olete lõpetanud testimine lahendus. Samuti loob andmete Factory lahendus on Azure'i paketi **töö** nimega: **adf -\<pool ID/nimi\>: töö-0000000001**. Kui olete katse lahendus soovi korral saate kustutada selle töö.
7. Kohandatud tegevuse ei kasuta teie paketi **app.config** faili. Seetõttu, kui teie kood loeb failist konfiguratsiooni mis tahes ühendusstringi, see ei tööta käitusajal. Kui Azure'i paketi abil hoida mis tahes saladusi on **Azure KeyVault**, on parim subjekt serdi-põhiste teenuste abil soovitud keyvault kaitse ja levitamine serdi Azure'i paketi ühendada. .Net-i kohandatud tegevuse siis pääsevad saladusi KeyVault käitusajal. See lahendus on üldine ja mis tahes tüüpi salajane, mitte ainult ühendusstringi saab skaala.

    On mõni lihtsam lahendus (kuid mitte parim): saate luua **lingitud Azure SQL-i teenus** on ühendusega stringi sätted, andmekomplekti, mis kasutab lingitud teenuse loomine ja keti andmekomplekti fiktiivne Sisestuskeel andmekomplekti kohandatud .NET tegevusele nimega. Pääsete juurde siis lingitud teenuse ühendusstringi kohandatud tegevuse kood ja see peaks töötama hästi käitusajal.  

#### <a name="extend-the-sample"></a>Valimi laiendamine

Saate laiendada selle valimi Azure'i andmed Factory ja Azure'i paketi funktsioonide kohta. Protsessi erinevate ajavahemiku sektorit, tehke järgmist.

1.  Lisada järgmised alamkaustad **inputfolder**: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 ja koht sisestamise failid need kaustad. Piirkonnast ja lõpuaja muutmine `2015-11-16T05:00:00Z` et `2015-11-16T10:00:00Z`. **Diagrammivaate**topeltklõpsake **InputDataset**ja veenduge, et Sisestuskeel sektorid on valmis. Topeltklõpsake **OuptutDataset** väljundi sektorid oleku kuvamiseks. Kui nad on valmis olekus, märkige ruut outputfolder väljund faili.

2.  Suurendage või vähendage **kokkulangevus** säte mõista, kuidas see mõjutab teie lahendus, eriti Azure'i paketi toimub töötlemine jõudlus. (Vt samm 4: loomine ja käivitamine tulemas Lisateavet sätte **kokkulangevus** .)

3.  Saate luua on suurem/väiksem **maksimum ülesannete kohta VM**. Teil on loodud uue kausta kohta leiate Azure'i paketi värskendada lingitud teenuse andmete Factory lahendus. (Vt samm 4: loomine ja käivitamine tulemas säte **maksimaalne tööülesannete VM kohta** lisateavet.)

4.  Looge Azure'i paketi kohapeal on **autoscale** funktsiooni. Automaatselt skaleerimist Arvuta sõlmed on Azure'i paketi pargis on dünaamiline muutmine teie rakendus kasutab power töötlemine. Näiteks võite luua Azure'i paketi kohapeal on sihtotstarbeline VMs ja mõnda autoscale valemit, mis põhineb tööülesannete arv 0:
 
    Ühe VM kohta ootel tööülesande korraga (nt: Anna ootel tööülesanded -> viis VMs):

        pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
        $TargetDedicated = max(pendingTaskSampleVector);

    Max ühe VM korraga sõltumata arvu ootel toimingud:

        pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
        $TargetDedicated = (max(pendingTaskSampleVector)>0)?1:0;

    Üksikasjad leiate [automaatselt skaala arvutada sõlmed on Azure'i paketi kogumi](../batch/batch-automatic-scaling.md) . 

    Kui pool kasutab vaikimisi [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), paketi teenuse võib võtta valmistuda VM kohandatud tegevuse 15-30 minutit.  Kui pool on kasutusel erinevad autoScaleEvaluationInterval, paketi teenuse võtta autoScaleEvaluationInterval + 10 minuti. 
     
5. Valimi lahenduse, **käivitamine** meetod käivitab **Calculate** meetod, mis töötleb mõnda sisendandmete sektorit, andes mõne väljundi andmete sektorit. Saate kirjutada oma meetod Sisestuskeel andmete töötlemiseks ja asendage Calculate meetod kõne täita meetod kõne oma meetod.

 


### <a name="next-steps-consume-the-data"></a>Järgmised sammud: andmete kasutamine

Pärast töödelda, saab seda kasutada koos võrgutööriistad nagu **Microsoft Power BI**. Siin on lingid, mis aitavad teil mõista Power BI ja kuidas seda kasutada Azure.

-   [Power BI andmekomplekt uurimine](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-data/)

-   [Power BI Desktopi kasutamise alustamine](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/)

-   [Power BI andmete värskendamine](https://powerbi.microsoft.com/en-us/documentation/powerbi-refresh-data/)

-   [Azure'i ja Power BI - põhi ülevaade](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Viited

-   [Azure'i andmed Factory](https://azure.microsoft.com/documentation/services/data-factory/)

    -   [Azure'i andmed Factory teenuse tutvustus](data-factory-introduction.md)

    -   [Azure'i andmed Factory kasutamise alustamine](data-factory-build-your-first-pipeline.md)

    -   [Kasutage kohandatud tegevuste on Azure andmete Factory müügivõimaluste](data-factory-use-custom-activities.md)

-   [Azure'i paketi](https://azure.microsoft.com/documentation/services/batch/)

    -   [Azure'i paketi põhialused](../batch/batch-technical-overview.md)

    -   [Azure'i paketi funktsioonide ülevaade](../batch/batch-api-basics.md)

    -   [Saate luua ja hallata Azure'i paketi konto Azure'i portaalis](../batch/batch-account-create-portal.md)

    -   [Azure'i paketi teegi .NET kasutamise alustamine](../batch/batch-dotnet-get-started.md)


[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx

