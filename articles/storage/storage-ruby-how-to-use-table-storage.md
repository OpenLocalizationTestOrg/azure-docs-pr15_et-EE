<properties
    pageTitle="Azure'i Tabelimälu kaudu Ruby kasutamine | Microsoft Azure'i"
    description="Liigendatud andmete talletamiseks pilveteenuses Azure'i tabelimälu, NoSQL andmete poe kaudu."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>Kuidas kasutada Ruby: Azure'i Tabelimälu

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas teha levinud stsenaariumi Azure'i tabeli teenuse abil. Näidiste kirjutada Ruby API-ga. Stsenaariumid, mis hõlmab kaasata **loomise ja tabeli kustutamine, lisamine ja päringute üksuste tabelis**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Foneetiline rakenduse loomine

Juhised leiate [Ruby on Rails veebirakenduse on Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)foneetiline rakenduse loomine.


## <a name="configure-your-application-to-access-storage"></a>Rakenduse salvestusruumi juurdepääsu konfigureerimine

Azure Storage kasutamiseks peate alla laadima ja foneetiline Azure'i pakett, mis sisaldab kogumi mugavuse teekide puhul, kus suhelda salvestusruumi ülejäänud teenuseid kasutada.

### <a name="use-rubygems-to-obtain-the-package"></a>Kasutage RubyGems saada pakett

1. Kasutage käsurea liides, nt **PowerShell** (Windows), **terminalis** (Mac) või **Bash** (Unix).

2. Tippige käsuviiba aken installida gem ja sõltuvused **gem installida azure** .

### <a name="import-the-package"></a>Paketi importimine

Kasutage oma lemmik tekstiredaktorit, lisage järgmine foneetiline fail, kuhu kavatsete kasutada salvestusruumi ülaosas:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Azure'i salvestusruumi-ühenduse häälestamine

Azure'i mooduli ei loe keskkonna muutujate **AZURE'I\_salvestusruumi\_konto** ja **AZURE'I\_salvestusruumi\_ACCESSI\_klahvi** Azure Storage oma kontoga ühenduse loomiseks vajalikku teavet. Kui need keskkonna muutujad on pole määratud, peate määrama kontoteabe enne **Azure::TableService** abil järgmine kood:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Klassikaline või ressursihaldur salvestusruumi konto Azure'i portaalis saada need väärtused:

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

## <a name="create-a-table"></a>Tabeli loomine

Objekti **Azure::TableService** võimaldab töötada tabelite ja üksused. Tabeli loomiseks kasutada funktsiooni **loomine\_table()** meetod. Järgmises näites luuakse tabeli või printige see tõrge, kui on olemas.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Üksuse lisamine tabelisse

Üksuse lisamiseks esmalt luua räsi objekti, mis määratleb oma üksuse atribuudid. Pange tähele, et iga üksuse jaoks peate määrama **PartitionKey** ja **RowKey**. Need on Kordumatud tunnused oma üksuste ja on väärtused, mida saate kasutataks kiiremini oma muid atribuute. Azure'i salvestusruumi kasutab **PartitionKey** jaotab automaatselt selle tabeli üksuste üle palju salvestusruumi sõlmi. Sama **PartitionKey** üksustel on talletatud sama sõlme. **RowKey** on see kuulub sektsioonis üksuse Ainuidentifikaator.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Üksuse värskendamine

On saadaval värskendada olemasoleva üksuse mitut meetodit.

* **värskendamine\_entity():** Olemasoleva üksuse värskendada, asendades selle.
* **Ühenda\_entity():** Värskendab olemasoleva üksuse, uue kinnisvarahindade ühendatakse olemasolevate üksus.
* **lisamine\_või\_Ühenda\_entity():** Olemasoleva üksuse värskendab, asendades selle. Kui ole üksus on olemas, lisatakse uus:
* **lisamine\_või\_asendamine\_entity():** Värskendab olemasoleva üksuse, uue kinnisvarahindade ühendatakse olemasolevate üksus. Kui ole üksus on olemas, lisatakse uus.

Järgmises näites näitab värskendamine kasutav üksus **värskendada\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

Koos **värskendada\_entity()** ja **Ühenda\_entity()**, kui üksus, mida te värskendate pole olemas, siis värskendamist ei õnnestu. Seetõttu kui soovite talletada üksus sõltumata sellest, kas see on juba olemas, peaksite kasutama **lisamine\_või\_asendamine\_entity()** või **lisamine\_või\_Ühenda\_entity()**.

## <a name="work-with-groups-of-entities"></a>Üksuste jaotistega töötamine

Mõnikord on mõistlik esitada mitut toimingute tagamiseks atomic töötlemine server partii. Mis täita esmakordsel loomisel **paketi** objekti ning seejärel kasutage soovitud **käivitada\_batch()** **TableService**meetod. Järgmises näites näitab esitamise kaks üksust RowKey 2 ja 3 partii. Pange tähele, et see toimib ainult sama PartitionKey üksustel.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Päringu üksuse

Päringu tabeli üksuse, kasutage funktsiooni **hankimine\_entity()** meetod tabeli nime, **PartitionKey** ja **RowKey**.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Päringu määratud üksuste

Päringu määratud üksuste tabeli, päringu räsi objekti loomine ja kasutamine on **päringu\_entities()** meetod. Järgmises näites näitab, et saada kõik üksused koos sama **PartitionKey**:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Kui tulem on liiga suur ühe päringu tagastamiseks, tagastatakse jätkamiseks luba, mille abil saate tuua järgmiste lehtede.

## <a name="query-a-subset-of-entity-properties"></a>Päringu alamhulga üksuse atribuudid

Päringu tabeli saate tuua vaid mõned atribuudid üksus. Selle meetodi, nimega "projektsiooni" vähendab läbilaskevõime ja parandada päringu jõudluse, eriti suurte üksuste puhul. Kasutage select-klauslis ja edastama soovite viia üle kliendi atribuutide nimesid.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Üksuse kustutamine

Kustuta üksus, kasutage funktsiooni **kustutamine\_entity()** meetod. Peate läbima tabel, mis sisaldab üksus, PartitionKey ja RowKey üksuse nimi.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Tabeli kustutamine

Tabeli kustutamiseks, kasutage funktsiooni **kustutamine\_table()** meetod ja edastamiseks nime tabel, mille soovite kustutada.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Järgmised sammud

Salvestusruumi keerukamate tööülesannete kohta lisateabe saamiseks järgige neid linke:

- [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure'i Tarkvaraarenduskomplektist, mis](http://github.com/WindowsAzure/azure-sdk-for-ruby) hoidla github
