<properties 
   pageTitle="Konfigureerimine HBase dispersioonanalüüs vahel kahes andmekeskuses | Microsoft Azure'i" 
   description="Saate teada, kuidas konfigureerida HBase dispersioonanalüüs üle kahe andmekeskuste ja kasutage juhtumite kobar kopeerimise kohta." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>Hdinsightiga HBase geo-dispersioonanalüüs konfigureerimine

> [AZURE.SELECTOR]
- [VPN ühenduvuse konfigureerimine](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS-i konfigureerimine](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigureerimine HBase dispersioonanalüüs](hdinsight-hbase-geo-replication.md) 
 
Saate teada, kuidas konfigureerida HBase dispersioonanalüüs üle kahe andmekeskuste. Mõned kasutavad juhul kobar dispersioonanalüüs sisaldama:

- Varundus- ja Avariijärgne taaste
- Andmete koondamine
- Geograafiliste andmete jaotuse.
- Võrgus andmete manustamisest koos ühenduseta andmeanalüüsi

Kobar dispersioonanalüüs kasutab allikas-push meetod. Mõne HBase kobar saab allikas, sihtkoht, või saate täita mõlemad rollid korraga. Kopeerimine on asünkroonne ja dispersioonanalüüs eesmärk on lõpliku järjepidevuse. Kui allika saab redigeerida veeru pere kordusega dispersioonanalüüs lubatud, selle redigeerimine olevatesse kõik sihtkoha kogumite. Kui andmed on kopeeritud ühe kobar teise, jälgitakse allika kobar ja kõik kogumite, mis on juba tarbitud andmete kopeerimine silmuseid vältimiseks. Lisateabe saamiseks selles õpetuses tuleb konfigureerida allikas sihtkoht kopeerimine.  Muud kobar topoloogiatest, vt [Apache HBase Lühijuhend](http://hbase.apache.org/book.html#_cluster_replication).

See on sarja kolmas osa:

- [Virtuaalse Privaatvõrgu ühendus kahe virtuaalse võrgu vahel konfigureerimine][hdinsight-hbase-replication-vnet]
- [Virtuaalne võrkude DNS-i konfigureerimine][hdinsight-hbase-replication-dns]
- HBase geo dispersioonanalüüs (selles õpetuses) konfigureerimine

Järgmine diagramm näitab kahe virtuaalse võrgu ja võrguühendus, mis on loodud [konfigureerimine virtuaalse Privaatvõrgu ühendus kahe virtuaalse võrgu vahel] [ hdinsight-hbase-geo-replication-vnet] ja [Konfigureerida DNS-i virtuaalse võrgu][hdinsight-hbase-replication-dns]: 

![Hdinsightiga HBase dispersioonanalüüs virtuaalse võrgustikudiagramm][img-vnet-diagram]

## <a id="prerequisites"></a>Eeltingimused

Enne alustamist selle õpetuse, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Töökoha Azure PowerShelli abil**.

    PowerShelli skriptide käivitamiseks peate Azure PowerShelli Käivita administraatorina ja *RemoteSigned*täitmise poliitika määramine. Vt cmdlet-käsu Set-ExecutionPolicy abil.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Kahe Azure virtuaalse võrgu virtuaalse Privaatvõrgu ühendus ja DNS-i, mis on konfigureeritud**.  Juhised leiate teemast [konfigureerimine VPN-ühendus kahe Azure virtuaalse võrgu vahel][hdinsight-hbase-replication-vnet], ja [Konfigureerida DNS-i kahe Azure virtuaalse võrgu vahel][hdinsight-hbase-replication-dns].


    Enne käivitamist PowerShelli skriptide, veenduge, et olete loonud ühenduse Azure tellimuse abil järgmine cmdlet-käsk:

        Add-AzureAccount

    Kui teil on mitu Azure tellimust, abil järgmine cmdlet-käsk praeguse tellimuse:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>Sätte HBase kogumite sisse Hdinsightiga

Konfigureerimine [VPN-ühendus kahe Azure virtuaalse võrgu vahel][hdinsight-hbase-replication-vnet], olete loonud virtuaalse võrgu Euroopa Liidus andmekeskuse ja virtuaalse võrgu USA andmekeskuse. Kahe virtuaalse võrk on ühendatud VPN-i kaudu. Selle seansi on mõni HBase kobar iga virtuaalne võrkude ette valmistada. Allpool olevat selles õpetuses te teete ühte HBase rühmad muude HBase kobar kopeerida.

Azure'i klassikaline portaal ei toeta ettevalmistamise Hdinsightiga kogumite kohandatud konfiguratsiooni võimalusi. Näiteks seada *hbase.replication* väärtuseks *true*. Kui seate konfiguratsioonifailis pärast klaster on ette valmistatud, lähevad pärast klaster reimaged säte. Lisateabe saamiseks vaadake [Hdinsightiga sisse säte Hadoopi kogumite][hdinsight-provision]. Üks suvanditest ettevalmistamise Hdinsightiga kobar koos kohandatud suvandid on Azure PowerShelli kaudu.


**Ettevalmistamise Contoso-VNet-Liidus on HBase kobar** 

1. Avage oma töökoha Windows PowerShell ISE.
2. Määrake muutujate skripti alguses ja seejärel käivitage skript.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Ettevalmistamise Contoso-VNet-USA-s on HBase kobar** 

- Sama skripti abil järgmised väärtused:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Kuna Azure'i konto on juba ühendatud, te ei pea käivitage järgmised comlets enam:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>DNS-i tingimusvormingu ekspediitor konfigureerimine

[Konfigureerimine]DNS-i virtuaalse võrkude[hdinsight-hbase-replication-dns], olete konfigureerinud DNS-serverid kahe võrgu jaoks. HBase rühmad on erinevad domeeni sufiksid. Seega peate konfigureerida täiendavaid DNS-i tingimusvormingu edasisuunamislahendusi.

Konfigureerimiseks tingimusvormingu ekspediitor, peate domeeni sufiksid kaks HBase rühmad, leida. 

**Leida domeeni sufiksid kaks HBase rühmad.**

1. RDP **Contoso-HBase -**Liitu.  Juhised leiate teemast [haldamine Hadoopi kogumite Hdinsightiga, kasutades Azure klassikaline portaali sisse][hdinsight-manage-portal]. See on tegelikult headnode0, klaster.
2. Avage Windows PowerShelli konsooli või Käsuviip.
3. Käivitage **ipconfig**ja kirjutage **kindla ühenduse DNS-i järelliite**.
4. Ärge sulgege RDP-seansi.  Peate selle hiljem, et testida domeeninimede lahendamine.
5. Korrake samu juhiseid teada **kindla ühenduse DNS-i järelliite** **Contoso-HBase-**USA-s.


**DNS-i edasisuunamislahendusi konfigureerimine**
 
1.  RDP **Contoso-DNS -**Liitu. 
2.  Klõpsake alumises vasakus Windowsi klahv.
2.  Klõpsake **Haldustööriistad**.
3.  Klõpsake **DNS-i**.
4.  Laiendage vasakpoolsel paanil **DSN-i**, **Contoso-DNS-EL**.
5.  Paremklõpsake **Tingimusvormingu Edasisuunamislahendusi**ja seejärel klõpsake nuppu **Uus tingimusvormingu ekspediitor**. 
5.  Sisestage järgmine teave:
    - **DNS-i Domeen**: sisestage DNS-i järelliite Contoso HBase USA. Näide: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **IP-aadresside juhtslaidi serverid**: sisestage 10.2.0.4, mis on Contoso-DNS-USA IP-aadress. Kontrollige IP. Oma DNS-i server võib olla erinev IP-aadress.
6.  Vajutage sisestusklahvi **ENTER**, ja seejärel klõpsake nuppu **OK**.  Nüüd on teil võimalik Contoso-DNS-EL Contoso-DNS-USA IP-aadressi lahendamiseks.
7.  Korrake juhiseid lisada tingimusvormingu ekspediitor DNS-i DNS-i teenuse Contoso ja DNS-USA virtual arvutisse koos järgmised väärtused:
    - **DNS-i Domeen**: sisestage DNS-i järelliite Contoso HBase EL. 
    - **IP-aadresside juhtslaidi serverid**: sisestage 10.2.0.4, mis on Contoso-DNS-Euroopa Liidu IP-aadress.

**Testimiseks domeeninimede lahendamine**

1. Contoso-HBase-EL RDP akna aktiveerimine.
2. Avage käsuviip.
3. Käivitage ping-käsk:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    ICMI Protocol (protokoll) on sisse lülitatud töötaja sõlmed HBase rühmad.

4. Ärge sulgege RDP-seansi. See on vaja siiski hiljem sisse õpetuse.
5. Korrake samu juhiseid ping Contoso-HBase EL Contoso HBase USA headnode0.

>[AZURE.IMPORTANT] DNS-i peavad tegema enne jätkamist järgige järgmisi juhiseid.

## <a name="enable-replication-between-hbase-tables"></a>Lubada kopeerimine HBase tabelite vahel

Nüüd saate luua valimi HBase tabeli, lubada kopeerimine ja testige mõned andmetega. Kasutage näidistabeli on kaks veergu peredele: Personal ja Office. 

Selles õpetuses teete Europe HBase kobar kobar allikas ja USA HBase kobar sihtkoha klaster nimega.

Luua HBase tabelid sama nimed ja nii lähte-kui ka kogumite, klõpsake veeru peredele, et sihtkoha kobar teab, kus ta saab andmete talletamiseks. HBase shell kasutamise kohta leiate lisateavet teemast [kasutamise alustamine rakenduses Hdinsightiga Apache HBase][hdinsight-hbase-get-started].

**Klõpsake Contoso-HBase-Euroopa Liidu HBase tabeli loomiseks**

1. **Contoso-HBase-EL** RDP akna aktiveerimine.
2. Töölaual, klõpsake **Hadoopi käsurea**.
2. HBase home kataloog kausta muutmiseks tehke järgmist.

        cd %HBASE_HOME%\bin
3. Avage HBase shell.

        hbase shell
4. HBase tabeli loomiseks:

        create 'Contacts', 'Personal', 'Office'
5. Ärge sulgege kas RDP-seansi ega Hadoopi käsurea akna. Need on vaja siiski hiljem sisse õpetuse.
    
**Contoso-HBase-USA HBase tabeli loomiseks**

- Korrake samu juhiseid Contoso-HBase-USA sama tabeli loomiseks.


**Contoso-HBase-USA dispersioonanalüüs omavahelistes lisamiseks**

1. **Contso-HBase_EU** RDP akna aktiveerimine.
2. Aknas HBase shell lisada sihtkoha kobar (Contoso-HBase-US) mõne partneri, näiteks:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    Valimis, domeeni nime järelliide pole *contoso-hbase-us.d4.internal.cloudapp.net*. Peate värskendage seda, et need vastaksid teie domeeni järelliide meile HBase kobar. Veenduge, et selle hostinimed vahel on tühikuid.

**Iga veeru pere allika kobar järgitav konfigureerimine**

1. Aknas HBase shell **Contso-HBase-EL** RDP-seansi konfigureerida iga veeru pere järgitav.

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Laadi andmed tabelisse HBase hulgi**

Näidisandmete fail on üles laaditud avaliku Azure'i bloobimälu container järgmine URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

Faili sisu:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

Saate laadida sama andmefaili HBase klaster ja sealt andmed importida.

1. **Contoso-HBase-EL** RDP akna aktiveerimine.
2. Töölaual, klõpsake **Hadoopi käsurea**.
3. Kausta HBase home directory muutmiseks järgmist.

        cd %HBASE_HOME%\bin

4. andmete üleslaadimiseks tehke järgmist.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Veenduge, et andmete kopeerimine toimub

Saate kontrollida selle kopeerimine toimub skannimine nii kogumite HBase shell järgmised käsud: tabelite abil.

        Scan 'Contacts'


## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses te teada, kuidas konfigureerida HBase dispersioonanalüüs üle kahe andmekeskuste. Hdinsightiga ja HBase kohta leiate lisateavet järgmistest teemadest.

- [Hdinsightiga teenus](https://azure.microsoft.com/services/hdinsight/)
- [Hdinsightiga dokumentatsioon](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Alustamine Apache HBase sisse Hdinsightiga][hdinsight-hbase-get-started]
- [Hdinsightiga HBase ülevaade][hdinsight-hbase-overview]
- [Sätte HBase kogumite Azure virtuaalse võrgus][hdinsight-hbase-provision-vnet]
- [Reaalajas Twitteri meeleolu HBase koos analüüsida.][hdinsight-hbase-twitter-sentiment]
- [Torm ja HBase sisse Hdinsightiga (Hadoopi) andur andmete analüüsimine][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
