<properties
   pageTitle="Saate luua ja hallata eraldi Azure teenuse struktuuri kobar | Microsoft Azure'i"
   description="Saate luua ja hallata ka Azure teenuse struktuuri kobar arvutis (füüsilise või virtuaalse) opsüsteemiga Windows Server, kas see on kohapealse või mis tahes pilveteenuses."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="dkshir;chackdan"/>


# <a name="create-and-manage-a-cluster-running-on-windows-server"></a>Loomine ja käivitamine Windows serveris klaster haldamine

Saate luua teenuse struktuuri kogumite virtuaalmasinates või arvutites, kus töötab Windows Server Azure'i teenuse struktuuri. See tähendab, et saate juurutada ja keskkonnas, mis sisaldab ühendatud Windows Server arvutite teenuse struktuuri rakendusi käivitada, kohapealne või mis tahes pilveteenuste pakkujaga. Teenuse struktuuri pakub installipaketi teenuse struktuuri kogumite ehk autonoomse Windows Server pakett loomiseks.

Selles artiklis tutvustatakse juhised luua klaster abil eraldiseisev pakett teenuse struktuuri kohapealne, kuigi seda saab hõlpsasti kohandada muude keskkonnas, nt muude teenusepakkujate pilve.

>[AZURE.NOTE] See autonoomse Windows Server pakett võib sisaldada funktsioone, mis on praegu eelvaade ja ärikasutuseks ei toetata. Siit leiate loendi funktsioonidest, mis on eelvaade, lugege teemat "eelvaade funktsioone kaasatud selle paketi." Võite ka [EULA koopia alla laadida](http://go.microsoft.com/fwlink/?LinkID=733084) .


<a id="getsupport"></a>
## <a name="get-support-for-the-service-fabric-standalone-package"></a>Teenuse struktuuri autonoomse paketi tootetugi

- Küsi ühenduse teenuse struktuuri eraldiseisev pakett Windows Server [Azure'i teenuse struktuuri Foorum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).

- Saate avada mõne Piletite [Professional tugi teenuse struktuuri](http://support.microsoft.com/oas/default.aspx?prid=16146 ).  Lisateavet [Microsofti tugiteenuse](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).

<a id="downloadpackage"></a>
## <a name="download-the-service-fabric-standalone-package"></a>Laadige teenuse struktuuri autonoomne


[Autonoomse Laadige teenuse struktuuri jaoks Windows Server 2012 R2 ja uuemad versioonid](http://go.microsoft.com/fwlink/?LinkId=730690), nimega Microsoft.Azure.ServiceFabric.WindowsServer. &lt;versioon&gt;zip.


Laadige alla pakett, leiate järgmised failid:

|**Faili nimi**|**Kokkuvõte**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|CAB fail, mis sisaldab iga seadme klaster juurutatud kahendfaile.|
|ClusterConfig.Unsecure.DevCluster.json|Kobar valimi konfiguratsioonifail, mis sisaldab mõnda turvamata, kolm sõlme ühe-(või virtuaalne arvuti) arengu kobar, sealhulgas teavet iga sõlme klaster sätete. |
|ClusterConfig.Unsecure.MultiMachine.json|Kobar valimi konfiguratsioonifail, mis sisaldab turvamata, mitme (või virtuaalne arvuti) kobar, sh teave iga seadme klaster sätete.|
|ClusterConfig.Windows.DevCluster.json|Kobar valimi konfiguratsioonifail, mis sisaldab kõiki sätteid on turvaline, kolm sõlme ühe-(või virtuaalne arvuti) arengu kobar, sh teave iga sõlme, mis on klaster. Klaster on turvatud abil [Windowsi identiteedid](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.Windows.MultiMachine.json|Kobar konfiguratsiooni valimi faili, mis sisaldab turvaline, mitme (või virtuaalne arvuti) kobar abil Windowsi Turve, sh teave igas arvutis, kus on turvaline kobar sätteid. Klaster on turvatud [Windows identiteedid](https://msdn.microsoft.com/library/ff649396.aspx)abil.|
|ClusterConfig.x509.DevCluster.json|Kobar valimi konfiguratsioonifail, mis sisaldab kõiki sätteid on turvaline, kolm sõlme ühe-(või virtuaalne arvuti) arengu kobar, sealhulgas teavet iga sõlme klaster. Klaster on turvatud abil x509 serdid.|
|ClusterConfig.x509.MultiMachine.json|Kobar konfiguratsiooni valimi faili, mis sisaldab turvaline, mitme (või virtuaalne arvuti) kobar, sealhulgas teavet iga sõlme turvaline kobar sätteid. Klaster on turvatud abil x509 serdid.|
|EULA_ENU.txt|Microsoft Azure'i teenuse struktuuri eraldiseisev Windows Server pakett kasutamiseks litsentsitingimused. Saate [alla laadida koopia EULA](http://go.microsoft.com/fwlink/?LinkID=733084) kohe.|
|Readme.txt|Link teemale väljalaskemärkmed ja installimise põhilised juhiseid. See on alamhulk juhiseid selles dokumendis.|
|CreateServiceFabricCluster.ps1|PowerShelli skripti, mis loob klaster ClusterConfig.json sätete abil.|
|RemoveServiceFabricCluster.ps1|PowerShelli skripti, mis eemaldab klaster ClusterConfig.json sätete abil.|
|ThirdPartyNotice.rtf |Kolmanda osapoole tarkvara, mis on pakett teade.|
|AddNode.ps1|PowerShelli skripti kohta, kuidas lisada sõlm olemasoleva juurutatud kobar.|
|RemoveNode.ps1|PowerShelli skripti sõlme eemaldamine olemasoleva juurutatud kobar.|
|CleanFabric.ps1|PowerShelli skripti puhastamiseks eraldi teenuse struktuuri installi praeguse seadme välja. Eelmise MSI installimist eemaldatakse abil oma seotud uninstallers.|
|TestConfiguration.ps1|PowerShelli skripti selle Cluster.json infrastruktuuri analüüsimiseks.|


## <a name="plan-and-prepare-your-cluster-deployment"></a>Järgmiseks juurutamise kobar
Enne klaster luua, tehke järgmist.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Samm 1: Leping oma kobar taristu
Olete loomine teenuse struktuuri kobar masinad kuulub teile, et saaksite otsustada, millist liiki tõrkeid soovitud klaster jääda. Näiteks ei vaja eraldi power jooned või Interneti-ühendused esitada need masinad? Lisaks, kaaluge need masinad füüsiline turvalisus. Kus asuvad masinad ja neile juurdepääsu vajav? Pärast nende otsuste saate vastendada masinad loogiliselt erinevate viga domeenid (vt samm 4). Kavandamise tootmise kogumite infrastruktuur on keerulisem kui testi kogumite.

<a id="preparemachines"></a>
### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Samm 2: Ettevalmistamine seotud vasta
Igas arvutis, kus soovite lisada klaster eeltingimused

- Soovitatav on vähemalt 16 GB RAM-i.
- Soovitatav on vähemalt 40 GB vaba kettaruumi.
- Soovitatav on 4 core või suurem CPU.
- Turvaline võrgu või võrkude kõik masinad Ühenduvus.
- Windows Server 2012 R2 või Windows Server 2012 (peate olema [KB2858668](https://support.microsoft.com/kb/2858668) installitud).
- [.NET Frameworki 4.5.1 või uuem versioon](https://www.microsoft.com/download/details.aspx?id=40773), täielik install.
- [Windows PowerShelli 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
- [RemoteRegistry teenuse](https://technet.microsoft.com/library/cc754820) peaks töötama kõik seadmed.

Kobar administraator juurutamine ja konfigureerimise klaster peavad olema iga masinad [administraatoriõigused](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) . Te ei saa installida teenuse struktuuri domeenikontrolleri.

### <a name="step-3-determine-the-initial-cluster-size"></a>Samm 3: Algne kobar mahu määratlemine
Iga sõlme autonoomse teenuse struktuuri kobar on teenuse struktuuri käitusaja juurutatud ja on klaster liige. Tüüpilised tootmise juurutamine, on üks sõlm OS eksemplari kohta (füüsilise või virtuaalse). Kobar suurus määrab teie ettevõtte vajadustele. Siiski peab teil olema minimaalne kobar suurus kolm sõlmed (masinad või virtuaalmasinates).
Arengu eesmärgil teil on rohkem kui üks sõlm antud arvutis. Tootmiskeskkonnas, teenuse struktuuri toetab ainult üks sõlm füüsilise või virtuaalse masina kohta.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Samm 4: Viga domeenide arvu ja täiendamine domeenide
*Viga domain* (FD) on tõrge füüsilise ühiku ja otseselt seotud füüsilise infrastruktuuri soovitud andmekeskuste. Viga domeeni koosneb tõrge ühtne jagavad riistavara (arvutites, parameetrid, võrkude ja muu). Kuigi kaardistamine 1:1 vahel viga domeenid ja raamile, arvestuskurss iga sektsioon pidada viga domeeni. Kui arutate soovitud klaster sõlmi, soovitame, et jaotada sõlmed vähemalt kolme viga domeenide vahel.

Kui määrate FDs ClusterConfig.json, saate valida iga FD nimi. Teenuse struktuuri toetab hierarhilise FDs, nii, et te saate vastavalt oma taristu topoloogia neid.  Näiteks järgmised FDs on lubatud.

- "faultDomain": "fd: / Room1/Rack1/Machine1"
- "faultDomain": "fd: / FD1"
- "faultDomain": "fd: / Room1/Rack1 PDU1 M1"


Mõnda *domeeni täiendamine* (UD) on loogiline sõlmed. Teenuse struktuuri orkestreerinud uuendamine (rakenduse täiendamine või kobar täiendamine), on UD kõik sõlmed võetakse versioonitäienduseks sõlmed muude UDs jäävad valmis kutsed. Püsivara uuendamine sooritate oma masinad au UDs, nii, et peate tegema need üks arvuti korraga.

Lihtsaim viis mõelda need on kavandatud hooldustööde ühik planeerimata ja UDs üksusena FDs silmas pidada.

Kui määrate UDs ClusterConfig.json, saate valida iga UD nimi. Näiteks järgmised nimed on lubatud.

- "upgradeDomain": "UD0"
- "upgradeDomain": "UD1A"
- "upgradeDomain": "DomainRed"
- "upgradeDomain": "Sinine"

Täiendamine domeenide ja viga domeenide üksikasjalikumat teavet leiate teemast [teenuse struktuuri kobar kirjeldav](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>Juhis 5: Laadige teenuse struktuuri autonoomse Windows Server
[Teenuse struktuuri autonoomse Laadige Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) ja pakkige see lahti pakkimine, kas juurutamise arvutisse, mis pole osa klaster või üks masinad, mis on osa klaster. Võib-olla mahalaadimist kausta ümbernimetamine `Microsoft.Azure.ServiceFabric.WindowsServer`.

<a id="createcluster"></a>
## <a name="create-your-cluster"></a>Klaster loomine

Pärast seda, kui olete läinud plaanimiseks ja ettevalmistamiseks juhised läbi, olete valmis looma klaster.

### <a name="step-1-modify-cluster-configuration"></a>Samm 1: Muutmine kobar konfigureerimine
Klaster on kirjeldatud ClusterConfig.json. Selle faili jaotiste kohta täpsema teabe saamiseks vt [autonoomse Windows kobar sätete konfigureerimine](service-fabric-cluster-manifest.md).
Ühe ClusterConfig.json faili avamine allalaaditud paketi ja muuta järgmisi sätteid.

<!--Loc Comment: Please, check that line 129 the clause has been modified to "that you use as placement constraints" instead of using "you are used as placement constraints"-->

|**Otsingukonfiguratsiooni säte**|**Kirjeldus**|
|-----------------------|--------------------------|
|**NodeTypes**|Tüüpi abil saate oma kobar sõlmed omaette eri rühmad. Klaster peab olema vähemalt üks NodeType. Rühma kõik sõlmed on levinud järgmised omadused. <br> **Nimi** – see on sõlm tippige nimi. <br>**Lõpp-punkti pordid** – need on erinevad nimega lõpp-punkti (pordid) mis on seotud sõlm tüüp. Saate kasutada mis tahes pordi number, mida soovite, kui nad vastuollu midagi muud selles manifestis ja ei ole juba mõni muu rakendus, mis töötab seadme/VM. <br> **Paigutuse atribuudid** - kirjeldavad need atribuudid see sõlm tüüp, mida kasutate paigutuse piiranguid süsteemi teenustele või teenuste jaoks. Neid atribuute on kasutaja määratletud võti ja väärtuse paarideks metaandmed eest ette antud sõlm. Sõlm atribuudid oleks, kas sõlme on kõvaketas või graafikakaarti, spindlid oma kõvakettale, südamikud ja muude omaduste arvu. <br> **Võimalusi** - sõlm võimalusi Määratle nimi ja kogus ressursile, mis on konkreetse sõlme tarbimine jaoks saadaval. Näiteks võib sõlm määratleda, et see on nimega "MemoryInMb" mõõdiku võimsus ja see on vaikimisi 2048 MB saadaval. Need kasutatakse käitusajal tagamaks, et teenused, mis nõuavad teatud hulgal ressursse paigutatakse sõlmed, mis on nõutavad summad need ressursid.<br>**IsPrimary** – kui teil on rohkem kui üks NodeType määratletud veenduge, et ainult üks on seatud juurde esmane koos selle väärtuse *true*, mis on, kus süsteemi teenuste käivitamine. Kõigi muude tüüpide sõlm peaks olema seatud väärtus *false*|
|**Sõlmed**|Need on üksikasjade iga sõlmed, mis on osa kobar (sõlm tüüp, sõlme nimi, IP-aadress, domeeni viga ja täiendamine domeeni sõlme). Masinad soovitud klaster siin loetletud koos oma IP-aadressid on vaja luua. <br> Kui kasutate kõik sõlmed sama IP-aadress, seejärel üks-karbi kobar on loodud, mida saate katsetamiseks kasutada. Ärge kasutage ühe-karbi kogumite tootmise töökoormus kasutamise kohta.|


### <a name="step-2-run-the-testconfiguration-script"></a>Samm 2: TestConfiguration skripti käivitada.

Skripti TestConfiguration kontrollib teie taristu cluster.json veenduge, et vajalikud õigused on määratud, masinad on omavahel ühendatud ja muud atribuudid on määratud nii, et juurutamise õnnestub määratletud. See on põhimõtteliselt mini head tavad analüsaator. Jätkame lisada rohkem valideerimised see tööriist aja jooksul oleks senisest käepärasemad.

See skript saab käitada arvutis, mis on administraatori juurdepääs masinad, mis on loetletud sõlmed kobar konfiguratsioonifailis. Arvuti, mis see skript käivitatakse pole osa klaster.

```powershell

PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True


```

### <a name="step-3-run-the-create-cluster-script"></a>Samm 3: Loo kobar skripti käivitada.
Olete kobar konfiguratsiooni JSON dokumenti muutnud ja lisatud sõlme teavet, kobar loomine *CreateServiceFabricCluster.ps1* PowerShelli skripti kaustast paketi käivitamine ja edastama JSON konfiguratsiooni faili tee. Kui see on lõpule jõudnud, aktsepteerima EULA.

See skript saab käitada arvutis, mis on administraatori juurdepääs masinad, mis on loetletud sõlmed kobar konfiguratsioonifailis. Arvuti, mis see skript käivitatakse pole osa klaster.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

>[AZURE.NOTE] Juurutamise logid on saadaval kohalikult käivitasite CreateServiceFabricCluster PowerShelli kohta VM/kohapeal. Need on alamkausta DeploymentTraces kaustas, kuhu PowerShelli käsu käivitamist. Kui teenuse struktuuri on õigesti juurutatud masina vaatamiseks leiate installitud failid kataloogis C:\ProgramData ja protsesside FabricHost.exe ja Fabric.exe näha Tegumihalduris.

### <a name="step-4-connect-to-the-cluster"></a>Samm 4: Ühenduse klaster

Ühenduse turvaline kobar, lugege teemat [teenuse struktuuri ühenduse turvaline kobar](service-fabric-connect-to-secure-cluster.md).

Ühenduse loomine mõne ebaturvaliste kobar, käivitage PowerShelli käsk:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### <a name="step-5-bring-up-service-fabric-explorer"></a>Juhis 5: Avab teenuse struktuuri Explorer

Nüüd saate luua ühenduse teenuse struktuuri Explorer kas otse üks masinad http://localhost:19080/Explorer/index.html või eemalt http://< klaster*IPAddressofaMachine*>: 19080/Explorer/index.html.



## <a name="add-and-remove-nodes"></a>Lisamine ja eemaldamine sõlmed

Saate lisada või eemaldada sõlmed klaster autonoomse teenuse struktuuri, kui ettevõtte vajadused muutuvad. Üksikasjalikud juhised leiate [lisamine või eemaldamine sõlmed teenuse struktuuri autonoomse arvutikobaras](service-fabric-cluster-windows-server-add-remove-nodes.md) .


## <a name="remove-a-cluster"></a>Klaster eemaldamine

Klaster eemaldamiseks *RemoveServiceFabricCluster.ps1* PowerShelli skripti kaustast paketi käivitamine ja edastama JSON konfiguratsiooni faili tee. Soovi korral saate määrata asukoha logi kustutamine.

See skript saab käitada arvutis, mis on administraatori juurdepääs masinad, mis on loetletud sõlmed kobar konfiguratsioonifailis. Arvuti, mis see skript käivitatakse pole osa klaster.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```


<a id="telemetry"></a>
## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Telemeetria kogutud andmete ja kuidas see neile kasuks tuli

Vaikimisi toote kogub telemeetria teenuse struktuuri kasutus toote parandamiseks. Parim tava analüsaator, mis töötab Ühenduvus häälestamise kontrolle osana [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Kui see pole kättesaadav, on installimine nurjub, kui valite telemeetria välja.

  1. Müügivõimaluste telemeetria proovib järgmised andmed üleslaadimine [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) üks kord päevas. See on parim – peegeldav üles ja kobar-funktsioonid ei mõjuta. Telemeetria saadetakse ainult sõlm, mis töötab on tõrkesiire esmane haldur. Sõlmi pole saata telemeetria.

  2. Telemeetria koosneb järgmistest:

-            Teenuste arv
-            ServiceTypes arv
-            Rakenduste arv
-            ApplicationUpgrades arv
-            FailoverUnits arv
-            InBuildFailoverUnits arv
-            UnhealthyFailoverUnits arv
-            Vastuste arv
-            InBuildReplicas arv
-            StandByReplicas arv
-            OfflineReplicas arv
-            CommonQueueLength
-            QueryQueueLength
-            FailoverUnitQueueLength
-            CommitQueueLength
-            Arvu sõlmed.
-            IsContextComplete: Tõene/väär
-            ClusterId: See on genereeritud juhusliku ID-ga jaoks iga kobar GUID
-            ServiceFabricVersion
-             IP-aadress on virtual või arvutis, kust on üles laaditud telemeetria


Telemeetria keelamiseks lisage järgmine oma kobar config *Atribuudid* : *enableTelemetry: false*.

<a id="previewfeatures"></a>
## <a name="preview-features-included-in-this-package"></a>Eelvaate funktsioonide pakett

Mitte keegi.

>[AZURE.NOTE] Uue [GA versiooni Windows Server autonoomse kobar (versioon 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), saate üle minna klaster tulevaste väljalasete, automaatselt või käsitsi. Kuna see funktsioon pole saadaval eelvaade versioonid, peate klaster GA versiooni abil luua ja teie andmeid ja rakendusi eelvaade kobar migreerimist. Hoia selle funktsiooni kohta lisateabe saamiseks.


## <a name="next-steps"></a>Järgmised sammud
- [Autonoomse Windows kobar sätete konfigureerimine](service-fabric-cluster-manifest.md)
- [Lisage või eemaldage sõlmed autonoomse teenuse struktuuri kobar](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Luua eraldi teenuse struktuuri kobar opsüsteemi Windows Azure VMs](service-fabric-cluster-creation-with-windows-azure-vms.md)
- [Turvaline autonoomse kobar Windows Windows turvalisuse abil](service-fabric-windows-cluster-windows-security.md)
- [Autonoomse kobar abil X509 Windows Secure serdid](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
