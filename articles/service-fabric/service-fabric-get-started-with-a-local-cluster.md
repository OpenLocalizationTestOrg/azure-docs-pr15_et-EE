<properties
   pageTitle="Alustamine juurutamine ja täiendamine rakenduste kohaliku klaster | Microsoft Azure'i"
   description="Saate seadistada kohalikku teenuse struktuuri kobar, taotluse selle juurutada ja seejärel selle rakenduse täiendamine."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Juurutamine ja üleminekut kohalik klaster rakenduse kasutamise alustamine
Azure'i teenus struktuuri SDK sisaldab täis kohalike arenduskeskkond, mille abil saate kiiresti alustada juurutamine ja kohalik klaster rakenduste haldamine. Selles artiklis saate luua kohaliku kobar, olemasoleva rakenduse juurutamine ja värskendada rakenduse uus versioon, kõik Windows PowerShelli kaudu.

> [AZURE.NOTE] Selles artiklis eeldatakse, et te juba [häälestamine oma arenduskeskkond](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Looge kohalik kobar
Teenuse struktuuri kobar tähistab kogumi riistvara ressursse, mida saab kasutada rakendusi. Tavaliselt klaster koosneb suvalist viielt mitu tuhat masinad. Siiski sisaldab teenuse struktuuri SDK kobar konfiguratsiooni, mida saab käitada ühte arvutisse.

See on oluline mõista teenuse struktuuri kohaliku kobar pole emulaator või simulaatorit. See töötab sama platvormi koodi, mida mitme masina kogumite leitud. Ainus erinevus on see, et see töötab platvormi protsessid, mis on tavaliselt viis arvutites ühes arvutis.

SDK pakub seadistada kohalikku kobar kaks võimalust: Windows PowerShelli skripti ja kohalik halduri system tray rakendus. Selles õpetuses kasutame PowerShelli skripti.

> [AZURE.NOTE] Kui olete juba loonud kohaliku kobar kasutades rakenduse Visual Studio, võite selle jaotise vahele jätta.


1. Käivitage PowerShelli uus aken administraatorina.

2. Käivitage kobar häälestamise skripti SDK kaust:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Kobar häälestus võtab mõne hetke aega. Kui häälestamine on lõpule jõudnud, peaksite nägema umbes väljundit:

    ![Kobar häälestamise väljund][cluster-setup-success]

    Nüüd olete valmis proovima klaster rakenduse juurutamine.

## <a name="deploy-an-application"></a>Rakenduse juurutamine
Teenuse struktuuri SDK sisaldab suurel hulgal raamistiku ja arendaja tööriistasid rakenduse loomiseks. Kui olete huvitatud õppida, kuidas luua rakendusi Visual Studios, lugege teemat [loomine esimese teenuse struktuuri rakenduse Visual Studios](service-fabric-create-your-first-application-in-visual-studio.md).

Selles õpetuses kasutame valimi taotluse (ehk WordCount) nii, et saame keskenduda halduse aspektid platvormi--juurutus, jälgimine ja täiendamine.


1. Käivitage PowerShelli uus aken administraatorina.

2. Teenuse struktuuri SDK PowerShelli mooduli importida.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Looge kaust rakendus, mida saate alla laadida ja juurutada, nt C:\ServiceFabric talletamiseks.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [Laadige rakendus WordCount](http://aka.ms/servicefabric-wordcountapp) asukohta loodud.  Märkus: Microsoft Edge'i brauseri salvestab *ZIP* -laiendiga faili.  Saate muuta faililaiendit *.sfpkg*.

5. Ühenduse loomine kohaliku kobar:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Loo uus rakendus – SDK juurutamise käsuga rakenduse pakett tee ja nimi.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Kui kõik hästi läheb, peaksite nägema väljundi umbes selline:

    ![Kohaliku klaster rakenduse juurutamine][deploy-app-to-local-cluster]

7. Toimingu rakenduse vaatamiseks brauseri käivitamine ja liikuge [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Näha peaks olema:

    ![Juurutatud rakendusega kasutajaliides][deployed-app-ui]

    WordCount rakendus on väga lihtne. See sisaldab kliendipoolne JavaScripti koodi loomiseks juhusliku 5-kohaline "sõnad", mis on seejärel edastatakse rakenduse kaudu ASP.net-i veebi-API. Stateful teenuse jälitab arvestatud sõnade arvu. Need on liigendatud põhjal sõna esimene märk. Lähtekoodi leiate WordCount rakenduse [Alustamine näidised](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/).

    Rakenduse meil juurutatud sisaldab neli sektsiooni. Nii, et sõnad, mis algavad A – G on talletatud esimese sektsiooni, sõnadega algab H kuni N on salvestatud teise sektsiooni jne.

## <a name="view-application-details-and-status"></a>Rakenduse üksikasjade kuvamine ja olek
Nüüd kus oleme juurutanud rakendus, Vaatame mõned rakenduse PowerShellis üksikasjad.

1. Päringu kõik juurutatud rakenduste klaster:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Eeldades, et olete ainult juurutanud WordCount rakenduse, kuvatakse midagi sarnast.

    ![Päringu PowerShellis juurutatud kõik rakendused][ps-getsfapp]

2. Avage uuele tasemele päringute WordCount rakenduse kaasatud teenuste määramine.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Loendi teenuste rakenduse PowerShellis][ps-getsfsvc]

    Rakendus koosneb kahe teenuse--veebi ja stateful teenus, mida haldab sõnad.

3. Lõpuks Heitke pilk sektsioonide loend WordCountService jaoks:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![PowerShelli teenuse sektsiooni kuvamine][ps-getsfpartitions]

    Käskude, mida kasutasite kõik teenuse struktuuri PowerShelli käske, nagu on mis tahes kobar, mida võib ühendada, kohaliku või kaugandmebaasiga.

    Rohkem visuaalne viis klaster suhelda, võite kasutada veebipõhine teenuse struktuuri Exploreri tööriista navigeerides [http://localhost:19080/Explorer](http://localhost:19080/Explorer) brauseris.

    ![Teenuse struktuuri Exploreri rakenduse üksikasjade kuvamine][sfx-service-overview]

    > [AZURE.NOTE] Teenuse struktuuri Exploreri kohta leiate lisateavet teemast [visualiseerimine klaster teenuse struktuuri Exploreriga](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Rakenduse täiendamine
Teenuse struktuuri pakub rakenduse seisundi jälgimine, nagu see koondab üle klaster ei-tööseisakute uuendamine. Vaatame teha lihtsa versiooniuuenduse WordCount rakenduse.

Uue versiooni kohe rakenduse loendab ainult sõnu, mis algavad vokaali. Kui täiendamise koondab, näeme kaks muudatuste rakenduse käitumine. Esmalt, määr, kus kasvab arvu peaks aeglane, kuna vähem sõnu loendatakse. Teiseks, kuna esimese sektsiooni on vokaalid (A ja E) ja kõik muud sektsioonid sisaldavad ainult ühte iga, selle count tuleks lõpuks hakata edestama teisi.

1. [Laadige WordCount v2](http://aka.ms/servicefabric-wordcountappv2) samasse asukohta, kus v1 alla laadisite.

2. Naaske oma PowerShelli aken ja – SDK versioonitäienduse käsu abil registreerida uus versioon klaster. Alustage üleminekut struktuuri: / WordCount rakendus.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Peaksite nägema väljundit PowerShellis, mis näeb välja järgmine täiendamise algab.

    ![Edenemise PowerShellis täiendamine][ps-appupgradeprogress]

3. Versiooniuuenduse ajal asumist teil võib-olla lihtsam jälgida olekut teenuse struktuuri Explorerist. Käivitage brauseriaknas ja liikuge [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Laiendage jaotis **rakenduste** puus vasakule, seejärel valige **WordCount**, ja lõpuks **struktuuri: / WordCount**. Menüü Essentialsi kuvatakse versioonitäienduse oleku edenedes soovitud klaster täiendamine domeenide kaudu.

    ![Edenemise teenuse struktuuri Exploreri versiooni täiendamine][sfx-upgradeprogress]

    Uuele versioonile edenedes iga domeeni kaudu tehakse seisundikontrollid tagamaks, et rakendus käitub õigesti.

4. Kui te uuesti varasema päringu jaoks teenused struktuuri: / WordCount rakenduse teade, et versiooni WordCountService muutunud, kuid WordCountWebService versioon ei olnud:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Päringu rakendusteenuste pärast uuele versioonile üleminekut][ps-getsfsvc-postupgrade]

    See näitab, kuidas teenuse struktuuri haldab rakenduse uuendused. Puudutab ainult määramine teenuseid (või koodi/konfigureerimise pakettide nende teenuste sees) mis on muutunud, mis muudab protsess täiendamise kiiremini ja usaldusväärne.

5. Lõpuks naaske brauseri jälgida käitumist rakenduse uus versioon. Oodatud viisil, arvu edenedes aeglasemalt ja esimese sektsiooni jõuab veidi rohkem maht.

    ![Uue rakenduse versiooni vaatamine brauseris][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Puhastamine

Enne lõpetamine, on oluline meeles pidada, et kohalik klaster on reaal. Rakenduste jätkatakse taustal kuni eemaldada.  Sõltuvalt teie rakendused, töötava rakenduse võivad katta märgatavat ressursid teie arvutis. Teil on mitu võimalust, rakenduste ja klaster juhtida.

1. Üksikute taotlus ja kõik andmed, käivitage järgmine eemaldamiseks tehke järgmist.

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Või teenuse struktuuri Exploreri menüü **toimingud** või kontekstimenüü vasakpoolsel paanil loendivaates rakenduse rakenduse kustutamine.

    ![Kustuta rakendus on teenuse struktuuri Explorer][sfe-delete-application]

2. Pärast kustutada rakenduse klaster, saate unregister 1.0.0 ja 2.0.0 tüüpi WordCount rakenduste versioonid. Kustutamise eemaldab rakenduse paketid, sh koodi ja konfigureerimise poest soovitud klaster pilt.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Või teenuse struktuuri Exploreris valige **Ettevalmistamise tüüp** rakenduse.

3. Klõpsake klaster sulgeda, kuid jätate rakenduse andmete ja jälgi **Peatamine kohaliku kobar** system tray rakenduses.

4. Klaster täiesti kustutamiseks klõpsake nuppu **Eemalda kohaliku kobar** system tray rakenduses. See suvand annab tulemuseks teise aeglane juurutamise järgmine kord, kui vajutate klahvi F5 Visual Studios. Eemaldage kohaliku kobar ainult juhul, kui te ei kavatse seda kasutada mõnda aega või kui teil on vaja ressursside.

## <a name="1-node-and-5-node-cluster-mode"></a>1 sõlm ja 5 sõlm kobar režiim

Kohaliku kobar koostada töötades leiate sageli ise teeme kiirülevaate iteratsiooni muutmine kood, koodi kirjutamine, silumine silumine jne. Selle protsessi optimeerimiseks käitamise kohaliku kobar kaks režiimi: 1 sõlm või 5 sõlm. Nii kobar režiimid on oma eelised.
5 sõlm kobar režiim võimaldab töötada otse kobar. Saate testida Tõrkesiirde stsenaariumid, töötada mitme eksemplari ja teenuste koopiad.
1 sõlm kobar režiim on optimeeritud kiiret ja registreerimise teenused, mis aitavad teil kiiresti valideerimiseks kood teenuse struktuuri käitusaja abil teha.

Nii 1 sõlm klaster režiimi ja 5 sõlm kobar režiim ei ole emulaator või simulaatorit. See töötab sama platvormi koodi, mida mitme masina kogumite leitud.

> [AZURE.NOTE] See funktsioon on saadaval SDK versioon 5,2 ja kohal.

1 sõlm kobar kobar režiimi muutmiseks kasutage teenuse struktuuri kohalik kobar haldur või kasutage PowerShelli järgmisel viisil:

1. Käivitage PowerShelli uus aken administraatorina.

2. Käivitage kobar häälestamise skripti SDK kaust:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Kobar häälestus võtab mõne hetke aega. Kui häälestamine on lõpule jõudnud, peaksite nägema umbes väljundit:
    
    ![Kobar häälestamise väljund][cluster-setup-success-1-node]

Kui kasutate teenuse struktuuri kohaliku kobar haldur:

![Üleminek kobar režiim][switch-cluster-mode]

> [AZURE.WARNING] Kobar režiimi muutmisel praeguse kobar eemaldatakse teie süsteemi ja uus klaster luuakse. Teil on salvestatud kobar, andmed kustutatakse kobar režiimi muutmisel.

## <a name="next-steps"></a>Järgmised sammud
- Nüüd, kui teil on juurutatud ja uuendatud mõned valmismallide rakendused, võite [proovida, hoone oma Visual Studios](service-fabric-create-your-first-application-in-visual-studio.md).
- [Azure'i kobar](service-fabric-cluster-creation-via-portal.md) samuti saab teha kõigi kohaliku kobar selles artiklis tehtavad toimingud.
- Uuele versioonile, mida me selles artiklis on lihtne. Vaadake selle [täiendamine dokumentatsiooni](service-fabric-application-upgrade.md) kohta power ja paindlikkust teenuse struktuuri uuendamine.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
