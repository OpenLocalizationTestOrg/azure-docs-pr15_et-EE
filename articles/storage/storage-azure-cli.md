<properties
    pageTitle="Azure'i salvestusruumi Azure CLI abil | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure käsurea liides (Azure'i CLI) Azure Storage luua ja hallata salvestusruumi kontod ja Azure plekid ja failidega töötamiseks. Azure'i CLI on mitu platvormi tööriist "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Azure'i salvestusruumi Azure CLI kasutamine

## <a name="overview"></a>Ülevaade

Azure'i CLI pakub avatud allikas komplekti platvormidel käsud töötamise Azure'i platvormi. Suur osa sellest sama funktsiooni leitud [Azure portaali](https://portal.azure.com) kui ka rikkaliku andmed Accessi toimimisele pakub.

Sellest juhendist uurime erinevaid Azure Storage arengu ja haldamise ülesannete täitmiseks [Azure'i käsurea liides (Azure'i CLI)](../xplat-cli-install.md) kasutamise kohta. Soovitame teil alla laadida ja installida või versioonile uusim Azure CLI enne selle juhendi abil.

Sellest juhendist eeldab, et mõistate Azure'i salvestusruumi põhimõtted. Juhend pakub mitmesuguseid skriptide näidata Azure'i CLI Azure Storage kasutamist. Ärge unustage skripti muutujaid vastavalt kasutatavale konfiguratsioonile enne iga skripti käivitamine.

> [AZURE.NOTE] Kava pakub Azure'i CLI käsk ja skripti näited klassikaline salvestusruumi kontod. Leiate [Azure'i CLI Mac, Linux, ja Windows Azure'i ressursside haldamise abil](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) Azure'i CLI käsud ressursihaldur salvestusruumi kontod.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Alustamine Azure Storage ja Azure CLI 5 minutit.

Sellest juhendist kasutab Ubuntu näiteid, kuid teiste platvormide OS peaks samuti teha.

