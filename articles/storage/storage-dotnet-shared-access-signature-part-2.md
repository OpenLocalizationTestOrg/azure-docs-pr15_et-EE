<properties
    pageTitle="Loomine ja kasutamine on SAS bloobimälu | Microsoft Azure'i"
    description="Selle õpetuse näete, kuidas luua ühiskasutusega juurdepääsu allkirjade kasutamiseks bloobimälu ja kuidas kasutada neid oma kliendi rakendustest."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Ühiskasutusse antud juurdepääs allkirju osa 2: Loomine ja kasutamine on SAS bloobimälu

## <a name="overview"></a>Ülevaade

Selle õpetuse uurida [1 osa](storage-dotnet-shared-access-signature-part-1.md) ühiskasutusse antud juurdepääs allkirjad (SAS) ja ülevaade nende kasutamise head tavad. 2 osa näitab, kuidas luua ja seejärel kasutage ühiskasutusega juurdepääsu allkirjade bloobimälu abil. Näiteid C# kirjutada ja Azure salvestusruumi kliendi teek .net-i jaoks kasutada. Stsenaariumid, mis hõlmab järgmised nende aspektide töötamine ühiskasutusega juurdepääsu allkirjad.

- Ühiskasutusega juurdepääsu allkirja ümbris genereerimine
- Ühiskasutusega juurdepääsu allkirja lisamine bloobimälu genereerimine
- Salvestatud Accessi poliitika haldamiseks allkirjade lisamine container ressursid loomine
- Ühiskasutusega juurdepääsu allkirjad kliendi rakendus testimine

## <a name="about-this-tutorial"></a>Selle õpetuse kohta

Selles õpetuses me keskenduda loomise ühiskasutusega juurdepääsu signatuure ümbriste ja plekid, luues kaks konsooli rakendusi. Ühiskasutusega juurdepääsu allkirjade ümbris ja soovitud bloobimälu loob esimese konsooli rakendus. See rakendus on teadmisi salvestusruumi konto klahvikombinatsioone. Teise konsooli rakendus, mis toimivad kliendi rakendus kasutab container ja bloobimälu ressursid abil loodud esimese ühiskasutusega juurdepääsu allkirjad. See rakendus kasutab ühiskasutusega juurdepääsu allkirjad ainult selle juurdepääsu ümbris autentimiseks ja Bloobivahemälu ressursid – ei saa teadmisi konto klahvikombinatsioone.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Osa 1: Luua ühiskasutusega juurdepääsu allkirjade konsooli rakenduse loomine

