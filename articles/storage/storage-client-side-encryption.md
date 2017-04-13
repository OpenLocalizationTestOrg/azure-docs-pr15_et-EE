<properties
    pageTitle="Kliendipoolne krüptimise .net-i abil Microsoft Azure Storage | Microsoft Azure'i"
    description="Azure'i salvestusruumi kliendi teek .NET toetab kliendipoolne krüptimise ja integreerimine Azure klahvi Vault maksimaalne turvalisus Azure Storage rakendusi."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Kliendipoolne krüptimise ja Azure võtme Vault Microsoft Azure Storage

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Ülevaade

[Azure'i salvestusruumi kliendi teek .NET Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.Storage) toetab krüptimist klientrakendustes jooksul enne Azure Storage üleslaadimine ja dekrüptimine andmete kliendile allalaadimise ajal. Teegi toetab ka integreerimine [Azure klahvi Vault](https://azure.microsoft.com/services/key-vault/) mäluhaldus konto võti.

Üksikasjaliku juhendi, mis viib teid läbi protsessi krüptimise plekid kliendipoolne krüptimise ja Azure klahvi Vault abil, vt [Microsoft Azure Storage abil Azure'i klahvi Vault krüptimine ja dekrüptimine plekid](storage-encrypt-decrypt-blobs-key-vault.md).

Kliendipoolne krüptimine Java, vt [Kliendipoolne krüptimise Java Microsoft Azure Storage](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Krüptimine ja dekrüptimine ümbriku tehnika kaudu

Krüptimine ja dekrüptimine järgige ümbriku meetod.

### <a name="encryption-via-the-envelope-technique"></a>Krüptimise teel ümbriku meetod

Krüptimise teel ümbriku meetod toimib järgmiselt:

1. Azure'i salvestusruumi kliendi teek loob sisu krüptovõtme (CEK), mis on üks kasutamiseks sümmeetriline võti.
2. Kasutaja andmed on krüptitud selle CEK.
3. Funktsiooni CEK on seejärel pakitud (krüptitud) klahv krüptovõtme (KEK). Selle KEK on tähistatud võtme identifikaator ja saate asümmeetriline võtme jutumärkide paar ilma või sümmeetriline võti ja saate olla hallatavate kohalikult või talletatud Azure'i klahvi võlvid.

    Salvestusruumi kliendi teek on kunagi KEK juurdepääs. Teegi käivitab võtme mähkimine algoritmi, mis on esitatud klahvi Vault. Kasutajad saavad valida kohandatud pakkujate kasutamiseks võtme mähkimine/unwrapping soovi.

4. Krüptitud andmed on seejärel laadida Azure'i salvestusteenus. Mõned täiendavad krüptimise metaandmete koos murtud võti on salvestatud metaandmete (klõpsake mõnda bloobimälu) või interpoleeritud krüptitud andmetega (järjekorda sõnumeid ja tabeli üksuste).

### <a name="decryption-via-the-envelope-technique"></a>Ümbriku tehnika kaudu dekrüptimine

Dekrüptimine kaudu ümbriku meetod toimib järgmiselt:

1. Kliendi teek eeldab, et kasutaja on klahv (KEK) krüptovõtme haldamine, kas kohapeal või Azure klahvi võlvid. Kasutaja ei pruugi teatud klahvi, mida kasutati krüptimiseks leida. Selle asemel võtme lahendaja, mis laheneb võtme ID-de klahvid saate häälestada ja kasutada.
2. Kliendi teek allalaadimist krüptitud andmed koos krüptimise materjali, mis on talletatud teenus.
3. Murtud sisu krüptovõtme (CEK) on siis pakendamata (lahtikrüptitud) abil võtme krüptimise võti (KEK). Siin uuesti kliendi teek pole juurdepääsu KEK. See viitab lihtsalt kohandatud või klahvi Vault pakkuja unwrapping algoritmi.
4. Sisu krüptovõtme (CEK) kasutatakse krüptitud kasutaja andmeid dekrüptida.

## <a name="encryption-mechanism"></a>Krüptimise süsteem

Salvestusruumi kliendi teek kasutab [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) krüptida kasutaja andmeid. Täpsemalt [Salakiri ploki Aheldamise (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) režiimi AES. Iga teenuse töötab mõnevõrra erineda, et me käsitleme iga neid siin.

### <a name="blobs"></a>Plekid

Kliendi teek toetab praegu ainult kogu plekid krüptimine. Täpsemalt krüptimise on toetatud, kui kasutajad kasutavad **UploadFrom** * meetodite või * *OpenWrite** meetod. Allalaaditavad failid, mõlemad lõpule viidud ja vahemiku allalaaditavad failid on toetatud.

Ajal krüptimist, kliendi teek on juhusliku lähtestamine vektorkuju IV 16 baiti, koos juhusliku sisu krüptimise võtme (CEK) 32 baiti, luua ja teha ümbriku krüptimist bloobimälu andmeid kasutada seda teavet. Murtud CEK ja mõned täiendavad krüptimise metaandmete siis talletatakse Bloobivahemälu metaandmete koos krüptitud bloobimälu teenuse.

> [AZURE.WARNING] Kui olete redigeerimise või üles soovitud bloobimälu oma metaandmed, peate tagamaks, et see metaandmete säilib. Kui laadite uue metaandmete ilma metaandmeid, murtud CEK, IV ja muude metaandmete kaotsi ja bloobimälu sisu kunagi olla tagastatava uuesti.

Allalaadimine on krüptitud bloobimälu hõlmab kogu bloobimälu, kasutades **LaeKui soovite**sisu toomine*/**BlobReadStream** mugavuse meetoditest. Murtud CEK pakendamata ja kasutada koos IV, (sel juhul bloobimälu metaandmete salvestatud), et naasta kasutajad kasutajaliidese kaudu.

Allalaadimine on suvaline valik (**DownloadRange*** meetodite) krüptitud bloobimälu hõlmab reguleerimine kasutajate esitatud Selleks, et saada väike täiendavad andmed, mida saab kasutada edukalt dekrüptida nõutud vahemik vahemik.

Kõik Bloobivahemälu tüüpi (plekid blokeerida, lehel plekid ja lisa plekid) saab krüptitud ja dekrüptida kava abil.

### <a name="queues"></a>Järjekorrad

Kuna järjekorda sõnumite võib olla mis tahes formaadis, määratleb kliendi teek kohandatud vorming, mis sisaldab lähtestamine vektorkuju (IV) ja krüptitud sisu krüptovõtme (CEK) sõnumi tekst.

Ajal krüptimist, kliendi teek genereeritud juhusliku IV 16 baiti koos juhusliku CEK 32 baiti ja sooritab ümbriku krüptimise abil seda teavet järjekorda sõnumi teksti. Seejärel lisatakse murtud CEK ja mõned täiendavad krüptimise metaandmete järjekorda krüptitud sõnumit. Teenuse salvestatakse see muudetud sõnum (vt allpool).

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Dekrüptimine, murtud võtit ekstraktimist järjekorda sõnumi ja pakendamata. Funktsiooni IV on ka ekstraktimist järjekorda sõnumi ja kasutada koos pakendamata võti järjekorda sõnumi andmeid dekrüptida. Pöörake tähelepanu sellele krüptimise metaandmeid (jaotises 500 baiti), väike, kuigi see arvestata 64KB limiit järjekorda sõnumi, mõju tuleks mõistliku.

### <a name="tables"></a>Tabelid

Kliendi teek toetab krüptimist üksuse atribuutide lisamine ja asendage toimingud.

>[AZURE.NOTE] Ühenda pole praegu toetatud. Kuna atribuudid alamhulga võib on krüptitud varem tähtsamad, tulemuseks lihtsalt uue atribuutide ühendamine ja metaandmete värskendamine andmete kaotsimineku. Kas ühendamise jaoks on vaja teenuse täiendav helistamist lugeda olemasoleva juriidilise teenuse või kasutades uue tootenumbri atribuudi kohta, mis ei sobi jõudluse.

Tabeli andmete krüptimine toimib järgmiselt:  

1. Kasutajatele määrata krüptimist atribuudid.
2. Kliendi teek genereeritud juhusliku lähtestamine vektorkuju (IV) 16 baiti koos juhusliku sisu krüptimise võti (CEK) 32 baiti iga üksuse jaoks ja sooritamine ümbriku krüptimise omaduste krüptimist, tulenevad uus IV atribuudi kohta. Krüptitud atribuudi talletatakse binaarandmeid.
3. Murtud CEK ja mõned täiendavad krüptimise metaandmete salvestatakse seejärel kaks täiendavad reserveeritud atribuudid. Esimene reserveeritud atribuut (_ClientEncryptionMetadata1) on string atribuut, mis sisaldab teavet IV versioon ja murtud võti. Teine reserveeritud atribuut (_ClientEncryptionMetadata2) on kahendarvu atribuut, mis sisaldab teavet atribuudid, mis on krüptitud. Teave selle teise atribuudi (_ClientEncryptionMetadata2) on krüptitud.
4. Tõttu need täiendavad reserveeritud atribuudid nõutavaid krüptimist, võivad kasutajad on nüüd 252 asemel ainult 250 kohandatud atribuudid. Suuruse üksuse peab olema väiksem kui 1 MB.

Pange tähele, et ainult stringi atribuudid saate krüptitud. Kui muud tüüpi atribuudid on krüptimist, peab olema teisendatakse stringide. Krüptitud stringid salvestatakse teenuse kahendarvu atribuudid ja need teisendatakse tagasi stringide pärast dekrüptimine.

Tabelid, lisaks krüptimise poliitika, peavad kasutajad Määrake krüptimist atribuudid. Seda saab teha määramisega kummagi [EncryptProperty] atribuut (POCO TableEntity saadud üksused) või mõne krüptimise lahendaja taotluse suvandid. Mõne krüptimise lahendaja on volitatud esindaja, mis võtab partition klahvi, rida võti ja atribuudi nimi ja tagastab kahendväärtus, mis näitab, kas selle atribuudi peaksid olema krüptitud. Ajal krüptimist, kliendi teek kasutab seda teavet otsustada, kas atribuut krüptida, et kaabel kirjutamise ajal. Volitatud esindaja näeb võimalust loogika ümber kuidas atribuudid on krüptitud. (Näiteks, kui X, seejärel krüptimiseks atribuudi A; muidu krüptida atribuudid A ja b) Pange tähele, et ei ole vaja selle teabe lugedes või päringu üksuste.

### <a name="batch-operations"></a>Toimingud

Paketi toimingute puhul sama KEK puhul kasutatakse protseduuris paketi kõik read kuna kliendi teek võimaldab ainult ühe Suvandid objekti (ja seega ühes poliitika/KEK) paketi toimingus. Siiski kliendi teek ettevõttesiseselt loob uue juhusliku IV ja rea euro kohta juhusliku CEK paketi. Kasutajate saate valida ka määratleda seda käitumist krüptimise lahendaja eri atribuutide iga toimingu paketi krüptimine.

### <a name="queries"></a>Päringud

Päringu toimingute tegemiseks peate määrama võtme lahendaja, mis on lahendada kõik võtmed tulemite hulk. Kui ettevõte, mis sisalduvad päringu tulem ei saa lahendada pakkujale, kuvatakse kliendi teek põhjustada tõrke. Päring, mis sooritavad serveripoolne prognooside, lisab kliendi teek (_ClientEncryptionMetadata1 ja _ClientEncryptionMetadata2) teisiti krüptimise metaandmete atribuutide vaikimisi valitud veerud.

## <a name="azure-key-vault"></a>Azure'i võtme Vault

Azure'i klahvi Vault aitab kaitse cryptographic võtmed ja saladusi kasutavad cloud rakenduste ja teenuste. Azure'i klahvi Vault abil saate kasutajad krüptida võtmed ja saladused (nt autentimise klahvid, salvestusruumi konto klahvid, andmete krüptimise võtmed. PFX-failid ja paroolid) kasutades klahve, mis on kaitstud riistvara turvalisus moodulid (HSMs). Lisateavet leiate teemast [mis on Azure klahvi Vault?](../key-vault/key-vault-whatis.md).

Salvestusruumi kliendi teek kasutab klahvi Vault core teegi nimestikku levinud üle Azure'i haldamise võtmed. Kasutajad saavad ka täiendavad klahvi Vault laiendid teegi kasutamise eelised. Laiendid teegi pakub kasulikke funktsioone lihtne ja sujuvalt sümmeetriline/RSA kohaliku ja pilveteenuse võtme pakkujate ümber ning liitmise ja vahemällu.

### <a name="interface-and-dependencies"></a>Kasutajaliidese ja sõltuvused

On kolme klahvi Vault paketid.

- Microsoft.Azure.KeyVault.Core sisaldab IKey ja IKeyResolver. See on väike pakett pole sõltuvusi. .Net-i salvestusruumi kliendi teek määratleb selle sõltuvus.
- Microsoft.Azure.KeyVault sisaldab klahvi Vault ülejäänud kliendi.
- Microsoft.Azure.KeyVault.Extensions sisaldab laiend kood, mis sisaldab rakendusi on RSAKey ja soovitud SymmetricKey ja cryptographic algoritmide kohta. See sõltub põhi- ja KeyVault nimeruumid ja funktsioonid on liitväärtuse lahendaja (kui kasutajad soovivad kasutage mitme klahv) ja vahemällu võtme lahendaja määratlemiseks. Kuigi salvestusruumi kliendi teek pole sõltuvad otseselt selle paketi, kui kasutajad soovivad Azure'i klahvi Vault salvestada oma võtmed või kasutada klahvi Vault laiendid tarbimine kohaliku ja cloud cryptographic pakkujad, tuleb see pakett.

Klahv Vault on mõeldud suure väärtusega juhtslaidi võtmed ja ahendamise limiitide kohta klahvi Vault on koostatud selle meeles. Kliendipoolne krüptimise abil klahvi Vault täites eelistatud mudel on kasutada sümmeetriliste juhtslaidi võtmed salvestatud saladusi klahvi võlvkelder ja vahemälus talletatud kohalik. Kasutajad, peate tegema järgmist:

1. Salajane ühenduseta luua ja üles see võti Vault.
2. Kasutage on salajane base identifikaator parameetrina lahendamiseks praeguse versiooni salajane krüptimiseks ja selle teabe kohalik vahemälu. Kasutage CachingKeyResolver vahemällu; Kasutajad ei ole oodata rakendada oma loogika vahemällu.
3. Kasutage vahemällu lahendaja sisendina krüptimise poliitika loomisel.

Klahv Vault kasutamise kohta saate lisateavet leiate [krüptimise koodinäiteid](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>Head tavad

Krüptimise tugi on saadaval ainult salvestusruumi kliendi teek .net-i jaoks. Windows Phone ja Windows käitusaja ei toeta krüptimist.

>[AZURE.IMPORTANT] Arvestage järgmisi olulisi punkte kliendipoolne krüptimise kasutamisel:
>
>- Kogu bloobimälu Laadi üles käske ja vahemik ja kogu bloobimälu allalaadimine käskude kasutamine lugemine või kirjutamine on krüptitud bloobimälu korral. Vältige kirjalikult on krüptitud bloobimälu abil protokolli toiminguid nagu sellele blokeerimine, sellele blokeeritute loendis, kirjutage lehtede, Eemalda lehtede või lisa plokk; muul juhul võite rikutud krüptitud bloobimälu ja veenduge, et see loetavad.
>- Tabelite puhul sarnased piirangu olemas. Jälgige, et te ei värskenda krüptitud atribuudid ilma krüptimise metaandmete värskendamine.
>- Kui seate metaandmete krüptitud bloobimälu, võib vaja dekrüptimine, kuna metaandmete säte ei ole lisaaine krüptimise seotud metaandmed üle kirjutada. See kehtib ka pilte; Vältige täpsustades metaandmete on krüptitud bloobimälu hetktõmmise loomisel. Kui metaandmete peab olema seatud, veenduge, et **FetchAttributes** meetod esmalt, et saada praegune krüptimise metaandmete ja vältida samaaegseid kirjutab ajal metaandmete seatakse.
>- Luba **RequireEncryption** vara taotluse vaikesuvandite kasutajad, mis peaks töötama krüptitud andmetega. Lisateabe saamiseks vt allpool.


## <a name="client-api--interface"></a>Kliendi API / Interface

Objekti EncryptionPolicy loomisel kasutajad saavad sisestada ainult klahvi (rakendamise IKey), ainult lahendaja (rakendamise IKeyResolver) või mõlemad. IKey on lihtne klahv tüüp, mis on tuvastatud võtme identifikaator abil ja mis antakse loogika mähkimine ja unwrapping. IKeyResolver kasutatakse lahendamiseks klahvi dekrüptimine käigus. See määratleb ResolveKey meetod, mis on antud võtme identifikaator IKey tagastab. See võimaldab kasutajatel võimalus valida mitu tutvustatakse, mida hallatakse mitmes asukohas.

- Krüptimine, kasutatakse alati ja klahvi puudumisel tulemuseks viga.
- Jaoks dekrüptimine:
    - Võtme lahendaja on kasutada, kui määratud saada võti. Kui soovitud lahendaja on määratud, kuid ei saa võtme identifikaator vastendus, tõrge Expression.error.
    - Kui lahendaja pole määratud, kuid klahvi on määratud, kasutatakse võti kui selle identifikaator vastab nõutud võtme identifikaator. Kui kood ei ühti, tõrge Expression.error.

[Krüptimise näidet](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) kujutavad üksikasjalikumat lõpuni stsenaarium plekid, järjekorrad ja tabelid, koos võti Vault integreerimine.

### <a name="requireencryption-mode"></a>RequireEncryption režiim

Soovi korral saavad kasutajad lubada kui üles- ja allalaadimised peab olema krüptitud turvarežiimi. Selles režiimis katsete andmete krüptimise poliitika ilma üles-või allalaadimine andmeid, mis on krüptitud teenus ei õnnestu klientarvutis. Taotluse Suvandid objekti atribuudi **RequireEncryption** juhtelemendid seda käitumist. Kui rakenduse krüptida kõiki objekte, mis on talletatud Azure Storage, siis saate seada atribuudi **RequireEncryption** taotluse vaikesuvandite kliendi objekti. Näiteks seada **CloudBlobClient.DefaultRequestOptions.RequireEncryption** väärtuseks **true** nõuda krüptimise kõigi bloobimälu toimingutest kliendi objekti kaudu.

### <a name="blob-service-encryption"></a>Bloobimälu teenuse krüptimine

**BlobEncryptionPolicy** objekti loomine ja määrake selle taotluse suvandite (API või kliendi tasemel, kasutades **DefaultRequestOptions**). Veel tegeleb kliendi teek ettevõttesiseselt.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Upload the encrypted contents to the blob.
    blob.UploadFromStream(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    MemoryStream outputStream = new MemoryStream();
    blob.DownloadToStream(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Järjekorda teenuse krüptimine

**QueueEncryptionPolicy** objekti loomine ja määrake selle taotluse suvandite (API või kliendi tasemel, kasutades **DefaultRequestOptions**). Veel tegeleb kliendi teek ettevõttesiseselt.


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
    queue.AddMessage(message, null, null, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);

### <a name="table-service-encryption"></a>Tabeli teenuse krüptimine

Lisaks krüptimise poliitika loomise ja millega see taotlus suvandid, peate määrata ka **EncryptionResolver** **TableRequestOptions**või määrata [EncryptProperty] atribuut üksus.

#### <a name="using-the-resolver"></a>Funktsiooni lahendaja abil


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    {
        EncryptionResolver = (pk, rk, propName) =>
        {
            if (propName == "foo")
            {
                return true;
            }
            return false;
        },
        EncryptionPolicy = policy
    };

    // Insert Entity
    currentTable.Execute(TableOperation.Insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    {
        EncryptionPolicy = policy
    };

    TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
    TableResult result = currentTable.Execute(operation, retrieveOptions, null);

#### <a name="using-attributes"></a>Atribuutide abil

Nagu eespool, kui üksus rakendab TableEntity, seejärel atribuudid saate kujundatud **EncryptionResolver**määratlemise asemel atribuudiga [EncryptProperty].

    [EncryptProperty]
    public string EncryptedProperty1 { get; set; }

## <a name="encryption-and-performance"></a>Krüptimise ja jõudlus

Pange tähele, et teie salvestusruumi tulemuste täiendavad jõudluse pea kohal krüptimine. Sisu võtme ja IV peab loodud, peab olema krüptitud sisu ise ja täiendavad metaandmete peab vormindatud ja üles laadida. Andmed on krüptitud kogus sõltub selle pea kohal. Soovitame, et kliendid saavad rakenduste arendamise käigus jõudluse alati testida.

## <a name="next-steps"></a>Järgmised sammud

- [Õpetus: Krüptimiseks ja dekrüptida plekid Microsoft Azure Storage Azure'i klahvi Vault abil](storage-encrypt-decrypt-blobs-key-vault.md)
- [Azure'i salvestusruumi kliendi teek .NET Nugeti paketi](https://www.nuget.org/packages/WindowsAzure.Storage) allalaadimine
- Azure'i klahvi Vault Nugeti [Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [klientide](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/)ja [laiendid](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) pakette alla laadida  
- Külastage [Azure klahvi võlvkelder dokumentatsioon](../key-vault/key-vault-whatis.md)

