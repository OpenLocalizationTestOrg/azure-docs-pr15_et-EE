<properties 
    pageTitle="Failide üleslaadimine Media Servicesi kontole .net-i abil | Microsoft Azure'i" 
    description="Saate teada, kuidas tuua meediumisisu Media Services abil luua ja üles laadida varad." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Failide üleslaadimine Media Servicesi kontole .net-i abil

 > [AZURE.SELECTOR]
 - [.NET-I](media-services-dotnet-upload-files.md)
 - [ÜLEJÄÄNUD](media-services-rest-upload-files.md)
 - [Portaal](media-services-portal-upload-files.md)

Media Services, saate üles laadida (või neelata) oma digitaalse vara failid. **Varade** üksus võib sisaldada video, heli, pilte, pisipiltide saidikogumid, teksti jälitab ja tiitrite failide (ja nende failide metaandmeid.)  Kui failid on üles laaditud, talletatakse teie sisu turvaliselt edasiseks töötlemiseks ja streaming pilveteenuses.

Vara failid nimetatakse **Varade failid**. **AssetFile** eksemplar ja tegeliku media fail on kaks erinevat objekti. AssetFile eksemplari sisaldab metaandmeid meediumifail, samal ajal media fail sisaldab tegelik meediumisisu.

>[AZURE.NOTE]Järgmisega kehtivad valimisel kuvatakse varade faili nimi.
>
>- Media Services kasutab IAssetFile.Name atribuudi väärtus, kui hoone URL-ide streaming sisu (nt http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Seetõttu protsentkodeering pole lubatud. **Nime** atribuudi väärtus ei tohi olla järgmised [protsendi-kodeeringus-reserveeritud märgid](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Samuti ei saa ühte "." failinimelaiend jaoks.
>
>- Nime pikkus ei peaks olema suurem kui 260 märki.

Kui loote varad, saate määrata krüptimise järgmistest suvanditest. 

- **Ükski** - krüptimist kasutatakse. See on vaikeväärtus. Pange tähele, et selle suvandi kasutamisel sisu pole kaitstud teel või ülejäänud salvestusruumi.
Kui plaanite esitamisel on MP4 abil järk-järgult allalaadimine, kasutage seda suvandit. 
- **CommonEncryption** – kasutage seda suvandit, mis on juba krüptitud ja levinud krüptimise või PlayReady DRM (nt sujuva voogesituse kaitstud PlayReady DRM) kaitsega sisu üleslaadimiseks.
- **EnvelopeEncrypted** – kasutage seda suvandit HLS krüptitud AES üleslaadimiseks. Pange tähele, et failid on kodeeritud ja muuta halduri krüptitud.
- **StorageEncrypted** - krüptib kohalikult abil AES-256 bit krüptimist ja seejärel seda Azure Storage, kui see on salvestatud krüptitud lisatud ülejäänud Eemalda sisu. Varade salvestusruumi krüptimise abil kaitstud automaatselt krüptimata ja enne kodeerimine süsteemi krüptitud faili asukoht ja soovi uuesti krüptitud enne üleslaadimist tagasi nimega uue väljundi varade. Salvestusruumi krüptimise esmane kasutamine puhul on, kui soovite oma kõrge kvaliteediga Sisestuskeel meediumifaile tugev krüptimist ja ülejäänud vaba.

    Media Servicesi pakub kettal salvestusruumi krüptimine oma varasid ei üle-the-kaabel nagu digitaalõiguste Manager (DRM).

    Kui teie vara on krüptitud salvestusruumi, tuleb konfigureerida varade kohaletoimetamise poliitika. Lisateabe saamiseks vt [seadistamine varade kohaletoimetamise poliitika](media-services-dotnet-configure-asset-delivery-policy.md).

Kui määrate oma varade **CommonEncrypted** suvand või suvandi **EnvelopeEncypted** krüptimist, peate mõne **ContentKey**oma varade seostada. Lisateabe saamiseks vaadake, [Kuidas luua mõne ContentKey](media-services-dotnet-create-contentkey.md). 

Kui määrate oma varade **StorageEncrypted** suvandi krüptimist, loob Media Services SDK .net-i **StorateEncrypted** **ContentKey** teie vara.


Selles teemas kirjeldatakse, kuidas kasutada Media Services .NET SDK kui ka Media Services .NET SDK laiendid failide üleslaadimine rakendusse Media Servicesi vara.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Media Services .NET SDK ühe faili üleslaadimine 

Proovi kood allpool kasutab .NET SDK teha järgmisi toiminguid: 

- Loob tühja vara.
- Loob AssetFile eksemplari, mille soovime vara seostada.
- Loob AccessPolicy eksemplari, mis määratleb õiguste ja Accessi kestus vara.
- Loob Locator eksemplari, mis pakub juurdepääsu vara.
- Meediumi teenustesse lisatud Üksik meediumifail. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Media Services .NET SDK mitme faili üleslaadimine 

Järgmine kood näitab, kuidas luua vara ja mitme faile üles laadida.

Koodi teeb järgmist.
    
-   Loob tühja vara eelmises etapis määratletud CreateEmptyAsset meetodi abil.
    
-   Loob **AccessPolicy** eksemplari, mis määratleb õiguste ja Accessi kestus vara.
    
-   Loob **Locator** eksemplari, mis pakub juurdepääsu vara.
    
-   Loob **BlobTransferClient** eksemplari. Seda tüüpi tähistab klient, mis toimib Azure plekid. Selles näites me kasutame kliendi üleslaadimise edenemist jälgida. 
    
-   Loetleb kaudu määratud kausta failide ja loob **AssetFile** eksemplari iga faili.
    
-   Lisatud failid Media Servicesi **UploadAsync** meetodi abil sisse. 
    
>[AZURE.NOTE] UploadAsync meetodi abil saate tagada, et kõned ei blokeeri ja samal ajal soovitud failid üles.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Kui suure hulga varad üles, arvestage järgmist.

- Looge uus **CloudMediaContext** objekt teema kohta. Klassi **CloudMediaContext** ei ole jutulõnga ohutu.
 
- Suurendada NumberOfConcurrentTransfers vaikeväärtust, milleks on 2-nagu 5 suurem väärtus. Selle atribuudi mõjutab **CloudMediaContext**kõik eksemplarid. 
 
- Hoia ParallelTransferThreadCount vaikeväärtust, milleks on 10.
 
##<a id="ingest_in_bulk"></a>Söömisega varad mitmekaupa Media Services .NET SDK abil 

Suure varade failide üleslaadimise võib olla kitsaskoht varade loomise ajal. Söömisega varad hulgi või "Hulgi söömisega", hõlmab lahutamine varade loomise üleslaadimine. Suure hulga söömisega lähenemine kasutamiseks luua manifestis (IngestManifest) vara ja seostatud failide kirjeldav. Seejärel ja seostatud failide üleslaadimine lastimanifesti bloobimälu container teie valitud meetodit üleslaadimise abil. Microsoft Azure Media Servicesi jälgimine bloobimälu ümbris seostatud manifest. Faili üleslaaditud bloobimälu container lõpetab Microsoft Azure Media Servicesi varade loomiskuupäeva manifestis (IngestManifestAsset) vara konfiguratsiooni põhjal.


Luua uue IngestManifest kõne loomine meetod, mis on esitatud selle CloudMediaContext IngestManifests kogumisega. See meetod loob uue IngestManifest esitate manifest nimega.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Looge varad, mis on seostatud hulgi IngestManifest. Konfigureerige vara for Hulgiimpordi söömisega krüptimise soovitud suvandid.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Mõne IngestManifestAsset seostab vara hulgi IngestManifest for Hulgiimpordi söömisega. See seostab ka iga vara moodustavaid AssetFiles. Mõne IngestManifestAsset loomiseks kasutage serveri kontekstist loomine meetodit.

Järgmises näites näitab, lisades kaks uut IngestManifestAssets, mis varem loodud hulgilisamine kaks vara seostada neelata manifesti. Iga IngestManifestAsset seob kogum faile, mis laaditakse iga üksuse jaoks ka ajal hulgi söömisega.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Saate kasutada mis tahes vara failide üleslaadimine bloobimälu salvestusruumi container URI **IIngestManifest.BlobStorageUriForUpload** atribuut on IngestManifest, mida saab kiire klientrakendusega. Ühe kiire märkimisväärne üleslaadimise teenus on [Aspera On Demand Azure'i rakenduse](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Samuti saate kirjutada koodi varad faile, nagu on näidatud järgmises näites kood üles laadida.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

Selles teemas kasutatud valimi varade failide üleslaadimise kood kuvatakse järgmine kood näide.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Saate määratleda edenemise hulgi söömisega kõikide varade statistika atribuut, **IngestManifest**Küsitlused on **IngestManifest** seotud. Edenemise teabe värskendamiseks peate kasutama uut **CloudMediaContext** iga kord, kui te küsitlus statistika atribuuti.

Järgmises näites näitab Küsitlused on IngestManifest oma **ID-d**.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Kasutades .NET SDK laiendid failide üleslaadimine 

Järgmises näites kujutatakse üles laadida ühte faili, kasutades .NET SDK laiendid. Selles näites kasutatakse **CreateFromFile** meetodit, kuid asünkroonne versioon on ka (**CreateFromFileAsync**). **CreateFromFile** meetodit saate määrata faili nime, krüptimise suvand ja pakkumist selleks, et aruande faili üleslaadimine edenemist.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

Järgmises näites nõuab UploadFile funktsioon ja määrab salvestusruumi krüptimise varade loomise võimalus.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Järgmise juhise juurde

Nüüd, kui teil on üleslaaditud vara Media Servicesi, avage teema [hankimine meediumi protsessor][] .

[Kuidas saada meediumi protsessor]: media-services-get-media-processor.md
 
