<properties
    pageTitle="Taru tabeli taru päringutega andmete uurimine | Microsoft Azure'i"
    description="Taru tabelite taru päringute kasutamine andmete uurimine."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Taru tabeli taru päringutega andmete uurimine

Selles dokumendis on toodud taru näidisskriptid, on kasutatud on Hdinsightiga Hadoopi kobar taru tabelite analüüsimiseks.

**Menüü** viivat teemadele, kus kirjeldatakse, kuidas erinevate salvestusruumi keskkonnas: andmete uurimine tööriistade abil.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et teil on:

* Loodud Azure storage konto. Kui vajate juhiseid, lugege teemat [Azure Storage konto loomine](../storage/storage-create-storage-account.md#create-a-storage-account)
* Ette valmistatud kohandatud Hadoopi kobar Hdinsightiga teenusega. Kui vajate juhiseid, lugege teemat [Kohandamine Azure Hdinsightiga Hadoopi kogumite Analyticsi täpsemalt](machine-learning-data-science-customize-hadoop-cluster.md).
* Andmed on laaditud Azure Hdinsightiga Hadoopi rühmades taru tabelitele. Kui see pole, järgige juhiseid [taru tabelite loomine ja laadi andmed](machine-learning-data-science-move-hive-tables.md) esmalt taru tabelite andmete üleslaadimiseks.
* Lubatud Kaugpöördusteenuse klaster. Kui vajate juhiseid, lugege teemat [Accessi pea sõlm Hadoopi kobar](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Kui teil on vaja juhised, kuidas esitada taru päringud, vaadake, [Kuidas esitada taru päringud](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Näide taru päringu skriptide andmete uurimine

1. Saada partition vaatluste arv `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Saada päevas vaatluste arv `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Tasemete kategooriline veerus hankimine  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Saada tasemete arvu kahe kategooriline veeru kombinatsioon `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Jaotuse arvväärtuste veergude hankimine  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Kirjete ekstraktida kahe tabeli liitumine

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Päringu skriptide takso reisi andmete stsenaariumid

Päringud, mis on omased [NYC takso reisi andmete](http://chriswhong.com/open-data/foil_nyc_taxi/) stsenaariumid näited on olemas [Github hoidla](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Need päringud juba on määratud andmete skeem ja käivitamiseks valmis.
