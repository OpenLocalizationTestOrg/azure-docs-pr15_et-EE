<properties
   pageTitle="Kopeerige andmed Azure salvestusruumi plekid Lake andmesalve | Microsoft Azure'i"
   description="Azure'i salvestusruumi plekid andmete kopeerimine Lake andmesalve AdlCopy tööriista abil"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Azure'i salvestusruumi plekid andmete kopeerimine andmesalve Lake

> [AZURE.SELECTOR]
- [DistCp abil](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy abil](data-lake-store-copy-data-azure-storage-blob.md)

Azure'i andmesalve Lake pakub mõne käsurea tööriista [AdlCopy](http://aka.ms/downloadadlcopy), andmete kopeerimiseks järgmistest allikatest:

* : Azure'i salvestusruumi plekid andmete Lake poest. AdlCopy abil ei saa andmeid kopeerida Lake andmesalve Azure Storage plekid.

* Vahel kaks Azure'i andmesalve Lake kontod. 

Samuti saate AdlCopy tööriista kaks erinevat režiimi:

* **Autonoomselt**, kus see tööriist kasutab Lake andmesalve ressursid soovitud toimingu sooritamiseks.

* **Andmeanalüüsi Lake konto abil**, kus kasutatakse ühikute Lake andmeanalüüsi kontole määratud toimingu Kopeeri. Võite selle suvandi kasutamiseks, kui otsite prognoositavad viisil Kopeeri ülesannete täitmiseks.

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- **Azure'i salvestusruumi plekid** container mõned andmetega.

- **Azure'i andmeanalüüsi Lake konto (valikuline)** – vt [Alustamine Azure'i andmeanalüüsi Lake](../data-lake-analytics/data-lake-analytics-get-started-portal.md) juhised, kuidas luua Lake andmesalve konto.

- **AdlCopy tööriista**. Installige tööriist AdlCopy [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Süntaks: AdlCopy tööriista

Töötamine AdlCopy tööriista järgmise süntaksi abil

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Parameetrid süntaks on kirjeldatud allpool.

| Suvand    | Kirjeldus                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Allikas    | Saate määrata asukoha lähteandmete salvestusruumi Azure'i bloobimälu. Allika võib olla bloobimälu container, on bloobimälu või muu Lake andmesalve konto.                                                                                                                                                                                                                                                                                                 |
| Dest      | Saate määrata Lake andmesalve sihtkohta kopeerida.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Saate määrata salvestusruumi kiirklahv salvestusruumi Azure'i bloobimälu allikat. See on vajalik ainult siis, kui Lähtevaluuta on bloobimälu container või mõne bloobimälu.                                                                                                                                                                                                                                                                                                                                                 |
| Konto   | **Valikuline**. Kasutage seda, kui soovite kasutada Azure andmeanalüüsi Lake konto Kopeeri töö käivitamiseks. Kui suvand /Account kasutamine süntaks, kuid ei määra Lake andmeanalüüsi konto, kasutab AdlCopy vaikekonto töö käivitamiseks. Ka, kui kasutate seda suvandit, peate lisama lähte (Azure'i salvestusruumi bloobimälu)-kui ka (Azure'i Lake andmesalve) andmeallikate Lake andmeanalüüsi konto jaoks.  |
| Ühikud     |  Saate määrata Lake andmeanalüüsi ühikute Kopeeri töö kasutatud. See suvand on kohustuslik, kui **/Account** suvandi abil saate määrata Lake andmeanalüüsi konto.
| Mustri   | Saate määrata regex mustri, mis näitab, milliseid plekid või faile kopeerida. AdlCopy kasutab tõstutundlik sobitamine. Vaikimisi muster kasutada, kui määratud pole mustri on kõigi üksuste kopeerimiseks. Määrata mitu faili mustreid ei toetata.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Kasutada AdlCopy (eraldi) andmeid kopeerida mõne salvestusruumi Azure'i bloobimälu

1. Avage käsuviip ja liikuge kausta, kuhu AdlCopy on installitud, tavaliselt `%HOMEPATH%\Documents\adlcopy`.

2. Source container teatud bloobimälu kopeerimiseks andmete Lake poe järgmine käsk:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Näiteks:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] Ülaltoodud süntaksit määrab faili kaustas Lake andmesalve konto kopeerida. AdlCopy tööriista loob kausta, kui määratud kausta nimi pole olemas.

    Teil palutakse sisestada mandaat Azure'i tellimus, mille olete oma Lake andmesalve konto. Kuvatakse järgmine väljund.

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Saate kopeerida ka kõik plekid ühe container Lake andmesalve kontoga järgmine käsk:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Näiteks:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Kasutada AdlCopy (eraldi) andmeid kopeerida Lake andmesalve teisele kontole

