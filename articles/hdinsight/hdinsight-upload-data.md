<properties
    pageTitle="Laadi andmed Hadoopi töökohta, Hdinsightile | Microsoft Azure'i"
    description="Saate teada, kuidas üles laadida ja juurdepääs Hadoopi töökohtade Hdinsightiga Azure CLI, Azure salvestusruumi Explorerit, Azure PowerShelli, Hadoop käsurea või Sqoop andmeid."
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Laadi andmed Hadoopi töökohta, Hdinsightiga

Azure Hdinsightiga leiate Azure'i bloobimälu üle kõiki võimalusi pakkuva Hadoopi failisüsteemi (HDFS). See on mõeldud klientidele tõrgeteta esitada HDFS laiendamine. See võimaldab täiskomplekti Hadoopi ökosüsteemis sõitmiseks hallatavate andmete komponendid. Azure'i bloobimälu ja HDFS on erinevate failisüsteemi, mis on optimeeritud andmete ja arvutuste, et andmeid. Azure'i bloobimälu kasutamise eeliste kohta leiate teemast [kasutamine Azure'i bloobimälu Hdinsightiga][hdinsight-storage].

**Eeltingimused**

Enne alustamist, võtke arvesse järgmisi nõudeid.

* Mõne Windows Azure Hdinsightiga kobar. Juhised leiate teemast [alustamine Windows Azure Hdinsightiga] [ hdinsight-get-started] või [sätte Hdinsightiga kogumite][hdinsight-provision].

##<a name="why-blob-storage"></a>Bloobivahemälu miks salvestusruumi?

Azure Hdinsightiga kogumite on tavaliselt juurutatud MapReduce tööde ja rühmad kõrvaldatakse pärast need tööd. Kuna andmeid hoitakse soovitud HDFS kogumite arvutuste on lõpetatud oleks kallis viis andmete talletamiseks. Azure'i bloobimälu on väga kättesaadav, väga paindlik, võimsus, madal ja jagatav salvestusruumi suvand andmeid töödeldakse Hdinsightiga abil. Andmete talletamine on bloobimälu võimaldab Hdinsightiga rühmad kasutatavate arvutus kaotamata andmeid turvaliselt vabastatakse.

###<a name="directories"></a>Kataloogide

Azure'i bloobimälu salvestusruumi ümbriste nimega /-väärtuse paarideks andmete talletamiseks ja puudub directory hierarhia. Märgi "/" saate siiski kasutada võtme nimi ilmuks, kui fail on salvestatud kataloogi struktuuri. Hdinsightiga näeb neid, kui need on tegelik kataloogide.

Näiteks võib mõne bloobimälu klahvi *input/log1.txt*. Tegelik "Sisestuskeel" kataloogi olemas, kuid tõttu võtme nimi "/" märgi, on selle faili tee ilmet.

Seetõttu, kui kasutate Azure Exploreris tööriistad märkate 0 bait mõned failid. Need failid on kaks otstarvet:

- Kui loendis on tühjad kaustad, märkida need kausta olemasolu. Azure'i bloobimälu on piisavalt tark, et leida, kui on bloobimälu nimetatakse foo/lint on olemas, on kaust nimega **foo**. Kuid ainus viis tähenda tühja kausta nimega **foo** on koht, millel see teisiti 0-baidine faili.

- Neil teisiti metaandmed, mis on vajalik Hadoopi failisüsteemi, eriti õiguste ja kaustade omanikud.

##<a name="command-line-utilities"></a>Käsurea Utiliidid

Microsoft osutab järgmised Utiliidid Azure'i bloobimälu töötamiseks:

| Tööriist | Linux | OS X | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure'i käsurea liides][azurecli] | ✔ | ✔ | ✔ |
| [Azure'i PowerShelli][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Hadoopi käsk](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Välise Azure, Hadoop käsk on saadaval Hdinsightiga kobar ainult ja võimaldab ainult kohaliku failisüsteemi andmete laadimine Azure'i bloobimälu kasutada ajal Azure'i CLI, Azure PowerShelli ja AzCopy saavad kõik.

###<a id="xplatcli"></a>Azure'i CLI

Azure'i CLI on mitu platvormi tööriista, mis võimaldab teil hallata Azure teenused. Azure'i bloobimälu andmete üleslaadimiseks tehke järgmist:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installida ja konfigureerida Mac, Linux ja Windows Azure'i CLI](../xplat-cli-install.md).

2. Avage käsuviip, bash või muude shell ja kasutage autentimiseks Azure tellimuse järgmist.

        azure login

    Küsimise korral sisestage tellimuse kasutajanimi ja parool.

3. Sisestage oma tellimusele mäluruumi kontode loend järgmine käsk:

        azure storage account list

4. Valige salvestusruumi konto, mis sisaldab bloobimälu, mida soovite töötada, siis järgmise käsu abil saate tuua selle konto võti:

        azure storage account keys list <storage-account-name>

    See peaks tagastama **esmaseid** ja **teiseseid** võtmed. Kopeerige **primaartabeli** väärtus, kuna seda kasutatakse järgmist.

5. Järgmise käsu abil saate tuua bloobimälu ümbriste salvestusruumi kontol loendit:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Saate kasutada järgmisi käske üles laadida ja selle bloobimälu faile alla laadida:

    * Faili üleslaadimiseks tehke järgmist.

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Faili allalaadimiseks tehke järgmist.

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Kui te ei tööta alati salvestusruumi sama kontoga, saate määrata järgmised keskkonna muutujad asemel täpsustades konto ja iga käsu võti:
>
> * **AZURE\_salvestusruumi\_konto**: salvestusruumikonto nimi
>
> * **AZURE\_salvestusruumi\_ACCESSI\_klahvi**: salvestusruumi konto võti

###<a id="powershell"></a>Azure'i PowerShelli

Azure'i PowerShelli on skriptimine keskkonnas, mille abil saate kontrollida ja juurutamise ja haldamise oma töökoormus Azure automatiseerida. Oma töökoha Azure PowerShelli käivitamiseks konfigureerimise kohta leiate teemast [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Kohaliku faili üleslaadimiseks Azure'i bloobimälu**

1. Avage Azure'i PowerShelli konsooli, nagu esitatud juhiste järgi [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).
2. Seadke esimesel viiel muutujate väärtused järgmise skripti:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Skripti kleepimine Azure PowerShelli konsooli käivitamiseks selle faili koopia.

Näiteks PowerShelli skriptide loodud tööta Hdinsightiga, et näha [Hdinsightiga tööriistad](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy on käsurea tööriista, mis on mõeldud selle ülesande edastada andmeid ja sealt Azure Storage konto. Saate seda kasutada ka tööriista või selles tööriistas taotluse lisada. [Laadige alla AzCopy][azure-azcopy-download].

AzCopy süntaks on järgmine:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Lisateavet leiate teemast [AzCopy - Azure plekid failide üleslaadimine või allalaadimine][azure-azcopy].


###<a id="commandline"></a>Hadoopi käsurea

Hadoopi käsurea on kasulik andmete salvestamiseks bloobimälu sisse, kui andmed on juba olemas kobar pea sõlme ainult.

Hadoopi käsu kasutamiseks tuleb esmalt ühendada headnode, kasutades ühte järgmistest:

* **Windowsi-põhiste Hdinsightiga**: [Kaugtöölaua ühendus kasutamise](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Linux-põhine Hdinsightiga**: ühenduse loomiseks kasutada SSH ([SSH käsk](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) või [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Kui ühendus on loodud, saate faili üleslaadimine salvestusruumi järgmist süntaksit.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Näiteks`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Kuna jaoks Hdinsightiga failisüsteemi Azure'i bloobimälu, /example/data.txt on tegelikult Azure'i bloobimälu. Saate vaadata ka fail nimega:

    wasbs:///example/data/data.txt

või

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Failidega töötamine teiste Hadoopi käskude loendi leiate teemast [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Klõpsake HBase kogumite, Blokeeri vaikimisi kasutada, kui andmete kirjutamise 256KB suurust. Kuigi see toimib hästi, kasutades HBase API-d või REST API-d, kasutades selle `hadoop` või `hdfs dfs` käsud kirjutamiseks andmete ~ 12GB mahukamate, on tulemuseks viga. [Kirjutage bloobimälu salvestusruumi erand](#storageexception) allpool olevast jaotisest lisateavet.

##<a name="graphical-clients"></a>Graafilise kliendid

Samuti on mitu rakendust, mis pakuvad graafilist liidest Azure Storage töötamiseks. Järgmine on mõned nende rakenduste loend.

| Kliendi | Linux | OS X | Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio Tools for Hdinsightiga](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Azure'i salvestusruumi Explorer](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Pilveteenuse salvestusruumi Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure'i Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools for Hdinsightiga

Lisateabe saamiseks lugege teemat [liikumine lingitud ressursid](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a id="storageexplorer"></a>Azure'i salvestusruumi Explorer

*Azure'i salvestusruumi Explorer* on kasulik kontrollimise ja andmete plekid muutmata. See on tasuta, avatud allika tööriist, mida saab alla laadida [http://storageexplorer.com/](http://storageexplorer.com/). Lähtekoodi on saadaval ka selle link.

Enne tööriista abil peate teadma oma Azure storage konto nimi ja konto võti. Juhised selle kohta, kuidas seda teavet leiate teemast on "kohta: vaade, kopeerimine ja uuesti luua salvestusruumi kiirklahvide" jaotis [loomine, haldamine, või salvestusruumi konto kustutada][azure-create-storage-account].  

1. Käivitage Azure Storage Explorer. Kui see on esimene kord, kui olete parandusfunktsiooni salvestusruumi Explorer, teil palutakse ___Salvestusruumikonto nimi__ ja __salvestusruumi konto võti__. Kui olete parandusfunktsiooni see enne kasutamist uue salvestusruumikonto nimi ja klahvi lisamine nuppu __Lisa__ .

    Sisestage soovitud nimi ja salvestusruumi konto võti kasutatavaid Hdinsightiga klaster ja valige __salvestamine ja avamine__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Klõpsake ümbriste kasutajaliideses vasakul loendis ümbris, mis on seotud Hdinsightiga klaster nime. Vaikimisi see on nimi Hdinsightiga kobar, kuid võib olla erinev, kui teatud nime sisestanud klaster loomisel.

6. Valige tööriist üles ikoon.

    ![Tööriista riba ikoon: üleslaadimine on esile tõstetud](./media/hdinsight-upload-data/toolbar.png)

7. Määrake faili üles laadida, ja klõpsake nuppu **Ava**. Vastava viiba kuvamisel valige __üles laadida__ faili üleslaadimine salvestusruumi-ümbrisest juurkaustas. Kui soovite faili üleslaadimine teatud tee, sisestage väljale __sihtkoha__ tee ja valige __üles laadida__.

    ![Faili üleslaadimine dialoogiboks](./media/hdinsight-upload-data/fileupload.png)
    
    Kui fail on lõpule jõudnud, üles, saate selle Hdinsightiga kobar tööd.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Kohalik ketas Paigaldage Azure'i bloobimälu

Vaata [Mount Azure'i bloobimälu nimega kohalik ketas](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

##<a name="services"></a>Teenused

###<a name="azure-data-factory"></a>Azure'i andmed Factory

Azure'i andmed Factory teenuse on täielikult hallatav teenus koostamise andmeteenuste salvestusruumi, andmete töötlemise ja andmete liikumine sujuv, scalable ja usaldusväärsed andmed tootmise torujuhtmete sisse.

Azure'i andmed Factory saab kasutada andmete viimine Azure'i bloobimälu, või luua andmete torustikud otse kasutada Hdinsightiga funktsioone nagu taru ja siga.

Lisateavet leiate [Azure'i andmed Factory dokumentatsiooni](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache Sqoop

Sqoop on mõeldud andmete edastamiseks Hadoopi ja relatsiooniandmebaasid tööriist. Andmete importimiseks nagu SQL Server, MySQL-i või Oracle süsteemi Hadoopi fail (HDFS) MapReduce või taru Hadoopi andmeid muuta, ja seejärel eksportige andmed tagasi mõne RDBMS relatsiooniandmebaasist haldamise süsteemi (RDBMS), saate seda kasutada.

Lisateabe saamiseks lugege teemat [Kasutamine Sqoop koos Hdinsightiga][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Arengu SDK-d

Azure'i bloobimälu pääseb juurde ka mõne programmeerimise järgmistes keeltes: Azure'i SDK abil:

* .NET-I
* Java
* Node.js
* PHP
* Python
* Ruby

Azure'i SDK-d installimise kohta leiate lisateavet teemast [Azure allalaadimine](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Tõrkeotsing

### <a id="storageexception"></a>Salvestusruumi erand kirjutada bloobimälu

__Sümptomid__: kasutamisel on `hadoop` või `hdfs dfs` käsud kirjutamiseks faile, mis on ~ 12 GB või suurem, klõpsake mõnda HBase kobar võivad ilmneda järgmine tõrketeade: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Põhjus__: HBase kohta Hdinsightiga kogumite vaikimisi blokeerimine suurus 256 kb kirjutamisel Azure salvestusruumi. Kuigi see töötab HBase API-d või REST API-d, selle tulemuseks viga kasutamisel on `hadoop` või `hdfs dfs` käsurea Utiliidid.

__Eraldusvõime__: kasutamine `fs.azure.write.request.size` määramiseks Blokeeri suuremaks. Saate teha seda kasutuspõhine põhjal, kasutades funktsiooni `-D` parameeter. Järgmine on näide, kasutades selle parameetri abil soovitud `hadoop` käsk:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Saate ka suurendada väärtus `fs.azure.write.request.size` globaalselt Ambari abil. Ambari Web UI väärtust muuta saab kasutada järgmist:

1. Avage oma brauseris Ambari Web UI klaster jaoks. See on https://CLUSTERNAME.azurehdinsight.net, kus on __CLUSTERNAME__ klaster nime.

    Küsimise korral sisestage klaster administraatori nimi ja parool.

2. Ekraani vasakus servas, valige __HDFS__ja seejärel valige vahekaart __Configs__ .

3. Sisestage väljale __filtreerimine...__ `fs.azure.write.request.size`. See kuvab välja ja praegune väärtus lehe keskel.

4. 262144 alasse (256KB) on uus väärtus väärtust muuta. Näiteks 4194304 (4MB).

![Pilt Ambari Web Kasutajaliidese kaudu väärtuse muutmine](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Ambari kasutamise kohta leiate lisateavet teemast [haldamine Hdinsightiga kogumite Ambari Web Kasutajaliidese kaudu](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui teil mõista, kuidas andmeid tuua Hdinsightiga, lugege järgmisi artikleid, et saate teada, kuidas analüüsida.

* [Alustamine Windows Azure Hdinsightiga][hdinsight-get-started]
* [Edastab Hadoopi tööd programmiliselt][hdinsight-submit-jobs]
* [Hdinsightiga taru kasutamine][hdinsight-use-hive]
* [Kasutage siga Hdinsightiga][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
