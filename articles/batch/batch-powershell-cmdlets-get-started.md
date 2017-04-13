<properties
   pageTitle="Azure'i paketi PowerShelliga alustamiseks | Microsoft Azure'i"
   description="Tutvustuse Azure PowerShelli cmdlet-käskude abil saate hallata Azure'i paketi teenuse hankimine"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Azure'i paketi PowerShelli cmdlet-käskude kasutamise alustamine
Azure'i paketi PowerShelli cmdlet-käskude abil saate teha ja skript paljude samu toiminguid saate teha paketi API-d, Azure portaali ja Azure käsurea liides (CLI). See on cmdlet-käskude abil saate hallata paketi kontode ja töö paketi ressurssidega, nt kaustu, töö ja ülesannete tutvustuse.

Paketi cmdlet-käskude ja üksikasjalik cmdlet-käsu süntaks täieliku loendi leiate [Azure'i paketi cmdleti viide](https://msdn.microsoft.com/library/azure/mt125957.aspx).

Selles artiklis on Azure versioon 3.0.0 PowerShelli cmdlet-käskude alusel. Soovitame värskendada oma Azure PowerShelli sageli teenusevärskendused ja täiustatud eeliseid.

## <a name="prerequisites"></a>Eeltingimused

Azure'i PowerShelli abil saate hallata oma paketi ressursse järgmisi toiminguid.

* [Installida ja konfigureerida Azure PowerShell](../powershell-install-configure.md)

* Käivitage tellimuse (selle Azure'i paketi cmdlettide tarne Azure'i ressursihaldur mooduli) ühenduse **Login-AzureRmAccount** cmdlet-käsk:

    `Login-AzureRmAccount`

* **Paketi pakkuja nimeruumi registreerida**. Selle toimingu ainult peab olema tehtud **ühe korra tellimuse kohta**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Hallata paketi kontod ja võtmed

### <a name="create-a-batch-account"></a>Paketi konto loomine

**Uus-AzureRmBatchAccount** loob määratud ressursirühm paketi konto. Kui teil pole veel ressursirühma, luua käivitades cmdleti [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) . Määrake **asukoht** parameeter, näiteks "Kesk-USA" Azure piirkondade. Näiteks:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Looge paketi konto ressursi jaotises täpsustades <*account_name*> konto ja asukoht ja nimi oma ressursirühma nimi. Konto paketi loomine võib võtta aega lõpuleviimiseks. Näiteks:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] Konto nimi peab olema kordumatu ressursirühma Azure alale, sisaldavad 3 – 24 märki ja väiketähti ja numbreid ainult kasutamine.

### <a name="get-account-access-keys"></a>Konto kiirklahvide hankimine
**Get-AzureRmBatchAccountKeys** kuvab Azure'i paketi kontoga seotud kiirklahvide. Näiteks käivitage järgmine saada loodud konto esmaseid ja teiseseid võtmed.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Luua uusi Accessi võti
**Uus-AzureRmBatchAccountKey** loob uue konto esmane või teisene võtme Azure'i paketi konto jaoks. Näiteks tippige uus konto paketi primaarvõtme loomiseks:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Luua uus teisene võti, määrake "Teisest" **KeyType** parameeter. Peate looma eraldi esmaseid ja teiseseid võtmed.

### <a name="delete-a-batch-account"></a>Paketi konto kustutamine
**Eemalda – AzureRmBatchAccount** kustutab paketi konto. Näiteks:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Kui kuvatakse vastav viip, kinnitage konto eemaldada. Konto eemaldamise võib võtta aega lõpuleviimiseks.

## <a name="create-a-batchaccountcontext-object"></a>BatchAccountContext objekti loomine

Autentida paketi PowerShelli cmdlet-käskude abil saate luua ja hallata paketi kaustu, töö, tööülesannete ja muud ressursid, kõigepealt looma BatchAccountContext objekti salvestada oma konto nimi ja klahvid:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Objekti BatchAccountContext edastamiseks kasutada parameetrit **BatchContext** cmdlet-käsud.

