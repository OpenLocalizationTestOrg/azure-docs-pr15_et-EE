<properties
    pageTitle="Kasutamise bloobimälu (objekti salvestusruumi) Ruby | Microsoft Azure'i"
    description="Azure'i bloobimälu (objekti salvestusruumi) koos pilveteenuses talletada struktureerimata andmed."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Kuidas kasutada bloobimälu Ruby kaudu

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Ülevaade

Azure'i bloobimälu on teenus, mis salvestab struktureerimata andmeid pilves objektide/plekid. Bloobimälu saab hoida mis tahes tüüpi teksti või binaarandmete, nt dokumenti, media fail või rakenduse installer. Bloobimälu ka edaspidi objekti salvestusruumi.

Sellest juhendist näitab teile, kuidas teha levinud stsenaariumi bloobimälu abil. Näidiste kirjutada Ruby API-ga. Stsenaariumid, mis hõlmab sisaldavad plekid **üles, alla, loetelu** ja **kustutamine** .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Foneetiline rakenduse loomine

Foneetiline rakenduse loomine. Juhised leiate teemast [Ruby on Rails veebirakenduse on Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Rakenduse salvestusruumi juurdepääsu konfigureerimine

Azure Storage kasutamiseks peate alla laadima ja foneetiline Azure'i pakett, mis sisaldab kogumi mugavuse teekide puhul, kus suhelda salvestusruumi ülejäänud teenuseid kasutada.

### <a name="use-rubygems-to-obtain-the-package"></a>Kasutage RubyGems saada pakett

1. Kasutage käsurea liides, nt **PowerShell** (Windows), **terminalis** (Mac) või **Bash** (Unix).

2. Tippige käsuviiba aken installida gem ja sõltuvused "gem installi azure".

### <a name="import-the-package"></a>Paketi importimine

Kui kavatsete kasutada salvestusruumi foneetiline faili ülaosas abil oma lemmik tekstiredaktoris, järgmine lisamiseks järgmist.

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Mõne Azure Storage ühenduse häälestamine

Azure'i mooduli ei loe keskkonna muutujate **AZURE'I\_salvestusruumi\_konto** ja **AZURE'I\_salvestusruumi\_ACCESS_KEY** Azure salvestusruumi oma kontoga ühenduse loomiseks vajalikku teavet. Kui need keskkonna muutujad on pole määratud, peate määrama kontoteabe enne **Azure::Blob::BlobService** abil järgmine kood:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Klassikaline või ressursihaldur salvestusruumi konto Azure portaali saada need väärtused:

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Liikuge salvestusruumi konto, mida soovite kasutada.
3. Klõpsake paremal labale sätted **Kiirklahvide**.
4. Accessi klahvid tera, mis kuvatakse, näete kiirklahv 1 ja 2 võti. Saate kasutada ühte järgmistest. 
5. Klõpsake ikooni koopia võti lõikelauale kopeerida. 

Klassikaline salvestusruumi konto klassikaline Azure'i portaalis saada need väärtused:

1. [Klassikaline Azure portaali](https://manage.windowsazure.com)sisse logida.
2. Liikuge salvestusruumi konto, mida soovite kasutada.
3. Klõpsake nuppu **Halda KIIRKLAHVIDE** navigeerimispaani allosas.
4. Dialoogiboksi üles pop, kuvatakse salvestusruumikonto nimi, esmane võti ja teisene kiirklahv. Kiirklahv, saate kasutada esmase või teisese ühe. 
5. Klõpsake ikooni koopia võti lõikelauale kopeerida.

## <a name="create-a-container"></a>Ümbris loomine

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Objekti **Azure::Blob::BlobService** võimaldab töötada ümbriste ja plekid. Ümbris loomiseks kasutage funktsiooni **loomine\_container()** meetod.

Koodi järgmises näites luuakse container või printige see tõrge, kui on olemas.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Kui soovite ümbris failid avalikuks, saate kõigepealt ümbrist õigused.

Saate muuta ainult selle <strong>loomine\_container()</strong> kõne edasi soovitud **: avaliku\_Accessi\_taseme** suvand:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Sobivad väärtused on **: avaliku\_Accessi\_taseme** suvand on:

* **bloobimälu:** Saate määrata täielikku avaliku lugemisõigus container ja bloobimälu andmete jaoks. Kliendid saate loetleda plekid container kaudu anonüümse taotluse sees, kuid ei saa nummerdada ümbriste salvestusruumi kontol.

* **container:** Saate määrata plekid avaliku lugemisõigus. See ümbris bloobimälu andmete saab lugeda anonüümse taotluse kaudu, kuid ümbrise andmed pole saadaval. Kliendid ei saa nummerdada plekid sees container anonüümse taotluse kaudu.

Teise võimalusena saate muuta avaliku juurdepääsu tase ümbris, kasutades **seadmine\_container\_acl()** meetodi abil määrata avaliku juurdepääsutase.

Järgmises näites koodi muudatusi **container**avaliku juurdepääsutase.

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Ümbris laadida soovitud bloobimälu

Sisu üleslaadimine on bloobimälu, kasutage funktsiooni **loomine\_Blokeeri\_blob()** meetod on bloobimälu loomiseks kasutada stringi või faili sisu on bloobimälu.

Järgmine kood, laaditakse fail **test.png** nimega uue bloobimälu, nimega "pilt-bloobimälu" ümbrises.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Loendi a ümbrises plekid

Loendis soovitud ümbriste, kasutage **list_containers()** meetodit.
Loendi plekid sees ümbris, kasutage **loendi\_blobs()** meetod.

See väljundid kõik plekid sisse kõik selle konto ümbriste URL-id.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>Laadige alla plekid

Allalaadimiseks plekid, kasutage funktsiooni **hankimine\_blob()** meetodi abil saate tuua sisu.

Järgmine kood näide näitab abil **saada\_blob()** "pilt-bloobimälu" sisu alla laadida ja kirjutada selle kohalik fail.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Kustutamine on bloobimälu
Lõpuks kustutada mõne bloobimälu, kasutage funktsiooni **kustutamine\_blob()** meetod. Järgmine kood näide näitab, kuidas kustutada mõne bloobimälu.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Järgmised sammud

Salvestusruumi keerukamate tööülesannete kohta lisateabe saamiseks järgige neid linke:

- [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure'i Tarkvaraarenduskomplektist, mis](https://github.com/WindowsAzure/azure-sdk-for-ruby) hoidla github
- [AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)
