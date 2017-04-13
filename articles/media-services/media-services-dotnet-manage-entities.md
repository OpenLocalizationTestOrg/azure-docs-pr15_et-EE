
<properties 
    pageTitle="Varad ja seotud üksuste Media teenustega .NET SDK haldamine" 
    description="Saate teada, kuidas hallata varad ja seotud üksuste Media Services SDK .net-i jaoks." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Varad ja seotud üksuste meediumi teenustega .NET SDK haldamine


> [AZURE.SELECTOR]
- [.NET-I](media-services-dotnet-manage-entities.md)
- [ÜLEJÄÄNUD](media-services-rest-manage-entities.md)


Selles teemas kirjeldatakse, kuidas täita Media Servicesi halduse järgmist:

- Saada mõne varade viide 
- Saada töö viide 
- Loetle kõik varad 
- Loendi töökohtade ja varad 
- Loetle kõik juurdepääsupoliitikaid 
- Loetle kõik lokaatorid
- – Suured saidikogumid üksuste nummerdamine
- Vara kustutamine 
- Töö kustutamine 
- Accessi poliitika kustutamine 

##<a name="prerequisites"></a>Eeltingimused 

Vt [keskkonna häälestamine](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Saada mõne varade viide

Sagedased ülesanne on avada olemasoleva vara viide Media Services. Koodi järgmises näites on kujutatud, kuidas saab on varade viide varad kollektsioonist serveris kontekstis objekti, võttes aluseks vara ID-ga.
Järgmine kood näide kasutab Linq päringu saada olemasoleva IAsset objekti viide.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Saada töö viide

Tööülesannete Media Servicesi koodi töötlemine töötamisel sageli peate hankima viide mõne olemasoleva töö põhjal Id. Koodi järgmises näites on kujutatud hankimine IJob objekti viide kollektsioonist tööde haldamine.
WarningWarning võib-olla peate saada töö viide pikaajalisi kodeering töö käivitamisel ja töö oleku kohta teema on vaja. Juhtudel, kui meetodit tagastab jutulõnga, peate värskendatud viide töö toomiseks.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Loetle kõik varad

Kui arv on salvestusruumi varad kasvab, oleks kasulik oma varasid loendis. Koodi järgmises näites kujutatakse kordamiseks varad saidikogumi Serveri konteksti objekti kaudu. Iga vara, kus koodi näide kirjutab osa selle atribuudi väärtuste konsooli. Näiteks iga vara võib sisaldada palju meediumifaile. Koodi näide kirjutab välja kõik failid, mis on seotud iga vara.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Loendi töökohtade ja varad

Loendi varad seotud tööga Media teenused on oluline seotud ülesanne. Koodi järgmises näites kirjeldatakse, kuidas loendis iga IJob objekti, ja seejärel jaoks iga töö väärtustamisel kuvatakse andmete allalaadimise kohta töö atribuudid, kõik seostuvad ülesanded, kõik sisestada, ja kõik väljundi vara. Selles näites kood võib olla kasulik paljude muude toimingute tegemiseks. Kui soovite loendisse väljundi varad kodeering töökohta, mis varem käivitasite, kuvatakse järgmine kood väljundi varad juurdepääsemise kohta. Kui teil on viide väljundi vara, saate pakkuda sisu teistele kasutajatele või rakenduste allalaadimist või URL-ide esitada. 

Pakkuda varad suvandite kohta leiate lisateavet teemast [Media Services SDK .net-i varad esitamisel](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Loetle kõik Juurdepääsupoliitikaid

Media Services, saate määratleda Accessi poliitika vara või asuvaid faile. Accessi poliitika määratleb õiguste faili või vara (mis tüüpi Accessi ja kestus). Media Servicesi koodi, saate määratleda tavaliselt on juurdepääs poliitika luues IAccessPolicy objekti ja seejärel seostada see olemasoleva vara. Looge ILocator objekti, mis võimaldab teil Media Servicesi varasid otsest juurdepääsu anda. Visual Studio projekt, mis kaasneb dokumentatsioon selle sarja sisaldab mitmeid koodi näiteid, mis näitab, kuidas luua ja määrata juurdepääsupoliitikaid ja lokaatorid varad.

Järgmine kood näide näitab kõigi juurdepääsupoliitikaid serveris loendite ja omavahel seotud õiguste tüübi. Teine kasulik viis juurdepääsupoliitikaid kuvamiseks on loendi kõigi ILocator objektide serveris ja seejärel iga locator jaoks saate loetleda seotud Accessi poliitika atribuudi AccessPolicy abil.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Loetle kõik lokaatorid

Mõne locator on URL, mis pakub juurdepääsu vara, õiguste ja vara koos selle locator seotud juurdepääsu poliitika määratletud otsene tee. Iga võib olla seotud selle atribuudi lokaatorid ILocator objektide kogum. Serveri konteksti on ka lokaatorid kogumi, mis sisaldab kõiki lokaatorid.

Järgmine kood näide on loetletud kõik lokaatorid serveris. See näitab iga locator jaoks Id seotud varade ja juurdepääsu poliitika. Samuti kuvatakse vara tüüpi õigused, aegumiskuupäeva ja täielik tee.

Pöörake tähelepanu sellele, locator tee vara ainult base URL-i vara. Üksikuid faile, mida kasutaja või rakendus võib sirvige otsene tee loomiseks peab teie koodi lisada locator tee teatud faili tee. Lisateavet selle kohta, kuidas seda teha, lugege teemat [Esitamisel varad Media Services SDK .net-i jaoks](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>– Suured saidikogumid üksuste nummerdamine

Kui päringu üksused, on piirang 1000 üksuste tagasi korraga, sest avaliku ülejäänud v2 piirab päringutulemite 1000 tulemusi. Peate kasutama jäta ja võta – suured saidikogumid üksuste nummerdamine. 
    
Järgmise funktsiooni tsüklid kõik tööde esitatud Media Services konto kaudu. Media Servicesi tagastab tööde haldamine saidikogumi 1000 tööde haldamine. Funktsioon kasutab jäta ja veenduge, et kõik tööd tee on loetletud (juhuks, kui teil on üle 1000 töö teie konto).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Vara kustutamine

Järgmises näites kustutab vara.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Töö kustutamine

Töö kustutamiseks märgite ruudu töö olek toodud atribuudi olekus. Kustutada saab, tööd, mis on lõpule jõudnud või tühistada, kui tööd, mis on teatud teistes riikides, nt ootel, ajastatud või töötlemist, tuleb esmalt tühistada, ja seejärel saate kustutada.
Koodi järgmises näites on kujutatud meetod töö kustutamise kontrollimine töö Ühendriigid ja seejärel kustutades, kui olek on lõpule jõudnud või on tühistatud. Järgmine kood sõltub eelmises jaotises selles teemas saada tööd viide: saada töö viide.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Accessi poliitika kustutamine

Järgmine kood näide näitab, kuidas saada juurdepääsu poliitika, mis põhineb poliitika ID-d, ning seejärel kustutage poliitika.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
