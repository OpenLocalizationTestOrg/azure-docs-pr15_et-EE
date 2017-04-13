<properties
pageTitle="Andmete kaudu ühiskasutusse antud juurdepääs allkirjade Hdinsightiga juurdepääsu piiramiseks"
description="Saate teada, kuidas kasutada ühiskasutusse Accessi andmed salvestatakse Azure storage plekid Hdinsightiga juurdepääsu piiramiseks."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Azure'i salvestusruumi ühiskasutusse Accessi allkirjade juurdepääsu piiramiseks Hdinsightiga andmete kasutamine

Hdinsightiga kasutab Azure storage plekid andmesalv. Ajal Hdinsightiga peab olema täielik juurdepääs bloobimälu, kasutada vaikimisi salvestusruumi klaster, saate piirata muude plekid kasutatavaid klaster talletatavad andmed õigused. Näiteks võite teha mõned andmed kirjutuskaitstud. Saate seda teha, kasutades ühiskasutusse Accessi allkirjad.

Ühiskasutusega juurdepääsu allkirjad (SAS) on Azure storage kontode funktsioon, mis võimaldab andmetele juurdepääsu piirata. Näiteks esitada kirjutuskaitstud juurdepääs andmetele. Selles dokumendis saate teada, kuidas lubada lugemis- ja loendi ainult bloobimälu container hdinsightist SAS abil.

##<a name="requirements"></a>Nõuded

* Azure'i tellimuse

* C# või Python. C# koodi näide on esitatud lahenduse Visual Studios.

    * Visual Studio peab olema versiooni 2013 või 2015
    
    * Python peab olema 2.7 või uuem versioon

* Linux-põhine Hdinsightiga klaster OR [Azure PowerShelli] [ powershell] – kui teil on olemasolevaid Linuxi-põhiste kobar, saate ühiskasutusse antud juurdepääs allkirja lisamiseks klaster Ambari. Kui ei, saate luua uue kobar ja kobar loomise ajal ühiskasutusse antud juurdepääs signatuuri lisamine Azure PowerShelli.

* Näide: [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature)failid. See hoidla on järgmine:

    * Saate luua salvestusruumi container, salvestatud poliitika, ja SAS Hdinsightiga kasutamiseks Visual Studio projekt
    
    * Python skripti, mis saate luua salvestusruumi container, salvestatud poliitika, ja SAS Hdinsightiga kasutamiseks
    
    * PowerShelli skripti, et saate luua uue Hdinsightiga kobar ja konfigureerida nii, et kasutada muude.

##<a name="shared-access-signatures"></a>Ühiskasutusega juurdepääsu allkirjad

On kahte tüüpi ühiskasutusse Accessi allkirjad.

* Sihtotstarbeline: algusaeg, aeg ja muude õiguste on kõik määratud SAS URI klõpsake (või ka mitte kaudset, juhul kui algusaeg puudub).

* Salvestatud Accessi poliitika: Salvestatud juurdepääsu poliitika on määratletud ressursi container - bloobimälu container, tabeli, järjekorda või faili ühiskasutusse andmine - ja saab hallata piiranguid ühe või mitme ühiskasutusega juurdepääsu allkirjade. Kui soovitud SAS seostada salvestatud Accessi poliitika muude pärib piiranguid - algusaeg, aeg ja õiguste - salvestatud Accessi poliitika jaoks määratletud.

Kahe vormide vahe on oluline üks klahv stsenaarium: tühistamine. Mõne SAS on URL-i, keegi, kes saab muude saate, olenemata sellest, kes seda nõutav alustada. Kui soovitud SAS on avaldatud avalikult, saate seda kasutada igaüks maailmas. SAS, mis on jaotatud kehtib seni, kuni juhtub neli võimalust:

1. MUUDE määratud kehtivuse aeg on jõudnud.

2. Salvestatud Accessi poliitika viidatud muude aegumise aja jõutakse (kui viidatud salvestatud juurdepääsu poliitika ja kui seda määrab on aeg). See võib juhtuda kas, sest intervalli kaitseaega või olete muutnud salvestatud Accessi poliitika on varem, mis on üks viis, kuidas tühistada muude aegumise aega.