Saate AdlCopy kaks Lake andmesalve kontode vahel andmete kopeerimiseks.

1. Avage käsuviip ja liikuge kausta, kuhu AdlCopy on installitud, tavaliselt `%HOMEPATH%\Documents\adlcopy`.

2. Käivitage järgmine käsk mingit kindlat tüüpi failiga Lake andmesalve ühelt kontolt teisele kopeerimiseks.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Näiteks:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] Ülaltoodud süntaksit määrab faili sihtkoht Lake andmesalve konto kausta kopeerida. AdlCopy tööriista loob kausta, kui määratud kausta nimi pole olemas.

    Teil palutakse sisestada mandaat Azure'i tellimus, mille olete oma Lake andmesalve konto. Kuvatakse järgmine väljund.

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Järgmine käsk valiku puhul kopeeritakse kõik failid kaust allikas Lake andmesalve konto sihtkoht Lake andmesalve konto kausta.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Andmete kopeerimiseks kasutada AdlCopy (andmeanalüüsi Lake kontoga)

Andmeanalüüsi Lake konto abil saate käivitada Azure storage plekid andmete kopeerimine Lake andmesalve AdlCopy töö. Te tavaliselt kasutada seda suvandit, kui andmete teisaldamiseks on vahemikus gigabaiti ja TB ning soovite hõlpsalt prognoosida ja parema jõudluse läbilaskevõime.

Kasutamiseks andmeanalüüsi Lake konto AdlCopy kopeerida mõnda salvestusruumi Azure'i bloobimälu lisatakse source (Azure'i salvestusruumi bloobimälu) andmeallikana Lake andmeanalüüsi konto jaoks. Täiendavad andmeallikate Lake andmeanalüüsi kontole lisamise juhised leiate teemast [haldamine Lake andmeanalüüsi konto andmeallikad](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Kui kopeerite konto Azure andmesalve Lake allikana Lake andmeanalüüsi kontoga, peate konto Lake andmesalve seostada Lake andmeanalüüsi konto. Andmeallika poe seostada Lake andmeanalüüsi konto on ainult siis, kui Lähtevaluuta on Azure Storage konto.

Kopeerimiseks mõnda salvestusruumi Azure'i bloobimälu Lake andmeanalüüsi kontoga Lake andmesalve konto järgmine käsk:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Näiteks:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Samamoodi, käivitage järgmine käsk kopeerimiseks mõnda salvestusruumi Azure'i bloobimälu Lake andmesalve konto Lake andmeanalüüsi kontoga:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Kopeerige andmed, kasutades mallvõrdluse AdlCopy abil

Selles jaotises saate teada, kuidas andmete kopeerimisel mõnest muust allikast AdlCopy abil (järgmises näites me kasutame Azure'i salvestusruumi bloobimälu) abil mallvõrdluse sihtkoha Lake andmesalve konto. Näiteks saate kopeerida kõik failid CSV-laiendiga allika bloobimälu sihtkoht alltoodud juhiseid.

1. Avage käsuviip ja liikuge kausta, kuhu AdlCopy on installitud, tavaliselt `%HOMEPATH%\Documents\adlcopy`.

2. Käivitage järgmine käsk Kopeeri kõik failid laiendiga *.csv teatud bloobimälu ümbrisest allika andmete Lake poest.

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Näiteks:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Arveldamine

* Kui kasutate tööriista AdlCopy autonoomse saate, saadetakse teile arve sealt kulude liikumiseks andmeid, kui allikas Azure Storage konto pole sama piirkonna Lake andmesalve.

* Kui kasutate tööriista AdlCopy Lake andmeanalüüsi kontoga, rakendatakse standard [Lake andmeanalüüsi arvelduse määra](https://azure.microsoft.com/pricing/details/data-lake-analytics/) .

## <a name="considerations-for-using-adlcopy"></a>Teave kasutamise AdlCopy

* AdlCopy (jaoks 1.0.5 versioon), toetab kopeerimine andmete allikatest, mis on rohkem kui tuhat faile ja kaustu. Juhul, kui teil tekib probleeme suure andmekogumi kopeerimine, saate jaotada faile ja kaustu erinevate alamkatalooge ja kasutada tee nende alamkatalooge allikana asemel.

## <a name="next-steps"></a>Järgmised sammud

- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
