<properties
    pageTitle="Azure'i bloobimälu iOS kasutamine | Microsoft Azure'i"
    description="Azure'i bloobimälu (objekti salvestusruumi) koos pilveteenuses talletada struktureerimata andmed."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Kuidas kasutada bloobimälu iOS-i kaudu

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Ülevaade

Selles artiklis näitab teile, kuidas teha levinud stsenaariumi, kasutades Microsoft Azure'i bloobimälu. Näidised on kirjutatud eesmärk-C ja kasutada [Azure salvestusruumi kliendi teek iOS-i](https://github.com/Azure/azure-storage-ios). Stsenaariumid, mis hõlmab kaasata **üles laadida**, **loetelu**, **allalaadimise**ja plekid **kustutamine** . Plekid kohta leiate lisateavet jaotisest [Järgmised toimingud](#next-steps) . Samuti saate alla laadida [valimi rakenduse](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) kiiresti näha Azure Storage kasutamine iOS-i rakendus.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Importimine rakenduse Azure Storage iOS-i teek

Azure Storage iOS-i teeki saate importida rakenduse [Azure'i salvestusruumi CocoaPod](https://cocoapods.org/pods/AZSClient) abil või **Framework** faili importimisel.

## <a name="cocoapod"></a>CocoaPod

1. Kui te pole seda veel teinud, [Installige CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) teie arvutis, avades terminal aknas ja käivitage järgmine käsk

        sudo gem install cocoapods

2. Järgmise projekti kataloogis (kataloogi, mis sisaldab teie `.xcodeproj` fail), luua uus fail nimega `Podfile`(faili laiend). Lisatakse järgmised `Podfile` ja salvestamine

        pod 'AZSClient'

3. Klõpsake aknas terminal liigute projekti ja käivitage järgmine käsk

        pod install

4. Kui teie `.xcodeproj` on Xcode avatud, sulgege see. Avage oma projekti directory vastloodud projekti faili, mis on selle `.xcworkspace` laiend. See on fail kuvatakse töötate kaudu jaoks kohe.

## <a name="framework"></a>Raames
Azure Storage iOS-i teegi kasutamiseks peate esmalt koostada Frameworki faili.

1. Esmalt Laadige alla või klooni [azure-salvestusruumi-iOS-i repo](https://github.com/azure/azure-storage-ios).

2. Minge *azure-salvestusruumi-iOS-i* -> *teegi* -> *Azure salvestusruumi kliendi teek*ja avage `AZSClient.xcodeproj` Xcode sisse.

3. Muutke vasakus ülanurgas, Xcode, aktiivne kava "Azure salvestusruumi kliendi teek" "Raamistik".

4. Koostada projekt (⌘ + B). See loob mõne `AZSClient.framework` fail oma töölauale.

Saate rakenduse Frameworki faili importida seejärel, tehes järgmist:

1. Uue projekti loomise või olemasoleva projekti Xcode avada.

2. Klõpsake vasakpoolsel navigeerimispaanil projekti ja projekti redaktori ülaosas vahekaarti *üldist* .

3. Klõpsake jaotises *lingitud raamistiku ja teekide* nuppu Lisa (+).

4. Klõpsake *Lisa muu …*. Liikuge ja lisage soovitud `AZSClient.framework` äsja loodud fail.

5. Jaotises *lingitud raamistiku ja teekide* , klõpsake nuppu Lisa (+).

6. Teekide juba loendis Otsi `libxml2.2.dylib` ja lisamine projekti.

7. Klõpsake vahekaarti *Sätted koostada* projekti redaktori ülaosas.

8. Jaotises *Otsing teed* topeltklõpsake *Framework otsing teed* ja lisage tee oma `AZSClient.framework` faili.

## <a name="import-statement"></a>Lause importimine
Peate lisada järgmised impordi teatise faili, kuhu soovite autonoomsest Azure Storage API.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asünkroonsete toimingute
> [AZURE.NOTE] Kõik meetodid, mille vastu teenuse taotluse täitmine on asünkroonsete toimingute. Koodinäiteid, leiate, et need meetodid on lõpetamise sündmuseohjuri. Sees lõpetamise sündmuseohjuri käivituvad **pärast** kutse on lõpule viidud. Pärast lõpetamist sündmuseohjuri käivitavad **ajal** taotluse kood tehakse.

## <a name="create-a-container"></a>Ümbris loomine
Iga bloobimälu Azure Storage peab asuma ümbris. Järgmises näites kujutatakse loomine container, *newcontainer*, ehk teie salvestusruumi konto, kui see pole juba olemas. Nimi oma container valimisel olema teadlik ülalnimetatud nime reegleid.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Saate kinnitada, et see toimib, vaadates [Microsoft Azure'i salvestusruumi Explorer](http://storageexplorer.com) ja selle *newcontainer* on loendis ümbriste salvestusruumi konto jaoks.

## <a name="set-container-permissions"></a>Õiguste seadmine Container
Mõne container õigused on vaikimisi konfigureeritud **Privaatne** juurdepääsu. Siiski ümbriste pakkuda mõni erinevaid võimalusi container juurdepääsu:

- **Privaatne**: Container ja bloobimälu andmeid saab lugeda ainult konto omanik.

- **Bloobimälu**: see ümbris bloobimälu andmete saab lugeda anonüümse taotluse kaudu, kuid ümbrise andmed pole saadaval. Kliendid ei saa nummerdada plekid sees container anonüümse taotluse kaudu.

- **Container**: Container ja bloobimälu andmeid saab lugeda anonüümse taotluse kaudu. Kliendid saate loetleda plekid container kaudu anonüümse taotluse sees, kuid ei saa nummerdada ümbriste salvestusruumi kontol.

Järgmises näites kirjeldatakse, kuidas luua ümbris **Container** juurdepääsuõigused, mis lubab juurdepääsu avalik, ainult lugemiseks Internetis kõikide kasutajate jaoks:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Ümbris laadida soovitud bloobimälu
Jaotises [bloobimälu teenuste kontseptsioonide](#blob-service-concepts) nimetatud bloobimälu pakub kolme eri tüüpi plekid: blokeerida plekid, lisa plekid ja lehel plekid. Sel hetkel Azure Storage iOS-i teegi toetab ainult Blokeeri plekid. Enamikul juhtudel, Blokeeri bloobimälu on soovitatav kasutada tüüp.

Järgmises näites kujutatakse Blokeeri bloobimälu mõne NSString kaudu üles laadida. Kui see ümbrises on sama nimega bloobimälu on juba olemas, kirjutatakse selle bloobimälu sisu.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Saate kinnitada, et see toimib, vaadates [Microsoft Azure'i salvestusruumi Explorer](http://storageexplorer.com) ja kontrollida, et ümbris, *containerpublic*, sisaldab bloobimälu, *sampleblob*. Selles näites kasutatakse avaliku container nii, et saate kontrollida, et see töötas minnes plekid URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Lisaks üles Blokeeri bloobimälu mõne NSString kaudu, sarnaselt meetodite olemas NSData, NSInputStream või kohalik fail.

## <a name="list-the-blobs-in-a-container"></a>Loendi a ümbrises plekid
Järgmises näites kujutatakse loendis kõik plekid on ümbrises. Kui seda toimingut tuleb arvesse võtma järgmisi:     

- **continuationToken** - tähistab jätkamiseks luba, kus kirjet toiming peab algama. Kui ei luba, loetletakse selle plekid algusest. Suvalist arvu plekid saate loetletud nullist kuni maksimaalne kogum. Isegi juhul, kui see meetod tagastab null tulemusi, kui `results.continuationToken` pole null, võib olla mitu plekid teenuse, mis pole loendis.
- **eesliite** – saate määrata selle prefiks bloobimälu kirjet. Ainult plekid, mis algavad see eesliide on loetletud.
- **useFlatBlobListing** - nimetatud jaotises [nimetamise viitavat ümbriste ja plekid](#naming-and-referencing-containers-and-blobs) kuigi bloobimälu teenus on tasapinnalise salvestusruumi värviskeemi, saate luua virtuaalse hierarhia nime panemise plekid tee teabega. Siiski-kindla kirjet pole praegu toetatud; See on peagi saadaval. Nüüd, tuleb see väärtus`YES`
- **blobListingDetails** – saate määrata loetelu plekid kaasatavad üksused
    - `AZSBlobListingDetailsNone`: Loetletakse vaid kinnitatud plekid, ja tagastada bloobimälu metaandmete.
    - `AZSBlobListingDetailsSnapshots`: Loend kinnitatud plekid ja bloobimälu pilte.
    - `AZSBlobListingDetailsMetadata`: Laadi alla bloobimälu metaandmete iga bloobimälu tagasi kirjet.
    - `AZSBlobListingDetailsUncommittedBlobs`: Loendi kinnitatud ja sisseviimata plekid.
    - `AZSBlobListingDetailsCopy`: Lisage Kopeeri atribuudid kirjet.
    - `AZSBlobListingDetailsAll`: Saadaval kohustuse plekid, sisseviimata plekid ja hetktõmmiste loendi ja tagastada kõik metaandmete ja kopeerige need plekid olek.
- **maxResults** - maksimumarv tulemite tagastamiseks selle toimingu jaoks. Kasutage -1 on piiratud pole määratud.
- **completionHandler** - koodi käivitada kirjet selle toimingu tulemustega plokk.

Selles näites on kasutatud helper meetodi rekursiivselt kõne loendi plekid meetod iga kord, kui tagastatakse jätkamiseks luba.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Laadige alla mõne bloobimälu

Järgmises näites kujutatakse NSString objekti lisamine bloobimälu alla laadida.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Kustutamine on bloobimälu

Järgmises näites kirjeldatakse, kuidas kustutada mõne bloobimälu.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Bloobimälu container kustutamine

Järgmises näites kirjeldatakse, kuidas kustutada ümbris.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud, kuidas kasutada bloobimälu iOS-i kaudu, järgige neid linke iOS-i teek ja salvestusruumi teenuste kohta lisateavet.

- [Azure'i salvestusruumi kliendi teek iOS-i](https://github.com/azure/azure-storage-ios)
- [Azure'i salvestusruumi iOS-i dokumentides](http://azure.github.io/azure-storage-ios/)
- [Azure'i salvestusruumi teenuste REST API-ga](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage)

Kui teil on küsimusi selle teegi julgelt postitada meie [MSDN-i Azure foorumist](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) või [Virnlintdiagrammil ületäitumise](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Kui teil on funktsioon soovitatud Azure Storage, saatke [Azure'i salvestusruumi tagasiside](https://feedback.azure.com/forums/217298-storage/).