3. Viidatud muude salvestatud Accessi poliitika on kustutatud, mis on teine võimalus muude tühistada. Pange tähele, et kui uuesti luua Accessi salvestatud poliitika täpselt sama nimega, kõik olemasolevad SAS sõned uuesti kehtib vastavalt määratud õigusi (eeldades, et muude kehtivuse aeg on möödas) salvestatud Accessi poliitika. Kui kavatsete tühistada muude kindlasti kasutage mõnda muud nime, kui juurdepääsu poliitika kehtivuse aeg edaspidi uuesti luua.

4. MUUDE loomiseks kasutatud kontovõti on uuesti. Pange tähele, et see põhjustab kõik komponendid abil selle konto võti nurjumise autentida seni, kuni ta on värskendatud kasutada muu sisestage kehtiv konto- või äsja regenereeritud konto võti.

> [AZURE.IMPORTANT] Ühiskasutusega juurdepääsu signatuuri URI on seostatud allkirja loomiseks kasutatud konto võti ja seotud talletatud juurdepääsu poliitika (vajaduse korral). Kui pole salvestatud Accessi poliitikat pole määratud, on ainus võimalus tühistada ühiskasutusega juurdepääsu signatuuri muutmiseks konto võti. 

On soovitatav alati kasutada salvestatud juurdepääsupoliitikaid, nii et saate tühistada allkirjad või pikendada vastavalt vajadusele. Selle dokumendi Kasuta salvestatud juurdepääsupoliitikaid SAS loomiseks.

Ühiskasutusse antud juurdepääs allkirjade kohta leiate lisateavet teemast [SAS andmemudeli mõistmine](../storage/storage-dotnet-shared-access-signature-part-1.md).

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Salvestatud poliitika loomine ja lisamine SAS luua

Praegu peab salvestatud poliitika loomine programmiliselt. Siit leiate nii C# ka Python näide loomise [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature)salvestatud poliitika ja SAS.

###<a name="create-a-stored-policy-and-sas-using-c"></a>Salvestatud poliitika ja C abil SAS loomine\#

1. Avage lahenduse Visual Studios.

2. Solution Exploreris __SASToken__ projekti paremklõpsake ja valige __Atribuudid__.

3. Valige __sätted__ ja lisage järgmised kirjed väärtused:

    * StorageConnectionString: Ühendusstringi jaoks salvestusruumi konto, mille soovite luua salvestatud poliitika ja SAS jaoks. Vorm peaks olema `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` kus `myaccount` on teie salvestusruumi konto nimi ja `mykey` on salvestusruumi konto võti.
    
    * Ümbrisenimi: Container salvestusruumi konto, mille soovite juurdepääsu piirata.
    
    * SASPolicyName: Nimi, mida kasutada salvestatud poliitika, mis luuakse.
    
    * FileToUpload:-Ümbrisest üleslaaditud faili tee.
    
4. Käivitage projekt. Konsooli aken ja järgmine teave kuvatakse pärast muude on loodud.

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Salvestage SAS poliitika luba, peate selle kui klaster HDInsight konto salvestusruumi seostada. Te peate salvestusruumikonto nimi ja container nimi.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Salvestatud poliitika ja kasutades Python SAS loomine

1. SASToken.py faili avada ja muuta järgmised väärtused:

    * poliitika\_nimi: kasutada salvestatud poliitika, mis luuakse nimi.
    
    * salvestusruumi\_konto\_nimi: teie salvestusruumi konto nimi.
    
    * salvestusruumi\_konto\_võti: salvestusruumi konto võti.
    
    * salvestusruumi\_container\_nimi: container salvestusruumi konto, mida soovite juurdepääsu piirata.
    
    * Näide\_faili\_tee:-ümbrisest üleslaaditud faili tee

2. Käivitage skript. Kui script on lõpule jõudnud SAS luba sarnaneb järgmisega kuvatakse:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Salvestage SAS poliitika luba, peate selle kui klaster HDInsight konto salvestusruumi seostada. Te peate salvestusruumikonto nimi ja container nimi.
    
##<a name="use-the-sas-with-hdinsight"></a>MUUDE Hdinsightiga kasutamine

Mõne Hdinsightiga kobar loomisel peate määrama esmased konto ja soovi korral saate määrata täiendavat salvestusruumi kontod. Mõlemad meetodid lisamise salvestusruumi vaja täielik juurdepääs salvestusruumi kontod ja ümbriste, mida kasutatakse.

Ühiskasutusse antud juurdepääs signatuuri ümbris juurdepääsu piiramiseks kasutamiseks peate lisama klaster konfiguratsiooni __core – saidile__ kohandatud kirje.