Esmalt veenduge, et .net-i installitud on Azure salvestusruumi kliendi teek. Saate installida [Nugeti pakett](http://nuget.org/packages/WindowsAzure.Storage/ "Nugeti paketi") , mis sisaldab kõige ajakohasemat assemblereid kliendi teegi; See on soovitatav meetod tagada, et olete Viimati tehtud parandused. Samuti saate alla laadida kliendi teek osana [Azure'i SDK .net-i](https://azure.microsoft.com/downloads/)uusima versiooni.

Visual Studio, luua uue rakenduse Windowsi konsooli ja pange sellele **GenerateSharedAccessSignatures**. Lisage viited **Microsoft.WindowsAzure.Configuration.dll** ja **Microsoft.WindowsAzure.Storage.dll**, kasutades ühte järgmistest võimalustest.

-   Kui soovite installida Nugeti paketi, esmalt installima [Nugeti klient](https://docs.nuget.org/consume/installing-nuget). Visual Studio, valige **projekti | Hallata Nugeti pakettide**, **Azure salvestusruum**võrgus otsimine ja järgige juhiseid, et installida.
-   Teise võimalusena Azure'i SDK installi koosolekutel otsimine ja lisage need viited.

Lisage Program.cs faili ülaosas **abil** järgmistest.

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

App.config faili redigeerida, nii et see sisaldaks Otsingukonfiguratsiooni säte ühendusstring, mis viitab salvestusruumi konto abil. App.config faili peaks välja nägema umbes selline:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Luua ühiskasutatava Accessi signatuuri URI ümbris

Kõigepealt lisame meetodi luua uus ümbris ühiskasutusega juurdepääsu allkirja. Sel juhul ei ole allkirja seostatud salvestatud Accessi poliitika, nii, et see viib URI teave, mis näitab selle aegumise aeg ja õigused, mis seda määrab.

Esmalt lisage kood juurdepääsu konto salvestusruumi autentimiseks ja luua uus ümbris **Main()** meetodit.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Järgmisena lisage uus meetod, mis loob ümbris ühiskasutusega juurdepääsu signatuur ja tagastab allkirja URI:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Lisage järgmised read meetod **Main()** enne kõne **Console.ReadLine()**, kõne **GetContainerSasUri()** ja kirjutada allkirja URI konsooli akna allosas.

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Kompileerida ja käivitage väljund ühiskasutusega juurdepääsu signatuur URI uue ümbrises. URI on sarnane järgmist URI:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Pärast koodi käitamist ühiskasutusega juurdepääsu allkirja ümbris loodud ei kehti järgmise 24 tunni jooksul. Allkirja annab kliendi õigust loendi plekid ümbrises ja kirjutamise uue bloobimälu ümbris.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Luua ühiskasutatava Accessi signatuuri URI on bloobimälu

Järgmiseks me kirjutada sarnase kood luua uue bloobimälu pakendi sees ja seda ühiskasutusse antud juurdepääs signatuuri luua. Ühiskasutusega juurdepääsu allkirja ei ole seotud salvestatud Accessi poliitika, nii, et see sisaldab algusaeg, aeg ja õiguste kohta URI.

Lisada uus meetod, mis loob uue bloobimälu ja kirjutage tekst, seejärel loob ühiskasutusega juurdepääsu allkirja ja tagastab allkirja URI.

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Meetod **Main()** allosas lisada järgmised read enne kõne **Console.ReadLine()**, **GetBlobSasUri()**, helistamiseks ja kirjutada ühiskasutusega juurdepääsu allkirja URI konsooli akna:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Kompileerida ja käivitage väljund ühiskasutusega juurdepääsu signatuur URI uue bloobimälu. URI on sarnane järgmist URI:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Salvestatud Accessi poliitika oleval loomine

Nüüd oleval, määrata mis tahes ühiskasutusega juurdepääsu allkirju, mis on seotud selle piiranguid loomine salvestatud juurdepääsu poliitika.

Eelmistes näidetes, oleme määratud algus kellaaeg (otseselt või kaudselt), aegumise aeg ja ühiskasutusega juurdepääsu allkirja URI ise õigused. Järgmistes näidetes me täpsustatakse need salvestatud Accessi poliitika, mitte ühiskasutusega juurdepääsu allkirja. Nii võimaldab meil muuta nende piiranguid ilma kordusväljaannete ühiskasutusega juurdepääsu allkirja.

On võimalik, et üks või mitu ühiskasutusega juurdepääsu allkirja piiranguid ja ülejäänud salvestatud Accessi poliitika kohta. Siiski saate ainult määrata algusaeg, aeg ja õiguste ühes kohas või Näiteks ei saa ühiskasutusega juurdepääsu allkirjastamise õiguste määramine ja need määrata ka salvestatud Accessi poliitika kohta.

Pange tähele, et access poliitika lisamisel ümbris, peate kõigepealt ümbrist olemasoleva õiguste hankimine, lisada uue juurdepääsu poliitika ja seejärel kõigepealt ümbrist õiguste määramine.

Lisage uus meetod, mis loob uue poliitika salvestatud Accessi ümbris ja tagastab poliitika nimi.

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

Allosas **Main()** meetodit, enne kõne **Console.ReadLine()**, lisada järgmised read esimese Tühjenda kõik olemasolevad juurdepääsupoliitikaid ja seejärel kõne **CreateSharedAccessPolicy()** meetodit:    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Pange tähele, et kui tühjendate juurdepääsu poliitika kohta ümbris, peate esmalt saada kõigepealt ümbrist olemasolevaid õigusi, siis tühjendage nende õiguste, siis kasutusõiguse uuesti.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Luua ühiskasutatava Accessi signatuuri URI-ümbrisest, mis kasutab mõnda Accessi poliitika kohta

Järgmiseks loome teise ühiskasutusse antud juurdepääs allkirja ümbris loodud varasemas versioonis, kuid seekord meil kuvatakse Accessi poliitika eelmises näites me loodud signatuuri seostada.

Lisage uus meetod luua teise ühiskasutusse antud juurdepääs signatuur ümbris.

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Allosas **Main()** meetodit, enne kõne **Console.ReadLine()**, lisada järgmised read **GetContainerSasUriWithPolicy** meetodit kutsuda:

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Luua ühiskasutatava Accessi signatuuri URI bloobimälu, mis kasutab mõnda Accessi poliitika kohta

Lõpuks lisame sarnase meetodi loomine mõne muu bloobimälu ja luua ühiskasutusega juurdepääsu allkirja, mis on seostatud mõne juurdepääsu poliitika.

Uus meetod on bloobimälu loomine ja luua ühiskasutusega juurdepääsu allkirja lisamiseks tehke järgmist.

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Allosas **Main()** meetodit, enne kõne **Console.ReadLine()**, lisada järgmised read **GetBlobSasUriWithPolicy** meetodit kutsuda:    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

**Main()** meetod peaks olema nüüd selline tervikuna. Käivitage see kirjutamise konsooli akna ühiskasutusega juurdepääsu allkirja URI-d ja seejärel kopeerige ja kleepige need tekstifaili selles õpetuses teine osa kasutamiseks.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

GenerateSharedAccessSignatures konsooli rakenduse käivitamisel kuvatakse väljundi aknas konsooli järgmise sisuga. Need on osa 2 õpetuse kasutamiseks ühiskasutusega juurdepääsu allkirjad.

![SAS-konsool-väljundi-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Osa 2: Konsooli testimiseks allkirjad ühiskasutatava Accessi rakenduse loomine

Ühiskasutusega juurdepääsu allkirjad eelmistes näidetes loodud testimiseks loome teise konsooli rakendus kasutava allkirjad ümbris ja soovitud bloobimälu toimingute tegemiseks.

> [AZURE.NOTE] Kui olete lõpule jõudnud õpetuse esimene osa on möödunud rohkem kui 24 tundi, loodud allkirjad enam ei kehti. Sel juhul peaks töötama koodi esimese konsooli rakendus luua teine osa õpetuse värske ühiskasutusega juurdepääsu allkirjade kasutamiseks.

Visual Studio, luua uue rakenduse Windowsi konsooli ja pange sellele **ConsumeSharedAccessSignatures**. Lisage viited **Microsoft.WindowsAzure.Configuration.dll** ja **Microsoft.WindowsAzure.Storage.dll**, nagu varem.

Lisage Program.cs faili ülaosas **abil** järgmistest.

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Kehas **Main()** meetod, lisage järgmised konstantide ja ühiskasutusega juurdepääsu allkirjad õpetuse 1 osa genereerinud nende väärtuste värskendamine.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Meetodi proovimiseks Container toimingute abil ühiskasutusse antud juurdepääs signatuuri lisamine

Seejärel lisame meetod, mis testib mõned tüüpilised container toimingute kaudu ühiskasutusse antud juurdepääs signatuuri ümbris. Pange tähele, et ühiskasutusega juurdepääsu signatuuri kasutatakse tagastab viite ümbris, autentimine juurdepääsu s.o ümbris, mille põhjal eraldi allkirja.

Lisage Program.cs järgmisel viisil:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Värskendage **Main()** meetodit helistamiseks **UseContainerSAS()** nii ümbris loodud ühiskasutusega juurdepääsu allkirjad.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Meetodi proovimiseks bloobimälu toimingute abil ühiskasutusse antud juurdepääs signatuuri lisamine

Lõpuks lisame meetod, mis testib tüüpilised bloobimälu tegevuse abil ühiskasutusse antud juurdepääs signatuuri soovitud bloobimälu. Sel juhul kasutame ehitaja **CloudBlockBlob(String)**, läbides ühiskasutusega juurdepääsu signatuuri, tagastatakse funktsiooni bloobimälu. Muud Autentimiseta on vaja; See põhineb eraldi allkirja.

Lisage Program.cs järgmisel viisil:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Värskendage **Main()** meetodit helistamiseks **UseBlobSAS()** nii on bloobimälu loodud ühiskasutusega juurdepääsu allkirjad.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Käivitage rakendus konsooli ja väljundi näha, millised toimingud on lubatud, milline allkirjade jälgida. Väljundi aknas konsooli näeb välja umbes järgmine:

![SAS konsooli väljundi 2][sas-console-output-2]

## <a name="next-steps"></a>Järgmised sammud

[Ühiskasutusse antud juurdepääs allkirju osa 1: SAS andmemudeli mõistmine](storage-dotnet-shared-access-signature-part-1.md)

[Ümbriste ja plekid anonüümse lugemisõigus haldamine](storage-manage-access-to-resources.md)

[Delegeeriv juurdepääsu ühiskasutusega Accessi allkirjastatud (REST API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Tabeli ja järjekorda SAS tutvustus](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
