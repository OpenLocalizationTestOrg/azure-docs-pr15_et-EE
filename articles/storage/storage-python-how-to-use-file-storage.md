<properties
    pageTitle="Azure'i failide talletamine kaudu Python kasutamise kohta | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure faili salvestusruumi Python kaudu üles, loendi, allalaadimine ja kustutada."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Kuidas kasutada Azure failide talletamine Python kaudu

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Ülevaade

Selles artiklis näitab teile, kuidas teha levinud stsenaariumi, kasutades failisalvestusruumi. Näidiste Python kirjutada ja kasutada [Microsoft Azure'i salvestusruumi SDK Python]. Stsenaariumid, mis hõlmab kaasata üleslaadimise, loetelu, allalaadimise ja failide kustutamine.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Luua aktsia

Objekti **FileService** võimaldab teil ühtlasi, kataloogid ja failidega töötamiseks. Järgmine kood tekitab **FileService** objekti. Saate lisada mis tahes Python faili, kus soovite programmiliselt juurde Azure Storage ülaosas järgmist.

    from azure.storage.file import FileService

Järgmine kood loob **FileService** objekti abil salvestusruumi konto nimi ja konto võti.  Asendage "minu konto" ja 'mykey' oma konto nimi ja võti.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

Järgmises näites kood, saate **FileService** objekti loomiseks ühiskasutus, kui seda pole.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Faili üleslaadimine osaks

Azure'i salvestusruumi failikettal sisaldab vähemalt, root kataloogi, kus failid võivad asuda. Selles jaotises saate teada, kuidas üles laadida faili kohaliku salvestusruumi juurkaust aktsia peale.

Uue faili loomine ja andmete üles laadida, kasutage funktsiooni **loomine\_faili\_kaudu\_tee**, **loomine\_faili\_kaudu\_voo**, **loomine\_faili\_kaudu\_baiti** või **loomine\_faili\_kaudu\_teksti** meetodid. Need on üksikasjalik viise, mida teha, kui andmete maht ületab 64 MB vajalikud murenemist.

**luua\_faili\_kaudu\_tee** lisatud määratud tee faili sisu ja **loomine\_faili\_kaudu\_voo** lisatud sisu juba avatud faili/voo kaudu. **luua\_faili\_kaudu\_baiti** lisatud massiivi baiti, ja **loomine\_faili\_kaudu\_teksti** lisatud määratud teksti väärtuse määratud kodeeringut (vaikeväärtus on UTF-8).

Järgmises näites lisatud **myfile** faili **sunset.png** faili sisu.

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Kuidas: luua kataloog

Samuti saate salvestusruumi paigutades faile sub kataloogide asemel kõik need juurkaustas sees. Azure'i faili salvestusteenus võimaldab teil luua nii palju kataloogide vastavalt teie konto võimaldab. Alljärgnev kood loob **sampledir** jaotises juurkaustas sub kataloogi.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Kuidas: loendi failide ja kataloogide ühiskasutamine

Failide ja ühiskasutamine kataloogide loend, kasutage funktsiooni **loendi\_kataloogide\_ja\_failide** meetod. See meetod tagastab generaator. Järgmine kood väljundid iga faili ja kataloogi ühiskasutamine konsooli **nimi** .

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Failide allalaadimine

Andmete alla laadida faili, kasutage **saada\_faili\_et\_tee**, **saada\_faili\_abil\_voo**, **saada\_faili\_abil\_baiti**, või **saada\_faili\_abil\_teksti**. Need on üksikasjalik viise, mida teha, kui andmete maht ületab 64 MB vajalikud murenemist.

Järgmises näites näitab abil **saada\_faili\_et\_tee** **myfile** faili sisu alla laadida ja salvestada see fail **välja-sunset.png** .

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Faili kustutamine

Lõpuks faili kustutamiseks helistada **delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid failide talletamine, järgige neid linke lisateabe.

- [Python Arenduskeskus](/develop/python/)
- [Azure'i salvestusruumi teenuste REST API-ga](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure'i salvestusruumi meeskonna ajaveeb]
- [Microsoft Azure'i salvestusruumi SDK Python]

[Azure'i salvestusruumi meeskonna ajaveeb]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure'i salvestusruumi SDK Python]: https://github.com/Azure/azure-storage-python