* __Windowsi-põhiste__ või __Linuxi-põhiste__ Hdinsightiga kogumite, saate teha seda PowerShelli kaudu kobar loomise ajal.

* Jaoks __Linux-põhine__ Hdinsightiga kogumite, kobar loomine abil Ambari pärast konfiguratsiooni muutmine.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Looge uus klaster, mis kasutab muude

Näiteks luua Hdinsightiga kobar, mida kasutatakse muude sisaldab selle `CreateCluster` hoidla kataloogi. Selle kasutamiseks tehke järgmist:

1. Avage soovitud `CreateCluster\HDInsightSAS.ps1` fail tekstiredaktoris ja muuta dokumendi alguses järgmised väärtused.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Näiteks muuta `'mycluster'` nimi kobar, mida luua soovite. SAS väärtused peaksid olema samad väärtused eelmisi juhiseid salvestusruumi konto ja SAS luba loomisel.
    
    Kui olete muutnud väärtusi, salvestage fail.

1. Avage uus Azure PowerShelli küsimus. Kui olete võõrad Azure PowerShelli või olete installinud, vt [installida ja konfigureerida Azure PowerShelli][powershell].

2. Kuvatakse vastav viip, kasutage autentimiseks Azure tellimuse järgmist:

        Login-AzureRmAccount
    
    Vastava viiba kuvamisel Azure tellimuse kontoga sisselogimine.
    
    Kui teie sisselogimise on seotud mitu Azure tellimust, peate kasutama `Select-AzureRmSubscription` valige tellimus, mida soovite kasutada.

2. Kataloogi, kuvatakse vastav viip, muuta selle `CreateCluster` HDInsightSAS.ps1 faili sisaldava kausta. Käivitage skript järgmine abil
        
        .\HDInsightSAS.ps1
    
    Nagu skript käivitub, kuvatakse see logige väljundi PowerShelli käsuviibas loob ressursi rühma ja salvestusruumi kontod. Seejärel palutakse teil sisestada kasutaja HTTP Hdinsightiga kobar. See on kasutajakonto, mida kasutatakse HTTP/s klaster juurdepääsu tagamiseks.
    
    Kui loote Linux-põhine kobar, ka küsitakse SSH konto kasutajanimi ja parool. Seda kasutatakse kaugühenduse teel klaster Logi sisse.
    
    > [AZURE.IMPORTANT] HTTP/s või SSH kasutajanime ja parooli küsimise korral peate sisestama parooli, mis vastab järgmistele tingimustele:
    >
    > - Peab olema vähemalt 10 märki
    > - Peab sisaldama vähemalt ühe numbrikoha
    > - Peab sisaldama vähemalt ühte tähtedest ja numbritest koosnev märki
    > - Peab sisaldama vähemalt ühte ülemise või tähistav


Selle võtab aega skripti lõpule viia, tavaliselt umbes 15 minutit. Kui skripti ilma vigu on lõpule jõudnud, on loodud klaster.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Mõne olemasoleva kobar kasutada muude värskendamine

Kui teil on olemasolevaid Linuxi-põhiste kobar, saate lisada muude __core – saidile__ konfiguratsiooni, kasutades järgmist:

1. Avage Ambari veebist UI klaster. Selle lehe aadress on https://YOURCLUSTERNAME.azurehdinsight.net. Vastava viiba kuvamisel autentida klaster administraatori nimi (haldus) ja parool, mida kasutasite klaster loomisel.

2. Vasakus servas Ambari web UI, valige __HDFS__ ja seejärel valige vahekaart __Configs__ lehe keskel.

3. Valige vahekaart __Täpsemalt__ ja seejärel kerige, kuni leiate jaotises __kohandatud core – saidile__ .

4. Laiendage jaotises __kohandatud core – saidile,__ seejärel leidke end ja valige __Lisa atribuut...__ link. Kasutamiseks __võti__ ja __väärtuse__ väljadele järgmised väärtused:

    * __Võti__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Väärtus__: The SAS tagastatud käivitasite varem C# või Python rakendus
    
    Asendage __ÜMBRISENIMI__ container kasutasite C# või SAS rakenduse nimi. Asendage __STORAGEACCOUNTNAME__ kasutatud salvestusruumi konto nimi.