> [AZURE.NOTE] Vaikimisi kasutatakse primaarvõtme konto autentimine, kuid saate valida konkreetselt võti BatchAccountContext objekti **KeyInUse** atribuudi kasutamine: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Luua ja muuta paketi ressursid
Nt **New-AzureBatchPool**, **New-AzureBatchJob**ja **Uus-AzureBatchTask** cmdlet-käskude abil saate luua ressursid paketi kasutajakontot. Vastavad **Get -** ja **Set -** olemasolevate atribuutide värskendamine, ja **Eemalda -** cmdlettide eemaldamiseks pakett kasutajakontot ressursid.

Nende cmdlettide Lisaks BatchContext objekti, läbides paljude kasutamisel peate loomine või läheb objekte, mis sisaldavad üksikasjalikku ressursisätete, nagu on näidatud järgmises näites. Leiate üksikasjaliku iga täiendava näiteid cmdlet-käsu jaoks.

### <a name="create-a-batch-pool"></a>Paketi kausta loomine

Kui loomine või värskendamine paketi kausta, valige pilvepõhise teenuse konfigureerimine või virtuaalse masina konfiguratsiooni operatsioonisüsteemi Arvuta sõlmed (vt [paketi funktsioonide ülevaade](batch-api-basics.md#pool)). Valitud määrab, kas teie Arvuta sõlmed on imaged ühe [Azure'i Külastajate OS välja](../cloud-services/cloud-services-guestos-update-matrix.md#releases) või ühe Azure'i turuplatsi toetatud Linux või Windows VM pilte.

**Uus-AzureBatchPool**käivitamisel edastada PSCloudServiceConfiguration või PSVirtualMachineConfiguration objekti operatsioonisüsteemi sätted. Näiteks järgmine cmdlet-käsk loob uue paketi rakenduskausta suurus väike Arvuta sõlmed pilvepõhise teenuse konfiguratsiooni imaged operatsioonisüsteemi uusima versiooni perekonna 3 (Windows Server 2012). Siin määrab **CloudServiceConfiguration** parameetri *$configuration* muutuja PSCloudServiceConfiguration objektina. Parameetri **BatchContext** määrab eelnevalt määratletud muutuv *$context* BatchAccountContext objekti.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Arvuta sõlmed kogumi uue sihtrühma arv määratakse mõnda autoscaling valemit. Sel juhul valem on lihtsalt **$TargetDedicated = 4**, Arvuta sõlmed kogumi arvu, mis näitab, on kõige 4.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Päringu kaustu, töökohtade, tööülesannete ja muud üksikasjad

Kasutada cmdlet-käskude nt **Get-AzureBatchPool**, **Get-AzureBatchJob**ja **Get-AzureBatchTask** päringu paketi kasutajakontot loodud üksused.

### <a name="query-for-data"></a>Päringu andmete jaoks

Näiteks oma kaustu otsimiseks kasutada **Get-AzureBatchPools** . Vaikimisi talletatakse see päringuid kõik kaustad vastavalt teie konto, eeldades, et teil on juba *$context*BatchAccountContext objekti.

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Kasutada filtrit OData

Teil on võimalik pakkuda OData filter, mis ainult teile huvi objektide leidmiseks parameetriga **Filter** . Näiteks võib leida kõik kaustad ID-d, alustades "myPool":

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

See meetod ei ole võimalikult paindlik kohaliku müügivõimaluste "Where-objekt" kasutamine. Siiski päringu saadetakse paketi teenuse otse nii, et kõigi filtrite juhtub serveripoolse, salvestades Interneti-läbilaskevõime.

### <a name="use-the-id-parameter"></a>Kasutage parameetrit Id

Määrab OData filter alternatiiv on kasutada parameetrit **ID-d** . Päringu jaoks teatud rakenduskausta koos id "myPool":

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Parameetri **Id** toetab ainult täis-id otsing, mitte metamärkide või OData-laadi filtrid.

### <a name="use-the-maxcount-parameter"></a>Kasutage parameetrit MaxCount

Vaikimisi iga cmdlet kuvab kuni 1000 objektid. Kui jõuate selle piirmäära, piiritleda filtrisse tuua tagasi objektide või konkreetselt määratud kuni parameetriga **MaxCount** . Näiteks:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Määrake ülemist eemaldamiseks **MaxCount** 0 või vähem.

### <a name="use-the-powershell-pipeline"></a>Müügivõimaluste PowerShelli kasutamine

Paketi cmdlettide saate kasutada PowerShelli müügivõimaluste saatmine cmdlet-käskude vahel. See on sama mõju täpsustades parameeter, kuid teeb koostööd mitme üksuste lihtsamaks.

Näiteks leida ja kuvada kõik tööülesanded jaotises konto.

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Taaskäivitage arvuti igal pool MSDN:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Paketi Rakendusehaldus

Rakenduse paketid võimalda lihtsustatud juurutada rakendused Arvuta sõlmi oma kaustadesse. Paketi PowerShelli cmdlet-käsud saate üles laadida ja hallata rakenduse paketid konto paketi ja juurutamine paketi versioonid arvutada sõlmed.

Rakenduse **loomine** :

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Lisa** rakendusepaketi:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Rakenduse **vaikeversiooniks** seadmiseks tehke järgmist.

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Loendi** rakenduse paketid

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Kustutage** rakendusepaketi

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Kustuta** rakendus

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Kustutage kõik rakenduse rakenduse paketi versioonid enne rakenduse kustutamiseks. Saate "Konflikti" tõrge, kui proovite kustutada rakendus, mis on praegu rakenduse paketid.

### <a name="deploy-an-application-package"></a>Rakendusepaketi juurutamine

Saate määrata ühe või mitme rakenduse paketid juurutamiseks on loomisel. Kui määrate paketi kausta loomise ajal, see juurutatud iga sõlme sõlm ühenduste pool. Paketid on ka juurutanud kui sõlme on rebooted või reimaged.

Määrake soovitud `-ApplicationPackageReference` valik pool, nagu nad liituda pool selle kausta sõlmed rakendusepaketi juurutamiseks loomisel. Esmalt **PSApplicationPackageReference** objekti loomine ja konfigureerimine selle rakenduse ID-d ja paketi versiooni soovite võtta kasutusele selle kausta Arvuta sõlmed.

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Nüüd pool loomine ja määrake paketi viide objekti argumendina soovitud `ApplicationPackageReferences` suvand:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Lisateavet rakenduse paketid leiate [Azure'i paketi rakenduse paketid rakenduse juurutamine](batch-application-packages.md).

>[AZURE.IMPORTANT] Peate kasutama rakenduse paketid kontole paketi [link Azure Storage konto](#linked-storage-account-autostorage) .

### <a name="update-a-pools-application-packages"></a>Mõne kausta rakenduse paketid värskendamine

Mõne olemasoleva kausta määratud rakenduste värskendamiseks kõigepealt looma PSApplicationPackageReference objekti soovitud atribuudid (rakenduste ID-d ja paketi versioon):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Järgmiseks pool toomine paketi, tühjendage välja mis tahes olemasolevaid pakettide, meie uue paketi viite lisamine ja värskendamine paketi teenuse uue kausta seaded:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Nüüd olete värskendanud paketi teenuse selle kausta atribuudid. Uue rakenduse paketi, et arvutada sõlmed kogumi tegelikult juurutamiseks siiski peate taaskäivitage või reimage need sõlmed. Taaskäivitamist sõlm igal pool see käsk:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] Arvuta sõlmed pargis saate juurutada mitme rakenduse paketid. Kui soovite *lisada* rakendusepaketi asemel asendades praegu juurutatud pakettide, argument on `$pool.ApplicationPackageReferences.Clear()` ülaloleva rea.

## <a name="next-steps"></a>Järgmised sammud

* Üksikasjalik cmdlet-käsu süntaks ja näited leiate [Azure'i paketi cmdleti viide](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Rakenduste ja rakenduse paketid paketi kohta leiate lisateavet teemast [rakenduse juurutamine Azure'i paketi rakenduse paketid](batch-application-packages.md).
