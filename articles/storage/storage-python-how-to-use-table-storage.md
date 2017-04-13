<properties
    pageTitle="Kasutamine: Python tabelimälu | Microsoft Azure'i"
    description="Liigendatud andmete talletamiseks pilveteenuses Azure'i tabelimälu, NoSQL andmete poe kaudu."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>Kuidas kasutada tabelimälu Python kaudu

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas salvestusteenus Azure'i tabeli abil saate teha levinud stsenaariumi. Näidiste Python kirjutada ja kasutada [Microsoft Azure'i salvestusruumi SDK Python]. Lepinguga seotud stsenaariumid kaasata loomine ja kustutamine tabelisse Lisaks lisamise ja päringute üksuste tabelis.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Tabeli loomine

Objekti **TableService** võimaldab töötada tabeli teenustega. Järgmine kood tekitab **TableService** objekti. Lisage mis tahes Python faili, kus soovite programmiliselt juurde Azure Storage ülaservas kood.

    from azure.storage.table import TableService, Entity

Järgmine kood loob **TableService** objekti salvestusruumi konto nimi ja konto võti abil.  Asendage "minu konto" ja 'mykey' oma konto nimi ja võti.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Üksuse lisamine tabelisse

Üksuse lisamiseks esmalt looge sõnastiku või üksus, mis määratleb oma üksuse atribuutide nimesid ja väärtused. Pange tähele, et iga üksuse jaoks peate määrama **PartitionKey** ja **RowKey**. Need on Kordumatud tunnused on teie üksused. Saate teha päringuid abil neid väärtusi kui saate teha päringuid oma muid atribuute. **PartitionKey** jaotab automaatselt tabeli üksuste üle palju salvestusruumi sõlmi kasutab süsteem.
Üksused, millel on sama **PartitionKey** on talletatud sama sõlme. **RowKey** on kordumatu ID üksuse sektsioonis, mis kuulub.

Üksuse lisamiseks tabeli edastavad sõnastiku objekti soovitud **lisamine\_üksus** meetod.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

Samuti saab edastada eksemplari klassi **üksus** on **lisamine\_üksus** meetod.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Üksuse värskendamine

Järgmine kood näitab, kuidas vanemat versiooni olemasoleva üksuse asendamiseks värskendatud versiooni.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Kui üksus, mida on värskendatud olemas, siis värskendamist ei õnnestu. Kui soovite üksuse sõltumata sellest, kas see on olemas enne talletada, kasutada **lisamine\_või\_replace_entity**.
Järgmises näites esimene asendab olemasoleva üksus. Teine kõne lisab uue üksuse, kuna ole üksus koos määratud **PartitionKey** ja **RowKey** tabelis on olemas.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Üksuste rühma muutmine

Mõnikord on mõistlik esitada mitut toimingute tagamiseks atomic töötlemine server partii. Tegemise, et kasutate **TableBatch** klassi. Kui soovite esitada paketi, saate kõne **Kinnita\_paketi**. Pange tähele, et kõik üksused peab olema sama sektsiooni selleks, et muuta partii. Järgmises näites liidab kaks üksust partii.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Töölehed saab kasutada ka Konktekstowe halduri süntaks:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Päringu üksuse

Päringu tabeli üksuse, kasutage funktsiooni **saada\_üksus** **PartitionKey** ja **RowKey**meetod.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Päringu määratud üksuste

Selles näites leiab kõik tööülesanded Seattle põhjal **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Päringu alamhulga üksuse atribuudid

Päringu tabeli saate tuua vaid mõned atribuudid üksus.
Selle meetodi, mida nimetatakse *projektsiooni*, vähendab läbilaskevõime ja parandada päringu jõudluse, eriti suurte üksuste puhul. **Valige** parameetri ja edastama nimed, atribuudid, mida soovite tuua üle kliendile.

Järgmine kood päringu tagastab tabeli ainult üksuste kirjeldused.

[AZURE.NOTE] Järgmised koodilõigu toimib ainult vastu salvestusruumi pilveteenuses. See ei toeta salvestusruumi emulaator.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Üksuse kustutamine

Saate kustutada selle sektsiooni ja rida klahvi abil üksus.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Tabeli kustutamine

Järgmine kood kustutab tabeli salvestusruumi konto.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud tabelimälu põhitõdesid, järgige neid linke lisateabe.

- [Python Arenduskeskus](/develop/python/)
- [Azure'i salvestusruumi teenuste REST API-ga](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure'i salvestusruumi meeskonna ajaveeb]
- [Microsoft Azure'i salvestusruumi SDK Python]

[Azure'i salvestusruumi meeskonna ajaveeb]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure'i salvestusruumi SDK Python]: https://github.com/Azure/azure-storage-python
