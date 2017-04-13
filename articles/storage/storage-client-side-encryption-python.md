<properties
    pageTitle="Kliendipoolne krüptimise Python Microsoft Azure Storage | Microsoft Azure'i"
    description="Azure'i salvestusruumi kliendi teek Python toetab kliendipoolne krüptimise maksimaalne turvalisus Azure Storage rakendusi."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Kliendipoolne krüptimise Python Microsoft Azure Storage

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Ülevaade

[Azure'i salvestusruumi kliendi teegi jaoks Python](https://pypi.python.org/pypi/azure-storage) toetab krüptimist klientrakendustes jooksul enne Azure Storage üleslaadimine ja dekrüptimine andmete kliendile allalaadimise ajal.

>[AZURE.NOTE] Azure'i salvestusruumi Python teek on eelvaates.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Krüptimine ja dekrüptimine ümbriku tehnika kaudu
Krüptimine ja dekrüptimine järgige ümbriku meetod.

### <a name="encryption-via-the-envelope-technique"></a>Krüptimise teel ümbriku meetod
Krüptimise teel ümbriku meetod toimib järgmiselt:

1.  Azure'i salvestusruumi kliendi teek loob sisu krüptovõtme (CEK), mis on üks kasutamiseks sümmeetriline võti.

2.  Kasutaja andmed on krüptitud selle CEK.

3.  Funktsiooni CEK on seejärel pakitud (krüptitud) klahv krüptovõtme (KEK). Funktsiooni KEK tuvastatakse võtme identifikaator ja võib olla mõni asümmeetriline paari või sümmeetriline võti, mis on kohalik õnnestus.
Salvestusruumi kliendi teek on kunagi KEK juurdepääs. Teegi käivitab võtme mähkimine algoritmi, mis on esitatud selle KEK. Kasutajad saavad valida kohandatud pakkujate kasutamiseks võtme mähkimine/unwrapping soovi.

4.  Krüptitud andmed on seejärel laadida Azure'i salvestusteenus. Mõned täiendavad krüptimise metaandmete koos murtud võti on salvestatud metaandmete (klõpsake mõnda bloobimälu) või interpoleeritud krüptitud andmetega (järjekorda sõnumeid ja tabeli üksuste).

### <a name="decryption-via-the-envelope-technique"></a>Ümbriku tehnika kaudu dekrüptimine
Dekrüptimine kaudu ümbriku meetod toimib järgmiselt:

1.  Kliendi teek eeldab, et kasutajal on klahv krüptovõtme (KEK) kohalikult haldamine. Kasutaja ei pruugi teatud klahvi, mida kasutati krüptimiseks leida. Selle asemel võtme lahendaja, mis annab tulemuseks vea võtme ID-de klahvid, saate häälestada ja kasutada.

2.  Kliendi teek allalaadimist krüptitud andmed koos krüptimise materjali, mis on talletatud teenus.

3.  Murtud sisu krüptovõtme (CEK) on siis pakendamata (lahtikrüptitud) abil võtme krüptimise võti (KEK). Siin uuesti kliendi teek pole juurdepääsu KEK. See viitab lihtsalt kohandatud pakkuja unwrapping algoritmi.

4.  Sisu krüptovõtme (CEK) kasutatakse krüptitud kasutaja andmeid dekrüptida.

## <a name="encryption-mechanism"></a>Krüptimise süsteem  
Salvestusruumi kliendi teek kasutab [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) krüptida kasutaja andmeid. Täpsemalt [Salakiri ploki Aheldamise (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) režiimi AES. Iga teenuse töötab mõnevõrra erineda, et me käsitleme iga neid siin.

### <a name="blobs"></a>Plekid
Kliendi teek toetab praegu ainult kogu plekid krüptimine. Täpsemalt krüptimise on toetatud, kui kasutajad kasutavad **loomine*** viise. Allalaaditavad failid, nii on lõpule viidud ja vahemiku allalaaditavad failid on toetatud ja parallelization nii üles- ja allalaadimine on saadaval.

Ajal krüptimist, kliendi teek on juhusliku lähtestamine vektorkuju IV 16 baiti, koos juhusliku sisu krüptimise võtme (CEK) 32 baiti, luua ja teha ümbriku krüptimist bloobimälu andmeid kasutada seda teavet. Murtud CEK ja mõned täiendavad krüptimise metaandmete siis talletatakse Bloobivahemälu metaandmete koos krüptitud bloobimälu teenuse.

>[AZURE.WARNING] Kui olete redigeerimise või üles soovitud bloobimälu oma metaandmed, peate tagamaks, et see metaandmete säilib. Kui laadite uue metaandmete ilma metaandmeid, murtud CEK, IV ja muude metaandmete läheb kaotsi ja bloobimälu sisu kunagi olla tagastatava uuesti.

Allalaadimine on krüptitud bloobimälu hõlmab kogu bloobimälu, kasutades **saada**sisu toomine * mugavuse meetoditest. Murtud CEK pakendamata ja kasutada koos IV, (sel juhul bloobimälu metaandmete salvestatud), et naasta kasutajad kasutajaliidese kaudu.

Allalaadimine on suvaline valik (**saada*** meetodite vahemiku parameetritega möödunud) krüptitud bloobimälu hõlmab reguleerimine kasutajate esitatud Selleks, et saada väike täiendavad andmed, mida saab kasutada edukalt dekrüptida nõutud vahemik vahemik.

Blokeeri plekid ja lehe plekid ainult saab krüptitud/dekrüptida kasutades selle kava. Praegu ei toetata jaoks krüptimise lisamiseks plekid.

### <a name="queues"></a>Järjekorrad
Kuna järjekorda sõnumite võib olla mis tahes formaadis, määratleb kliendi teek kohandatud vorming, mis sisaldab lähtestamine vektorkuju (IV) ja krüptitud sisu krüptovõtme (CEK) sõnumi tekst.

Ajal krüptimist, kliendi teek genereeritud juhusliku IV 16 baiti koos juhusliku CEK 32 baiti ja sooritab ümbriku krüptimise abil seda teavet järjekorda sõnumi teksti. Seejärel lisatakse murtud CEK ja mõned täiendavad krüptimise metaandmete järjekorda krüptitud sõnumit. Teenuse salvestatakse see muudetud sõnum (vt allpool).

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Dekrüptimine, murtud võtit ekstraktimist järjekorda sõnumi ja pakendamata. Funktsiooni IV on ka ekstraktimist järjekorda sõnumi ja kasutada koos pakendamata võti järjekorda sõnumi andmeid dekrüptida. Pöörake tähelepanu sellele krüptimise metaandmeid (jaotises 500 baiti), väike, kuigi see arvestata 64KB limiit järjekorda sõnumi, mõju tuleks mõistliku.

### <a name="tables"></a>Tabelid
Kliendi teek toetab krüptimist üksuse atribuutide lisamine ja asendage toimingud.

>[AZURE.NOTE] Ühenda pole praegu toetatud. Kuna atribuudid alamhulga võib on krüptitud varem tähtsamad, tulemuseks lihtsalt uue atribuutide ühendamine ja metaandmete värskendamine andmete kaotsimineku. Kas ühendamise jaoks on vaja teenuse täiendav helistamist lugeda olemasoleva üksuse teenuse või kasutades uue tootenumbri atribuudi kohta, mis ei sobi jõudluse.

Tabeli andmete krüptimine toimib järgmiselt:

1.  Kasutajatele määrata krüptimist atribuudid.

2.  Kliendi teek genereeritud juhusliku lähtestamine vektorkuju (IV) 16 baiti koos juhusliku sisu krüptimise võti (CEK) 32 baiti iga üksuse jaoks ja sooritamine ümbriku krüptimise omaduste krüptimist, tulenevad uus IV atribuudi kohta. Krüptitud atribuudi talletatakse binaarandmeid.

3.  Murtud CEK ja mõned täiendavad krüptimise metaandmete salvestatakse seejärel kaks täiendavad reserveeritud atribuudid. Esimene reserveeritud atribuudi (\_ClientEncryptionMetadata1) on string atribuut, mis sisaldab teavet IV versioon ja murtud võti. Teine reserveeritud atribuudi (\_ClientEncryptionMetadata2) on kahendarvu atribuut, mis sisaldab teavet atribuudid, mis on krüptitud. Teave selle teise atribuudi (\_ClientEncryptionMetadata2) on krüptitud.

4.  Tõttu need täiendavad reserveeritud atribuudid nõutavaid krüptimist, võivad kasutajad on nüüd 252 asemel ainult 250 kohandatud atribuudid. Suuruse üksuse peab olema väiksem kui 1MB.

    Pange tähele, et ainult stringi atribuudid saate krüptitud. Kui muud tüüpi atribuudid on krüptimist, peab olema teisendatakse stringide. Krüptitud stringid salvestatakse teenuse kahendarvu atribuudid ja need teisendatakse tagasi stringide (töötlemata stringid, mitte EntityProperties tüüpi EdmType.STRING) pärast dekrüptimine.

    Tabelid, lisaks krüptimise poliitika, peavad kasutajad Määrake krüptimist atribuudid. Seda saab teha, kas talletamine atribuutidest TableEntity objektide EdmType.STRING tüüp on seatud ja krüptimine väärtuseks true või säte on encryption_resolver_function tableservice objekti. Mõne krüptimise lahendaja on funktsioon, mis võtab partition klahvi, rida võti ja atribuudi nimi ja tagastab kahendväärtus, mis näitab, kas selle atribuudi peaksid olema krüptitud. Ajal krüptimist, kliendi teek kasutab seda teavet otsustada, kas atribuut krüptida, et kaabel kirjutamise ajal. Volitatud esindaja näeb võimalust loogika ümber kuidas atribuudid on krüptitud. (Näiteks, kui X, seejärel krüptimiseks atribuudi A; muidu krüptida atribuudid A ja b) Pange tähele, et ei ole vaja selle teabe lugedes või päringu üksuste.

### <a name="batch-operations"></a>Toimingud
Ühe krüptimise poliitika kehtib paketi kõik read. Kliendi teek luua uue juhusliku IV ja rea euro kohta juhusliku CEK ettevõttesiseselt pakett. Kasutajate saate valida ka määratleda seda käitumist krüptimise lahendaja eri atribuutide iga toimingu paketi krüptimine.
Kui partii on loodud haldurina konteksti kaudu tableservice batch() meetod, rakendatakse automaatselt selle tableservice krüptimise poliitika pakett. Kui partii on loodud konkreetselt helistades ehitaja, peate krüptimise poliitika edastatakse ja mille paketi eluiga vasakule pole muudetud.
Pange tähele, nagu nad on lisada paketi abil soovitud paketi krüptimise poliitika (üksused on krüptitud toime abil soovitud tableservice krüptimise poliitika paketi ajal), et üksused on krüptitud.

### <a name="queries"></a>Päringud
Päringu toimingute tegemiseks peate määrama võtme lahendaja, mis on lahendada kõik võtmed tulemite hulk. Kui ettevõte, mis sisalduvad päringu tulem ei saa laiendada pakkuja, kuvatakse kliendi teek põhjustada tõrke. Päring, mis sooritavad serveri pool prognooside, lisab kliendi teek teisiti krüptimise metaandmete atribuutide (\_ClientEncryptionMetadata1 ja \_ClientEncryptionMetadata2) vaikimisi valitud veerud.

>[AZURE.IMPORTANT] Arvestage järgmisi olulisi punkte kliendipoolne krüptimise kasutamisel:
>
>- Kogu bloobimälu Laadi üles käske ja vahemiku/kogu bloobimälu allalaadimine käskude kasutamine lugemine või kirjutamine on krüptitud bloobimälu korral. Vältige kirjalikult on krüptitud bloobimälu abil protokolli toiminguid nagu sellele Blokeeri, sellele blokeeritute loendis, kirjutage lehtede või Eemalda lehtede; muul juhul võite rikutud krüptitud bloobimälu ja veenduge, et see loetavad.
>
>- Tabelite puhul sarnased piirangu olemas. Jälgige, et te ei värskenda krüptitud atribuudid ilma krüptimise metaandmete värskendamine.
>
>- Kui seate metaandmete krüptitud bloobimälu, võib vaja dekrüptimine, kuna metaandmete säte ei ole lisaaine krüptimise seotud metaandmed üle kirjutada. See kehtib ka pilte; Vältige täpsustades metaandmete on krüptitud bloobimälu hetktõmmise loomisel. Kui metaandmete peab olema seatud, veenduge, et **get_blob_metadata** meetod esmalt, et saada praegune krüptimise metaandmete ja vältida samaaegseid kirjutab ajal metaandmete seatakse.
>
>- Luba **require_encryption** lipu teenuse objekti kasutajatele, kes peaksid töötavad ainult krüptitud andmed. Lisateabe saamiseks vt allpool.

Salvestusruumi kliendi teek eeldab, et esitatud KEK ja võtme lahendaja järgmised kasutajaliidese rakendada. [Azure'i klahvi Vault](https://azure.microsoft.com/services/key-vault/) Python KEK halduse tugi on ootel ja integreeritakse selle teegi lõppedes.


## <a name="client-api--interface"></a>Kliendi API / Interface
Pärast salvestusruumi teenuse objekti (nt blockblobservice) on loodud, kasutaja võib määrata välju, mida kujutavad endast krüptimise poliitika väärtused: key_encryption_key, key_resolver_function, ja require_encryption. Kasutajad saavad sisestada ainult KEK ainult lahendaja või mõlemad. key_encryption_key on lihtne võtme tüüp, mis on tuvastatud võtme identifikaator abil ja mis antakse loogika mähkimine/unwrapping. key_resolver_function kasutatakse lahendamiseks klahvi dekrüptimine käigus. Tagastab see lubatud KEK, mis on antud võtme identifikaator. See võimaldab kasutajatel võimalus valida mitu tutvustatakse, mida hallatakse mitmes asukohas.

Funktsiooni KEK rakendama krüptimiseks edukalt andmete järgmistest viisidest:
- wrap_key(cek): murtakse määratud CEK (baiti) algoritmi kasutaja valitud abil. Tagastab murtud võti.
- get_key_wrap_algorithm(): annab algoritmi kasutatavad kiirklahvid.
- get_kid(): tagastab stringi võtme id selle KEK.
Funktsiooni KEK rakendama edukalt dekrüptida andmete järgmistest viisidest:
- unwrap_key (cek, algoritmi): tagastab määratud CEK, stringi määratud algoritmi pakendamata kujul.
- get_kid(): tagastab stringi võtme id selle KEK.

Võtme lahendaja rakendama vähemalt meetod, mis on antud võtme ID-d, tagastab vastavate KEK, rakendamise ülaltoodud kasutajaliidese. Ainult see meetod on teenuse objekti atribuudi key_resolver_function määrata.

- Krüptimine, kasutatakse alati ja klahvi puudumisel tulemuseks viga.
- Jaoks dekrüptimine:
    - Võtme lahendaja on kasutada, kui määratud saada võti. Kui soovitud lahendaja on määratud, kuid ei saa võtme identifikaator vastendus, tõrge Expression.error.
    - Kui lahendaja pole määratud, kuid klahvi on määratud, kasutatakse võti kui selle identifikaator vastab nõutud võtme identifikaator. Kui kood ei ühti, tõrge Expression.error.

      Krüptimise proovide puhul azure.storage.samples <fix URL>kujutavad üksikasjalikumat lõpuni stsenaariumi plekid, järjekorrad ja tabeleid.
        Valimi rakendusi KEK ja võtme lahendaja on antud valimi failid KeyWrapper ja KeyResolver vastavalt.

### <a name="requireencryption-mode"></a>RequireEncryption režiim
Soovi korral saavad kasutajad lubada kui üles- ja allalaadimised peab olema krüptitud turvarežiimi. Selles režiimis katsete andmete krüptimise poliitika ilma üles-või allalaadimine andmeid, mis on krüptitud teenus ei õnnestu klientarvutis. **Require_encryption** lipu teenuse objekti juhtelemendid seda käitumist.

### <a name="blob-service-encryption"></a>Bloobimälu teenuse krüptimine
Määrake krüptimise poliitika väljad blockblobservice objekti. Veel tegeleb kliendi teek ettevõttesiseselt.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Järjekorda teenuse krüptimine
Määrake krüptimise poliitika väljad queueservice objekti. Veel tegeleb kliendi teek ettevõttesiseselt.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Tabeli teenuse krüptimine
Lisaks krüptimise poliitika loomise ja millega see taotlus suvandid, peate määrata ka **encryption_resolver_function** **tableservice**või määrata Krüpti atribuut on EntityProperty.

### <a name="using-the-resolver"></a>Funktsiooni lahendaja abil

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Atribuutide abil
Eespool nimetatud atribuut võib olla tähistatud krüptimiseks talletamine selle objekti EntityProperty ja seades krüptimine välja.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Krüptimise ja jõudlus
Pange tähele, et teie salvestusruumi tulemuste täiendavad jõudluse pea kohal krüptimine. Peab loodud sisu võtme ja IV, peab olema krüptitud sisu ise ja täiendavad metaandmed peab vormindatud ja üles laadida. Andmed on krüptitud kogus sõltub selle pea kohal. Soovitame, et kliendid saavad rakenduste arendamise käigus jõudluse alati testida.

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i salvestusruumi kliendi teek Java PyPi paketi](https://pypi.python.org/pypi/azure-storage) allalaadimine
- [Azure'i salvestusruumi kliendi teegi jaoks Python lähtekood kaudu GitHub](https://github.com/Azure/azure-storage-python) allalaadimine
