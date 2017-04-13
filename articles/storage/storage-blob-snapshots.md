<properties
    pageTitle="Luua mõne bloobimälu kirjutuskaitstud hetktõmmise | Microsoft Azure'i"
    description="Saate teada, kuidas luua bloobimälu, bloobimälu varundada antud ajahetkel hetktõmmise. Mõista, kuidas hetktõmmiseid on arve ja kuidas neid kasutada minimeerimiseks eest."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="create-a-blob-snapshot"></a>Bloobimälu hetktõmmise loomine

## <a name="overview"></a>Ülevaade

Hetktõmmise on kirjutuskaitstud versioon bloobimälu, mis on võetud samal ajal aega. Hetktõmmiseid on kasulikud plekid varundada. Kui olete loonud hetktõmmise, saate lugeda, kopeerida või kustutada, kuid te ei saa seda muuta.

Mõne bloobimälu hetktõmmise on identne selle base bloobimälu, välja arvatud bloobimälu URI on lisatud hetktõmmis on võetud ajapunkti URI bloobimälu **kuupäeva ja kellaaja** väärtuse. Näiteks kui leht Bloobivahemälu URI on `http://storagesample.core.blob.windows.net/mydrives/myvhd`, URI on sarnane hetktõmmis `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Kõik pilte base bloobimälu URI ühiskasutusse anda. Ainult vahet base bloobimälu ja hetktõmmis on lisatud **kuupäeva ja kellaaja** väärtuse.

Mõne bloobimälu võib olla mõni muu arv pilte. Hetktõmmiseid püsida kuni konkreetselt kustutatakse. Hetktõmmise ei saa selle base bloobimälu minetaksid. Saate loetleda tõmmised seostatud base bloobimälu praeguse tõmmised jälgimiseks.

Mõne bloobimälu hetktõmmise loomisel kopeeritakse selle bloobimälu Süsteemiatribuudid hetktõmmise sama väärtusega. Base bloobimälu metaandmete kopeeritakse ka hetktõmmise, kui te ei määra eraldi metaandmete hetktõmmis selle loomisel.

Mis tahes liisingute seostatud base bloobimälu ei mõjuta hetktõmmis. Te ei saa omandada liisingueset hetktõmmise.

## <a name="create-a-snapshot"></a>Hetktõmmise loomine

Koodi järgmises näites kujutatakse .NET hetktõmmise loomiseks. Selles näites määratakse eraldi metaandmete hetktõmmis loomisel.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Kopeerige pilte

Kopeeri toimingutega plekid ja pilte järgige järgmisi reegleid:

- Saate kopeerida hetktõmmise base bloobimälu oma kohale. Edendada hetktõmmise base bloobimälu asukohta, saate taastada on bloobimälu varasemas versioonis. Hetktõmmis jääb, kuid alus bloobimälu kirjutatakse hetktõmmis kirjutatav koopia.

- Saate kopeerida hetktõmmise sihtkoha bloobimälu erineva nimega. Tulemuseks sihtkoha bloobimälu on kirjutatav bloobimälu ja pole hetktõmmise.

- Andmeallika bloobimälu kopeerimisel ei kopeerita mis tahes hetktõmmiste allika bloobimälu sihtkohta. Kui sihtkoha bloobimälu kirjutatakse koopia, mis tahes pilte, mis on seotud algse sihtkoha bloobimälu jäävad samaks.

- Blokeeri bloobimälu hetktõmmise loomisel on bloobimälu kinnitatud blokeeritute loendis kopeeritakse ka hetktõmmis. Mis tahes sisseviimata plokid ei kopeerita.

## <a name="specify-an-access-condition"></a>Määrake tingimuses juurdepääs

Accessi tingimuses saate määrata, et hetktõmmis on loodud ainult juhul, kui tingimus on täidetud. Accessi tingimuses määramiseks kasutage **AccessCondition** atribuuti. Kui määratud tingimus ei ole täidetud, ei looda hetktõmmis ja bloobimälu teenus tagastab olekukoodi HTTPStatusCode.PreconditionFailed.

## <a name="delete-snapshots"></a>Hetktõmmiseid kustutamine

Ei saa kustutada mõne bloobimälu koos pilte, kui tõmmised kustutatakse ka. Saate kustutada hetktõmmise ükshaaval või määrata kõik pilte kustutatakse allika bloobimälu kustutamisel. Kui proovite kustutada bloobimälu, mis on veel pilte, on tulemuseks viga.

Koodi järgmises näites on kujutatud, kuidas kustutada mõne bloobimälu ja selle pilte rakenduses .net-i, kus `blockBlob` on muutuja tüüpi **CloudBlockBlob**:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Hetktõmmiseid Azure Premium Storage

Kasutades hetktõmmiseid Premium mälu jälgi järgmisi reegleid:

- Hetktõmmiste kohta lehe bloobimälu premium salvestusruumi konto maksimumarv on 100 eurot. Selle piirmäära ületamise korral hetktõmmise bloobimälu toiming tagastab tõrkekoodi 409 (**SnapshotCountExceeded**).

- Saate lehe bloobimälu hetktõmmise salvestusruumi premium konto iga 10 minuti järel. Kui see määr on suurem, tagastab hetktõmmise bloobimälu toimingu tõrkekood 409 (**SnaphotOperationRateExceeded**).

- Ei saa helistada saada bloobimälu lehe bloobimälu premium salvestusruumi konto hetktõmmise lugeda. Saada bloobimälu hetktõmmise premium salvestusruumi konto, kutsudes tagastab tõrkekoodi 400 (**InvalidOperation**). Siiski saate helistada saada bloobimälu atribuudid ja saada bloobimälu metaandmete hetktõmmise premium salvestusruumi konto suhtes.

- Lugeda hetktõmmise, saate toimingu Kopeeri bloobimälu hetktõmmise kopeerimiseks teise lehe bloobimälu kontole. Sihtkoha bloobimälu Kopeeri toimingu jaoks ei tohi olla mis tahes olemasolevaid pilte. Kui sihtkoha bloobimälu on pilte, tagastab selle toimingu Kopeeri bloobimälu tõrkekood 409 (**SnapshotsPresent**).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Absoluutne URI naasmiseks hetktõmmise

C# koodi näites loob hetktõmmise ja kirjutab välja absoluutne URI esmane asukoht.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Kuidas lisada pilte kulude mõistmiseks

Loomise hetktõmmise, mis on saanud bloobimälu kirjutuskaitstud koopia, võib põhjustada täiendava salvestusruumi andmesidetasud kontole. Rakenduse kujundamisel on oluline, millega peaks arvestama, kuidas need võivad lisada nii, et saate minimeerida tarbetute kulude.

### <a name="important-billing-considerations"></a>Arvelduse vajavateks

Järgmine loend sisaldab põhipunktid hetktõmmise loomisel silmas pidada.

- Salvestusruumi konto andmesidekasutusele lisanduvad kordumatu plokid või lehed, kas need on bloobimälu või hetktõmmis. Oma kontole kanna hetktõmmiseid seostatud mõne bloobimälu kuni värskendamiseni bloobimälu, kus need põhinevad lisatasud. Pärast värskendamist base bloobimälu, see erineb selle pilte. Kui see juhtub, on kordumatu plokid või lehed iga bloobimälu või hetktõmmise eest tasu.

- Kui asendate plokk Blokeeri bloobimälu sees, lisandu ploki hiljem kordumatu ploki. See kehtib ka siis, kui plokk on sama plokk-ID ja samad andmed, kui see hetktõmmis. Kui plokk on kinnitatud uuesti see erineb mis tahes hetktõmmise vastava asutuse ja teil tuleb tasuda oma andmete jaoks. Sama kehtib lehe lehe bloobimälu, mida on värskendatud identse andmetega.

- Blokeeri bloobimälu asendamine **UploadFile**, **UploadText**, **UploadStream**või **UploadByteArray** meetodi asendab kõik selle bloobimälu plokid. Kui teil on seostatud selle bloobimälu hetktõmmise, kõik plokid base bloobimälu ja hetktõmmise nüüd erinevad ja teil tuleb tasuda kõigi plokid nii plekid. See kehtib ka siis, kui andmed base bloobimälu ja hetktõmmis jääma samaks.

- Azure'i bloobimälu teenus pole tähendab, et määratleda, kas kaks sisaldavad identse andmed. Iga ploki, mis on üles laaditud ja kinnitatud käsitletakse nii kordumatu ja isegi juhul, kui see on samad andmed ja sama Blokeeri ID. Kuna kulude teki kordumatu plokid, on oluline silmas pidada, et värskendamine bloobimälu, mis on hetkeseisu tulemused täiendavad kordumatu blocks ja lisatasud.

> [AZURE.NOTE] Head tavad nõuavad, et haldate hetktõmmiseid hoolikalt, et vältida lisatasud. Soovitame, et hallata hetktõmmiseid järgmisel viisil:

> - Kustutamine ja uuesti pilte, mis on seostatud mõne bloobimälu iga kord, kui värskendate bloobimälu, isegi siis, kui värskendate identse andmetega, kui teie rakendus kujundus ei nõua, et haldate hetktõmmiseid luua. Kustutamine ja uuesti luua selle bloobimälu pilte, saate tagada, et bloobimälu ja pilte erineda.

> - Kui on säilitamine soovitud bloobimälu hetktõmmiste, ärge **UploadFile** **UploadText**, **UploadStream**või **UploadByteArray** soovitud bloobimälu värskendada. Meetodid, asendage kõik plokid bloobimälu, nii, et teie base bloobimälu ja pilte erinevad, mida on oluliselt. Selle asemel, värskendage vähem võimalik arv plokid **PutBlock** ja **PutBlockList** viisidel.


### <a name="snapshot-billing-scenarios"></a>Hetktõmmis arveldus stsenaariumid


Järgmistel juhtudel näitavad, kuidas kulude tulla Blokeeri bloobimälu ja selle pilte.

1 stsenaariumi base bloobimälu pole värskendatud pärast hetktõmmis tegemist nii, et kulude tekivad ainult kordumatute plokid 1, 2 ja 3.

![Azure'i salvestusruumi ressursid](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

2 stsenaariumi base bloobimälu on värskendatud, kuid ei ole hetktõmmis. Blokeeri 3 värskendati ja isegi juhul, kui see sisaldab samad andmed ja sama ID-d, ei ole sama blokeerida 3 hetktõmmis. Seetõttu tuleb tasuda konto nelja plokid.

![Azure'i salvestusruumi ressursid](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

3 stsenaariumi base bloobimälu on värskendatud, kuid ei ole hetktõmmis. Blokeeri 3 asendati base bloobimälu plokk 4, kuid hetktõmmis peegeldab Blokeeri 3. Seetõttu tuleb tasuda konto nelja plokid.

![Azure'i salvestusruumi ressursid](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

4 stsenaariumi base bloobimälu täielikult värskendatud ja see sisaldab ükski selle algse plokid. Seetõttu tuleb tasuda konto kõik kaheksa kordumatu plokid. Sel juhul võib ilmneda kui kasutate **UploadFile**, **UploadText**, **UploadFromStream**või **UploadByteArray**, nt sisestusmeetod update, kuna nende meetodite asendamine soovitud bloobimälu sisu.

![Azure'i salvestusruumi ressursid](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Järgmised sammud

Täiendavad näiteid bloobimälu abil leiate [Azure'i koodinäiteid](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Saate valimi rakenduse allalaadimine ja käivitage see või sirvige kood github. 
