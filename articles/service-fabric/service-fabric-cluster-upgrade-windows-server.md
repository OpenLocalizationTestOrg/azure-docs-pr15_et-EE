<properties
   pageTitle="Versioonile Windows Server on eraldi teenuse struktuuri kobar | Microsoft Azure'i"
   description="Versioonitäienduse teenuse struktuuri kood ja/või konfiguratsiooni, mis töötab autonoomse teenuse struktuuri kobar, sh kobar värskendamise režiim seadmine"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Windows Server teenuse struktuuri klaster autonoomse versiooni täiendamine

> [AZURE.SELECTOR]
- [Azure'i kobar](service-fabric-cluster-upgrade.md)
- [Autonoomse kobar](service-fabric-cluster-upgrade-windows-server.md)

Mis tahes tänapäevane süsteemi laiendatavus kavandamine on oluline toote pikaajalise edu saavutamisel. Teenuse struktuuri kobar on ressurss, mille omanik te olete. Selles artiklis kirjeldatakse, kuidas saate olla kindel, et klaster töötab alati toetatud versioonide teenuse struktuuri, koodi ja konfiguratsioone.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Struktuuri versioon, mis töötab klaster juhtimine

Saate seada klaster struktuuri teenusevärskendused alla laadida, kui Microsoft välja uus versioon või valige soovitud klaster olevat toetatud struktuuri versiooni valimine. 

Tehke seda seadmisega "fabricClusterAutoupgradeEnabled" kobar konfiguratsiooni true või false.


>[AZURE.NOTE] Veenduge, et hoida alati versiooniga toetatud teenuse struktuuri klaster. Kui me teatada vabastamist uue versiooni teenuse struktuuri, eelmine versioon on märgitud tugi lõppu vähemalt 60 päeva pärast. Uute plaatide on teada [teenuse struktuuri meeskonna ajaveebist](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Uus versioon on saadaval, siis valida. 


Ainult siis, kui kasutate tootmise laadis sõlm konfiguratsioon, kus iga teenuse struktuuri sõlm on eraldatud eraldi füüsilise või virtuaalse masina, saate uue versiooni täiendada klaster. Kui teil on arengu kobar, kus on rohkem kui üks teenuse struktuuri sõlmed ühe füüsilise või virtuaalse masina, peate Sättiä klaster ja looge selle uue versiooniga.


On kaks erinevat töövoogude klaster Viimane või toetatud struktuuri versiooni täiendamiseks. Kogumite, mis on ühendus automaatselt alla laadida uusima versiooni jaoks ja teine kogumite, mis on teenuse struktuuri uusima versiooni allalaadimiseks ühendust pole jaoks.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Rühmad täiendamine ühenduvuse alla laadida uusima kood ja konfigureerimine 

Järgmiste juhiste abil versiooniks klaster toetatud, kui teie kobar sõlmed on Interneti-ühenduse [http://download.microsoft.com](http://download.microsoft.com) 

Jaoks kogumite, mis on [http://download.microsoft.com](http://download.microsoft.com)Ühenduvus me aeg-ajalt otsida uue teenuse struktuuri versioonide kättesaadavus.


Kui teenus struktuuri uus versioon on saadaval, on paketi alla kohalikult klaster ja ette valmistatud üleminek versioonile. Lisaks teavitada kliendi see uus versioon, süsteemi paigutab konkreetsete kobar hoiatuse on järgmine:

"Kobar versioon [versiooni #], tugi lõpeb [kuupäev].", kui klaster töötab uusim versioon, hoiatus kaob.


#### <a name="cluster-upgrade-workflow"></a>Kobar versioonitäienduse töövoog.
 
Kui näete kobar hoiatuse, peate tegema järgmist:

1. Ühenduse klaster arvutist, millele pääseb masinad, mis on loetletud sõlmed klaster administraator. Arvuti, mis see skript käivitatakse ei pea olema osa klaster

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Saada teenuse struktuuri loendit versioonid, millele saate üle minna

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    saate väljund umbes järgmine:

    ![saada struktuuri versioonid][getfabversions]

3. Võrgukoosolekuga kobar versioonile versioon, mis on saadaval [cmd algus-ServiceFabricClusterUpgrade PowerShelli](https://msdn.microsoft.com/library/mt125872.aspx) abil

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Saate jälgida edenemist järgmise käsu power shelli või teenuse struktuuri Exploreri versioonile

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Kui on fikseeritud tulemuseks tagasipööramine probleeme, peate algatada täiendamise uuesti, enne kui samu juhiseid järgides.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Versioonitäienduse rühmad koos <U>ühendust pole</u> alla laadida uusima kood ja konfigureerimine

Järgmiste juhiste abil versiooniks klaster toetatud, kui teie kobar sõlmed **on** Interneti-ühenduse [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Kui teil on klaster, mis pole ühendatud internet, on teil jälgida selle struktuuri meeskonna ajaveebi teatise saamine kindla uue versiooni. Mis tahes kobar hoiatuse märku sellest süsteemi **ei** paigutada.  

1. Muutke oma kobar konfiguratsioon järgmine atribuudi väärtuseks väär.

        "fabricClusterAutoupgradeEnabled": false,

ja võrgukoosolekuga konfiguratsiooni versioonitäiendus. Vaadake [algus-ServiceFabricClusterUpgrade PS cmd](https://msdn.microsoft.com/library/mt125872.aspx) kasutusteavet. Kobar manifest versioon on versioon, et teil on selle clusterConfig.JSON. Veenduge, et värskendada enne algatada konfiguratsiooni uuele versioonile.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Kobar versioonitäienduse töövoog.
 


1. Paketi uusima versiooni saate alla laadida dokumendi [loomine teenuse struktuuri kobar windows server](service-fabric-cluster-creation-for-windows-server.md) 


1. Ühenduse klaster arvutist, millele pääseb masinad, mis on loetletud sõlmed klaster administraator. Arvuti, mis see skript käivitatakse ei pea olema osa klaster 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Kopeerige allalaaditud pakett kobar pilt pood.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Kopeeritud paketi registreerimine 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Võrgukoosolekuga kobar versioonile versioon, mis on saadaval. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Saate jälgida edenemist järgmise käsu power shelli või teenuse struktuuri Exploreri versioonile

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Kui on fikseeritud tulemuseks tagasipööramine probleeme, peate algatada täiendamise uuesti, enne kui samu juhiseid järgides.



## <a name="next-steps"></a>Järgmised sammud
- Siit saate teada, kuidas kohandada teatud [struktuuri kobar struktuuri Teenusesätted](service-fabric-cluster-fabric-settings.md)
- Siit saate teada, kuidas [mastaapimiseks klaster ja vähendamine](service-fabric-cluster-scale-up-down.md)
- Lisateavet [rakenduse uuendused](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
