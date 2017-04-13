<properties
    pageTitle="Ümbriste ja plekid anonüümse lugemisõigus haldamine | Microsoft Azure'i"
    description="Siit saate teada, kuidas teha ümbriste ja plekid saadaval Anonüümne juurdepääs, ja kuidas neile programmiliselt juurde pääseda."
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

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Ümbriste ja plekid anonüümse lugemisõigus haldamine

## <a name="overview"></a>Ülevaade

Vaikimisi juurdepääs ainult salvestusruumi konto omanik selle konto salvestusruumi ressursse. Ainult bloobimälu, saate määrata mõne container õigused lubada anonüümse lugemisõigus ümbris ja selle plekid nii, et saate anda juurdepääsu nende ressursside ilma ühiskasutuse oma konto võti.

Anonüümse juurdepääsu on parim stsenaariumid, kus soovite teatud plekid alati kättesaadavaks tegemiseks lugege anonüümse juurdepääsu. Peenem tekstuuriga juhtelemendi, saate luua ühiskasutusega juurdepääsu allkirja, mis võimaldab volitatud esindaja piiratud juurdepääsu kasutamine eri õiguste ja teatud ajavahemiku jooksul. Ühiskasutusega juurdepääsu signatuuride loomise kohta leiate lisateavet teemast [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Ümbriste ja plekid anonüümsete kasutajate jaoks õiguste andmine

Vaikimisi ümbris ja mis tahes plekid kogumahu olla kättesaadav ainult salvestusruumi konto omanik. Avaliku juurdepääsu lubamiseks container õigusi saab seada anonüümsed kasutajad lugemisõigused ümbris ja selle plekid anda. Anonüümsed kasutajad saate lugeda plekid jooksul avalikult juurdepääsetava container ilma kutse autentimist.

Ümbriste pakuvad container juurdepääsu haldamine järgmistest suvanditest:

- **Täielik avaliku lugemisõigus:** Container ja bloobimälu andmeid saab lugeda anonüümse taotluse kaudu. Kliendid saate loetleda plekid container kaudu anonüümse taotluse sees, kuid ei saa nummerdada ümbriste salvestusruumi kontol.

- **Avaliku lugeda juurdepääs ainult plekid:** See ümbris bloobimälu andmete saab lugeda anonüümse taotluse kaudu, kuid ümbrise andmed pole saadaval. Kliendid ei saa nummerdada plekid sees container anonüümse taotluse kaudu.

- **Pole avaliku vt Accessi:** Container ja bloobimälu andmeid saab lugeda ainult konto omanik.

Container õigusi saab seada järgmiselt:

- [Azure'i portaalis](https://portal.azure.com).
- Programmiliselt, kasutades salvestusruumi kliendi teek või REST API-ga.
- PowerShelli abil. Azure'i PowerShelli kaudu container õiguste seadmise kohta leiate teemast [Azure PowerShelli kasutamine Azure Storage](storage-powershell-guide-full.md#how-to-manage-azure-blobs).

### <a name="setting-container-permissions-from-the-azure-portal"></a>Azure'i portaalis container õiguste seadmine

[Azure portaali](https://portal.azure.com)container õiguste seadmiseks tehke järgmist.

1. Liikuge armatuurlaua salvestusruumi konto jaoks.
2. Valige loendist container nimi. Klõpsake selle nime seab valitud ümbrises plekid
3. Valige tööriistaribal **juurdepääsu poliitika** .
4. Valige väljal **juurdepääsutüüp** teie soovitud õiguste tase, nagu on näidatud pildil.

    ![Dialoogiboksi Container metaandmete redigeerimine](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>Container õiguste programmiliselt .net-i abil

Kasutades .net-i kliendi teek ümbris õiguste seadmiseks esmalt tuua kõigepealt ümbrist olemasolevaid õigusi, helistades **GetPermissions** meetod. Klõpsake atribuudi **PublicAccess** **BlobContainerPermissions** objekti, mis tagastatakse **GetPermissions** meetod. Lõpetuseks, helistage värskendatud õigustega **Õiguste_andmine** meetodit.

Järgmises näites määrab kõigepealt ümbrist õiguste täielikku avaliku lugemisõigus. Seada õigusi avaliku lugemisõigus plekid ainult, määrake atribuudi **PublicAccess** **BlobContainerPublicAccessType.Blob**. Kõik õigused anonüümsed kasutajad eemaldamiseks atribuudi **BlobContainerPublicAccessType.Off**abil.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Ümbriste ja plekid anonüümselt juurdepääsuks

Kliendi, mis kasutab ümbriste ja plekid anonüümselt saate kasutada ehitajatel, mis ei nõua mandaat. Järgmised näited kujutavad viitamiseks anonüümselt bloobimälu teenuste ressursid on mitu võimalust.

### <a name="create-an-anonymous-client-object"></a>Anonüümse kliendi objekti loomine

Saate luua uue teenuse kliendi objekti anonüümse juurdepääsu bloobimälu teenuse lõpp-punkti luues konto. Kuid peate teadma nimi on container selle konto, mis on saadaval Anonüümne juurdepääs.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>Ümbris anonüümselt viitamine.

Kui teil on ümbrises, mis on saadaval anonüümselt URL-i, saate seda kasutada ümbris otse viidata.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Viide on bloobimälu anonüümselt

Kui teil on bloobimälu, mis on saadaval anonüümse juurdepääsu URL-i, viitamiseks bloobimälu, kasutades seda URL-i.

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Anonüümne kasutajatele saadaval olevad funktsioonid.

Järgmises tabelis on näidatud, milliseid toiminguid nimetatakse anonüümsed kasutajad on container ACL määramisel avaliku juurdepääsu lubamiseks.

| ÜLEJÄÄNUD toiming                                         | Täielikku avaliku lugemine juurdepääsu õigus | Lugege teemat avaliku juurdepääsuga plekid ainult õigus |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Ümbriste loend                                        | Ainult omanik                              | Ainult omanik                                        |
| Container loomine                                       | Ainult omanik                              | Ainult omanik                                        |
| Container atribuudid hankimine                               | Kõik                                     | Ainult omanik                                        |
| Container metaandmete hankimine                                 | Kõik                                     | Ainult omanik                                        |
| Container Sea metaandmete                                 | Ainult omanik                              | Ainult omanik                                        |
| Container ACL hankimine                                      | Ainult omanik                              | Ainult omanik                                        |
| Container ACL seadmine                                      | Ainult omanik                              | Ainult omanik                                        |
| Container kustutamine                                       | Ainult omanik                              | Ainult omanik                                        |
| Loendi plekid                                             | Kõik                                     | Ainult omanik                                        |
| Sellele bloobimälu                                               | Ainult omanik                              | Ainult omanik                                        |
| Saada bloobimälu                                               | Kõik                                     | Kõik                                               |
| Saada bloobimälu atribuudid                                    | Kõik                                     | Kõik                                               |
| Atribuutide seadmine bloobimälu                                    | Ainult omanik                              | Ainult omanik                                        |
| Saada bloobimälu metaandmed                                      | Kõik                                     | Kõik                                               |
| Metaandmete määramine bloobimälu                                      | Ainult omanik                              | Ainult omanik                                        |
| Sellele blokeerimine                                              | Ainult omanik                              | Ainult omanik                                        |
| Saada blokeeritute loendis (kinnitatud plokid ainult)                 | Kõik                                     | Kõik                                               |
| Saada blokeeritute loendis (ainult sisseviimata plokid või kõik plokid) | Ainult omanik                              | Ainult omanik                                        |
| Sellele plokkloend                                         | Ainult omanik                              | Ainult omanik                                        |
| Kustutage bloobimälu                                            | Ainult omanik                              | Ainult omanik                                        |
| Kopeerige bloobimälu                                              | Ainult omanik                              | Ainult omanik                                        |
| Hetktõmmis bloobimälu                                          | Ainult omanik                              | Ainult omanik                                        |
| Üürilepingu bloobimälu                                             | Ainult omanik                              | Ainult omanik                                        |
| Sellele lehele                                               | Ainult omanik                              | Ainult omanik                                        |
| Hangi vahemikud                                        | Kõik                                     | Kõik                                                  |
| Lisa bloobimälu                                            | Ainult omanik                              | Ainult omanik                                                  |


## <a name="see-also"></a>Vt ka

- [Azure'i salvestusruumi teenuste autentimine](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Ühiskasutusega juurdepääsu allkirjad (SAS) abil](storage-dotnet-shared-access-signature-part-1.md)
- [Delegeeriv juurdepääsu ühiskasutusega Accessi allkirjastatud](https://msdn.microsoft.com/library/azure/ee395415.aspx)