**Uue Azure:** Microsoft Azure'i tellimust ja selle tellimusega seostatud Microsofti konto hankida. Azure'i ostmise suvandite kohta leiate teavet leiate [Tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/), [Ostke suvandid](https://azure.microsoft.com/pricing/purchase-options/)ja [Liikme pakub](https://azure.microsoft.com/pricing/member-offers/) (MSDN-i, Microsoft Partner Network, ja BizSpark ja muude Microsofti programmide puhul).

Azure'i abonementide kohta leiate lisateavet teemast [administraatorirollide Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Pärast Microsoft Azure'i tellimus ja konto loomiseks:**

1. Laadige alla ja installige Azure'i CLI [installida Azure CLI](../xplat-cli-install.md)kirjeldatud juhiseid.
2. Kui Azure CLI on installitud, saab Azure'i CLI käskudele juurdepääsemiseks azure käsu kaudu oma käsurea liides (Bash, Terminal, Käsuviip) abil. Tippige `azure` käsk ja te peaksite nägema järgmine väljund.

    ![Azure'i käsu väljund][Image1]

3. Tippige soovitud käsurea liides, `azure storage` leiate Azure'i salvestusruumi käskude andmiseks loend ja saada esimene mulje funktsioonide Azure'i CLI. Saate tippida **-h** parameetriga käsu nimi (nt `azure storage share create -h`) käsu süntaks üksikasjade kuvamiseks.
4. Nüüd pakume teile lihtsa skripti, mis näitab põhilised Azure'i CLI käsud Azure Storage juurdepääsu. Skripti esmalt palub teil määrata kahte muutujat salvestusruumi konto ja võti. Seejärel skripti uus ümbris salvestusruumi uue konto loomine ja selle container olemasoleva pildifaili (bloobimälu) üles laadida. Pärast skripti loetleb kõik plekid selle ümbrises, laadib pildifaili sihtkausta, mis on olemas kohalikus arvutis.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. Kohalikus arvutis, avage oma eelistatud tekstiredaktor (nt vim). Tippige oma tekstiredaktoris ülaltoodud skripti.

6. Nüüd, peate värskendama skripti muutujate põhjal sätete konfigureerimine.

    - **< storage_account_name >** Skript kasutamine antud nimi või sisestage uus nimi salvestusruumi konto jaoks. **Oluline:** Salvestusruumi konto nimi peab olema kordumatu Azure. See peab olema väiketähtedes, liiga!

    - **< storage_account_key >** Kiirklahv konto salvestusruumi.

    - **< container_name >** Skript kasutamine antud nimi või sisestage uus nimi oma ümbrises.

    - **< image_to_upload >** Sisestage tee pildi oma kohalikus arvutis, näiteks: "~ / images/HelloWorld.png".

    - **< destination_folder >** Sisestage tee kohalikku kausta salvestada faile alla laadida Azure Storage, näiteks: "~/downloadImages".

7. Kui olete vajalikud muutujad vim värskendanud, vajutage klahvikombinatsioone "paoklahvi (ESC),:, wq!" salvestamiseks.

8. Käivitage see skript, tippige bash konsooli lihtsalt skripti faili nimi. Pärast seda skripti käivitamist peaks teil olema kohaliku sihtkausta, mis sisaldab allalaaditud faili. Järgmine pilt on kuvatud näide väljund.

Pärast skripti, peaks teil olema kohaliku sihtkausta, mis sisaldab allalaaditud faili.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Salvestusruumi kontod Azure'i CLI haldamine

### <a name="connect-to-your-azure-subscription"></a>Ühenduse loomine Azure tellimuse

Kuigi enamik salvestusruumi käsud ei tööta ilma Azure tellimuse, soovitame teil ühenduse loomiseks oma tellimuse Azure'i CLI. Azure'i CLI töötamiseks tellimuse konfigureerimiseks järgige [Azure tellimuse Azure'i CLI kaudu ühenduse loomine](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Mäluruumi uue konto loomine

Azure'i salvestusruumi kasutamiseks peate salvestusruumi konto. Saate luua uue Azure storage konto pärast tellimuse ühenduse arvuti on konfigureeritud.

        azure storage account create <account_name>

Teie salvestusruumi konto nimi peab olema vahel 3 ja 24 märki ja kasutada numbreid ja väiketähed ainult tähti.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Keskkonna muutujate vaikimisi Azure storage konto määramine

Teie tellimus võib olla mitu salvestusruumi kontod. Saate valida ühe ja määraks keskkonna muutujate salvestusruumi käsud sama seansi jooksul. See võimaldab teil käivitada ilma salvestusruumi konto Azure CLI salvestusruumi käsud ja põhilised otseselt.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Teine võimalus määrata vaikimisi salvestusruumi konto kasutab ühendusstring. Esiteks hankida ühendusstringi käsuga:

        azure storage account connectionstring show <account_name>

Seejärel kopeerige ühendusstring väljundi ja määrake selle väärtuseks muutuja:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Saate luua ja hallata plekid

Azure'i bloobimälu on teenus salvestamiseks suure hulga struktureerimata andmed, nt teksti või binaarsed andmed, millele pääseb juurde kõikjalt maailmas HTTP- või HTTPS-i kaudu. Selles jaotises eeldab, et teil on juba tuttavad Azure'i bloobimälu salvestusruumi põhimõtet. Üksikasjalikku teavet leiate teemast [Alustamine Azure'i bloobimälu kasutades .net-i](storage-dotnet-how-to-use-blobs.md) ja [Bloobimälu teenuste kontseptsioonide](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Ümbris loomine

Ümbris peab olema iga laos Azure'i bloobimälu. Saate luua privaatne container kohta, kasutades funktsiooni `azure storage container create` käsk:

        azure storage container create mycontainer

> [AZURE.NOTE] On kolm taset anonüümse lugemisõigus: **väljalülitamine**, **bloobimälu**ja **ümbrises**. Anonüümse juurdepääsu vältimiseks plekid, seadke parameetri õiguse **off**. Vaikimisi uus ümbris on privaatne ja pääsevad juurde ainult konto omanik. Anonüümse avaliku lugemisõigus bloobimälu ressursse, kuid mitte container metaandmete või loendisse plekid ümbrises lubamiseks parameetri õiguste määramine **bloobimälu**. Täielik avaliku lugemisõigus bloobimälu ressursid lubamiseks container metaandmete ja plekid ümbrises, loendi seatud õiguste parameetri **ümbrises**. Lisateavet leiate teemast [haldamine anonüümse lugemisõigus ümbriste ja plekid](storage-manage-access-to-resources.md).

### <a name="upload-a-blob-into-a-container"></a>Ümbris laadida soovitud bloobimälu

Azure'i bloobimälu toetab Blokeeri plekid ja lehe plekid. Lisateabe saamiseks vt [mõistmine Blokeeri plekid, plekid lisada, ja lehe plekid](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Üles laadida plekid ümbris, saate kasutada funktsiooni `azure storage blob upload`. Vaikimisi lisatud selle käsu kohalike failide blokeerimise bloobimälu. Määrake soovitud bloobimälu jaoks tüüp, võite kasutada funktsiooni `--blobtype` parameeter.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>Laadige alla plekid pakendist

Järgmises näites näitab, kuidas alla laadida plekid pakendist.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>Kopeerige plekid

Saate kopeerida asünkroonselt plekid sees või salvestusruumi kontod ja piirkondade vahel.

Järgmises näites näitab, kuidas plekid salvestusruumi ühelt kontolt teisele kopeerimiseks. Selles näites loome ümbris, kus plekid on avalikult, anonüümselt kättesaadav.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

See teostab selle asünkroonne koopia. Saate jälgida olekut iga koopia toiming töötab soovitud `azure storage blob copy show` toiming.

Pange tähele, et lähte-URL, mis on mõeldud kopeerimine peate olema avalikult juurdepääsetava või kaasata märgiks SAS (ühiskasutusega juurdepääsu signatuur).

### <a name="delete-a-blob"></a>Kustutamine on bloobimälu

Kustutada mõne bloobimälu, kasutage funktsiooni all käsk:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Saate luua ja hallata failikettad

Azure'i failide talletamine pakub jagatud salvestusruumi rakenduste standard SMB protokolli abil. Microsoft Azure'i virtuaalmasinates ja pilveteenustega, samuti kohapealse rakendused, saate jagada faili kaudu ühendatud ühtlasi. Saate hallata failikettad ja faili andmetele Azure'i CLI kaudu. Azure'i failisalvestusruumi kohta leiate lisateavet teemast [alustamine Windows Azure'i failide talletamine](storage-dotnet-how-to-use-files.md) ja [Kuidas kasutada Azure failide talletamine Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Faili ühiskasutus loomine

Mõne Azure'i failikettal on Azure SMB faili osa. Failide ühiskasutamine tuleb luua kõik kataloogid ja failid. Konto võib sisaldada piiramatu arvu osakud ja osa saate talletada piiramatu arv failide salvestusruumi konto võimsus piires. Järgmises näites luuakse failikettale nimega **myshare**.

        azure storage share create myshare

### <a name="create-a-directory"></a>Looge kaust

Kataloogi pakub on valikuline hierarhiat on Azure faili ühiskasutusse andmine. Järgmises näites luuakse kataloogi **myDir** faili ühiskasutus.

        azure storage directory create myshare myDir

Märkus selle kausta tee saate kaasata mitu taset, *nt* **a ja b**. Siiski peab tagada, et kõik ema kataloogid olemas. Näiteks tee **lisamine / b**, peate kataloogi **on** esmalt looma, siis directory **b**loomine.

### <a name="upload-a-local-file-to-directory"></a>Kohaliku faili üleslaadimine kataloog

Järgmises näites on lisatud faili **~/temp/samplefile.txt** **myDir** kataloogi. Redigeeri faili tee nii, et see oleks lubatud faili oma kohalikus arvutis:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Pange tähele, et faili ühiskasutus võib olla kuni 1 TB suurusega.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Faili ühiskasutus juurkausta või kataloog

Saate loetleda faile ja alamkatalooge ühiskasutus juurkausta või kataloogis järgmine käsk:

        azure storage file list myshare myDir

Pange tähele, et kataloogi nimi on valikuline kirjet selle toimingu jaoks. Kui välja jäetud, on loetletud käsk ühiskasutus root kataloogi sisu.

### <a name="copy-files"></a>Failide kopeerimine

Alates Azure'i CLI 0.9.8 versiooni, saate kopeerida faili mõne muu faili, on bloobimälu või faili lisamine bloobimälu faili. Allpool näitame, kuidas neid kasutada CLI käske Kopeeri toimingute tegemiseks. Faili kopeerimine uue kataloogi:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Faili kataloogi on bloobimälu kopeerimine

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Järgmised sammud

Siin on mõned seotud artiklid ja Azure Storage kohta rohkem ressursse.

- [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)
- [Azure'i salvestusruumi REST API viide](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