5. Klõpsake nuppu __Lisa__ see võti ja väärtuse salvestamiseks ja seejärel konfiguratsiooni muudatuste salvestamiseks nuppu __Salvesta__ . Vastava viiba kuvamisel lisada kirjelduse muutmine ("lisades SAS juurdepääs" näiteks) ja klõpsake siis nuppu __Salvesta__.

    Kui soovitud muudatused on lõpule viidud, klõpsake nuppu __OK__ .

    > [AZURE.IMPORTANT] See salvestab konfiguratsiooni muudatusi, kuid peate mitu teenuste enne jõustub Muuda.

6. Ambari web UI __HDFS__ loendist vasakul ja valige seejärel __Taaskäivitage kõik__ __Rakendused__ ripploendit paremal. Vastava viiba kuvamisel valige __hooldustööd režiimi__ ja seejärel valige __Conform taaskäivitage kõik".

    Korrake seda toimingut MapReduce2 ja LÕNG kirjete loendist klõpsake lehe vasakus servas.

7. Kui need on taaskäivitada, valige iga üks ja keelake hooldustööd režiimi __Rakenduste kinnitamine__ rippmenüü.

##<a name="test-restricted-access"></a>Juurdepääsupiirangute testimine

Veenduge, et teil on piiratud juurdepääs, kasutage järgmist:

* __Windowsi-põhiste__ Hdinsightiga kogumite, kasutada kaugtöölaua ühenduse klaster. Lisateabe saamiseks vaadake [Connecto abil RDP Hdinsightiga abil](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) .

    Kui ühendus on loodud, kasutage __Hadoopi käsurea__ ikooni töölaual Käsuviip avamiseks.

* __Linux-põhine__ Hdinsightiga kogumite, kasutada SSH ühenduse klaster. Linux-põhine kogumite SSH kasutamise kohta leiate järgmist teavet.

    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, OS X ja Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Kui ühendus on loodud klaster, järgige järgmisi juhiseid kinnitamaks, et saate teha ainult lugemis- ja loendi üksuste SAS salvestusruumi konto:

1. Vastava viiba kuvamisel tippige järgmist. Asendage __SASCONTAINER__ ümbris loodud SAS salvestusruumi konto nimi. Salvestusruumi konto, mida kasutatakse muude nimi __SASACCOUNTNAME__ asendamiseks tehke järgmist.

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    See kuvab ümbris, mis peaks sisaldama faili, mis on üles laaditud sisu container ja SAS loomise.
    
2. Veenduge, et saate lugeda faili sisu kasutada järgmist. Asendage __SASCONTAINER__ ja __SASACCOUNTNAME__ , nagu eelmises etapis. Eelmise käsu kuvatakse faili nimi __faili nimi__ asendamiseks tehke järgmist.

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    See kuvab soovitud faili sisu.
    
3. Faili allalaadimiseks kohaliku failisüsteemi kasutage järgmist:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    See on kohaliku faili nimega __testfile.txt__faili alla laadida.

4. Uus fail nimega __testupload.txt__ SAS Storage kohaliku faili üleslaadimine kasutage järgmist:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Kuvatakse järgmine teade:
    
        put: java.io.IOException
        
    See tõrge ilmneb talletuskoht on loetud + ainult loendi. Andmed sellele vaikimisi salvestusruumi kobar, mis on kirjutatav jaoks kasutage järgmist:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Sel ajal, tuleks toiming lõpule viia.
    
##<a name="troubleshooting"></a>Tõrkeotsing

###<a name="a-task-was-canceled"></a>Tööülesande on tühistatud.

__Sümptomid__: luua klaster PowerShelli skripti kaudu, võidakse kuvada järgmine tõrketeade:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Põhjus__: See tõrge võib ilmneda juhul, kui kasutate klaster või (Linux-põhine kogumite,) administraator/http kaudu kasutaja parooli SSH kasutaja.

__Eraldusvõime__: kasutage parooli, mis vastab järgmistele tingimustele:

- Peab olema vähemalt 10 märki
- Peab sisaldama vähemalt ühe numbrikoha
- Peab sisaldama vähemalt ühte tähtedest ja numbritest koosnev märki
- Peab sisaldama vähemalt ühte ülemise või tähistav

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete piiratud juurdepääsuga salvestusruumi lisamine klaster Hdinsightiga õppinud, lugege muul viisil klaster andmetega töötamiseks:

* [Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)

* [Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
