<properties
    pageTitle="Azure'i ressursirühm projekti juurutamise skripti ülevaade | Microsoft Azure'i"
    description="Kirjeldatakse PowerShelli skripti Azure ressursirühm juurutamise projekti toimimise."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Azure'i ressursirühm projekti juurutamise skripti ülevaade

Azure'i ressursirühm juurutamise projektid aitavad teil etapp ja failide ja muude esemeid juurutada Azure. Saate luua Azure'i ressursihaldur juurutamise projekti Visual Studios, lisatakse PowerShelli skripti nimega **Deploy-AzureResourceGroup.ps1** projekt. Selles teemas antakse see skript mida ja kuidas täita nii seest kui ka väljaspool Visual Studio üksikasjad.

## <a name="what-does-the-script-do"></a>Mida teeb skripti?

Deploy-AzureResourceGroup.ps1 skript ei mõlemat oluliste juurutamise töövoog.

- Laadige faile või esemeid malli kasutamiseks vajalik
- Malli

Esimene osa skripti lisatud failide ja esemeid juurutamiseks ja viimase cmdlet-käsk skripti tegelikult malli. Näiteks kui virtuaalse masina peab olema konfigureeritud skripti, juurutamise skripti esmalt turvaliselt lisatud konfigureerimise skripti Azure storage konto. See teeb selle kättesaadavaks Azure'i ressursihaldur konfigureerida virtuaalse masina ettevalmistamise ajal.

Kuna kõik malli kasutuselevõttu pea eest esemeid, mis tuleb üles laadida, hinnatakse nimetatakse *uploadArtifacts* vahetamise parameeter. Kui mis tahes esemeid vaja üles laadida, määrake parameetrit *uploadArtifacts* skripti helistamisel. Pange tähele, et peamised mallifail ja parameetrite fail pole üles laadida. Ainult muid faile, nt konfiguratsioon skriptid, pesastatud juurutamise Mallid ja rakenduse failid on vaja üles laadida.

## <a name="detailed-script-description"></a>Skripti üksikasjalik kirjeldus

Järgmine on kirjeldus, mis valige jaotistes Deploy-AzureResourceGroup.ps1 Azure PowerShelli skripti ära.

>[AZURE.NOTE] See kirjeldab Deploy-AzureResourceGroup.ps1 skripti versiooni 1.0.

1.  Azure'i ressursihaldur juurutamise projekti jaoks vajalik parameetrite deklareerida. Mõned parameetrid on vaikeväärtused, kui projekt on loodud määratud. Saate muuta need vaikeväärtused skripti või lisada erinevate parameetrite väärtused, enne kui täidate skripti.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parameetri|Kirjeldus|
  	|---|---|
  	|$ResourceGroupLocation|Piirkonna või andmete keskele ressursirühma, nt **Lääne USA** või **Ida-Aasia**asukoht.|
  	|$ResourceGroupName|Azure'i ressursirühma nimi.|
  	|$UploadArtifacts|Kahendväärtus, mis näitab, kas esemeid on vaja Azure üles laadida süsteemist.|
  	|$StorageAccountName|Kui teie esemeid laaditakse Azure storage konto nimi.|
  	|$StorageAccountResourceGroupName|Azure'i ressursirühma, mis sisaldab salvestusruumi konto nimi.|
  	|$StorageContainerName|Salvestusruumi ümbrisest kasutatakse üleslaadimise esemeid nimi.|
  	|$TemplateFile|Juurutamise faili tee (`<app name>.json`) Azure'i ressursirühm projekti.|
  	|$TemplateParametersFile|Parameetrite faili tee (`<app name>.parameters.json`) Azure'i ressursirühm projekti.|
  	|$ArtifactStagingDirectory|Teie arvutisse, kus esemeid kohalikult üles, sh PowerShelli skripti juurkausta tee. See tee võib olla absoluutne või skripti asukoha suhtes.|
  	|$AzCopyPath|Kui AzCopy.exe tööriista kopeerib selle ZIP-failid, sh PowerShelli skripti juurkausta tee. See tee võib olla absoluutne või skripti asukoha suhtes.|
  	|$DSCSourceFolder|DSC (soovitud oleku konfiguratsioon) lähtekaust, sh PowerShelli skripti juurkausta tee. See tee võib olla absoluutne või skripti asukoha suhtes. Vajaduse korral lisateavet, lugege teemat [Azure PowerShelli DSC (soovitud oleku konfiguratsioon) laiendamine tutvustus](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx).|

1.  Kontrollige, kas esemeid on vaja Azure üles laadida. Kui ei, siis jätkake 11. Muul juhul tehke järgmist.

1.  Mis tahes muutujate suhtelised teed teisendada absoluutne teed. Näiteks muuta tee nagu `..\Tools\AzCopy.exe` et `C:\YourFolder\Tools\AzCopy.exe`. Lisaks lähtestada muutujate *ArtifactsLocationName* ja *ArtifactsLocationSasTokenName* väärtuseks null. *ArtifactsLocation* ja *SaSToken* võib parameetrite malli. Kui nende väärtuste on null pärast lugemist parameetrid faili, loob skripti need väärtused.

    Azure'i tööriistade abil parameetrite väärtused *_artifactsLocation* ja *_artifactsLocationSasToken* malli esemeid haldamine. Kui PowerShelli skripti leiab parameetrite nimed, kuid parameetrite väärtused ei esitata, skripti laaditakse esemeid ja tagastab nende parameetrite väärtused. See edastab need siis cmdlet-i kaudu `@OptionsParameters`.

  	|Muutuja|Kirjeldus|
  	|---|---|
  	|ArtifactsLocationName|Kus asub Azure esemeid tee.|
  	|ArtifactsLocationSasTokenName|SAS (ühiskasutusse Accessi allkiri) Turbeloa nimi, mida kasutatakse skripti teenuse siini autentida. Lisateabe saamiseks vaadake [Ühiskasutusse Accessi allkirja autentimine teenuse siini](service-bus-shared-access-signature-authentication.md) .|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Selle jaotise kontrollib, kas selle <app name>. parameters.json faili (nn "parameetrite file") on ema sõlm nimega **Parameetrid** (sisse selle `else` blokeerimine). Muul juhul on pole vanema sõlme. Kas vorming on lubatud.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Itereerima JSON parameetrite kaudu. Kui parameetri väärtus on määratud *_artifactsLocation* või *_artifactsLocationSasToken*, siis määrake muutuv *$OptionalParameters* neid väärtusi. See takistab skripti tahtmatult ülekirjutamise mis tahes esitate parameetrite väärtused.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Saada salvestusruumi konto ressursi kasutada hoidke esemeid juurutamiseks salvestusruumi konto võti ja kontekstis.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Kui kasutate PowerShelli DSC konfigureerida virtuaalse masina, pikendamiseks DSC esemeid lisada ühe zip-failina. Nii, et luua arhiivifail DSC konfiguratsiooni. Selleks, kontrollige $DSCSourceFolder olemasolu. Kui DSC konfigureerimine on olemas, eemaldage see ja seejärel looge uus tihendatud fail nimega dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Azure'i artefakte ei tee kui parameetrite fail, määrake PowerShelli skripti esemeid üleslaaditavate tee. Selleks saate luua kasutades kombinatsiooni salvestusruumi konto lõpp-punkti tee ja salvestusruumi container nimega tee. Seejärel värskendage parameetrid faili selle uue teega.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Kopeerige kõik failid teie kohaliku salvestusruumi drop tee online Azure Storage konto **AzCopy** kasuliku (sisaldub Azure'i ressursirühm juurutamise projekti kausta **Tööriistad** ) abil. Kui see toiming nurjus, väljuge skripti, kuna juurutamise ei ole vaja esemeid ilma aitavad.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Kui mõni SAS luba esemeid asukoha jaoks pole parameetrite fail, luua ajutine salvestusruum võrgus ümbris kirjutuskaitstud juurdepääsu. Seejärel edastavad selle SAS luba kellelegi käsurea nimega "optionalParameter." Pange tähele, et edasi käsurea parameetrid on ülimuslik failis parameetrite väärtused.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Ressursi rühma luua, kui see pole juba olemas ja Kontrollige malli ja parameetrite faili valideerimise vigu juurutamise õnnestumist takistavad.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Lõpuks malli. Järgmine kood tekitab juurutamise abil ajatempel kordumatu nimi.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Ressursirühma juurutamine

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Visual Studio ressursirühma juurutamiseks

1. Valige otseteemenüü Azure'i ressursirühm projekti **Deploy** > **Uue juurutamise**.

    ![][0]

1. Dialoogiboksis **Deploy ressursirühma** valida, kas ressursi olemasolevasse rühma rakenduses ripploendiboksi võtta kasutusele või valige ** &lt;Loo uus... &gt;** Kui soovite luua uue ressursirühma.

    ![][1]

1. Kui kuvatakse vastav viip, sisestage ressursi rühma nimi ja asukoht **Loomine ressursirühm** dialoogiboksi ja seejärel klõpsake nuppu **Loo** .

    ![][2]

1. Dialoogiboksi **Parameetrid redigeerida** kuvamiseks ja sisestage mis tahes puuduvate parameetrite väärtused **Parameetrite redigeerimiseks** klõpsake nuppu.

    ![][3]

    >[AZURE.NOTE] Kui mõni nõutav parameetrite väärtused, kuvatakse see dialoogiboksi automaatselt juurutamisel.

    ![][4]

1. Kui olete lõpetanud sisestada parameetrite väärtused, klõpsake nuppu **Salvesta** ja seejärel valige nupp **Deploy** .

    Juurutamise script (Deploy AzureResourceGroup.ps1) käivitub ja Azure juurutamine malli, mis tahes esemeid koos.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Juurutamiseks ressursirühma PowerShelli abil

Kui soovite skripti kiirmenüü käsk Visual Studio juurutada ja UI, kasutamata skripti käivitada, valige **Ava PowerShell ISE**.

![][5]


## <a name="command-deployment-examples"></a>Käsk juurutamise näited

### <a name="deploy-using-default-values"></a>Kasutades vaikeväärtused juurutamine

Selles näites kujutatakse parameetrite vaikeväärtuste kasutamine skripti käivitamiseks. (Kuna parameetri asukoht ei sisalda vaikeväärtuse, tuleb sisestada ühte.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Juurutamine vaikeväärtused alistamine

Selles näites kujutatakse skripti mall ja parameetrite failid, mis erinevad vaikeväärtused juurutamiseks.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>UploadArtifacts kasutamise lavastus juurutamine

Selles näites kujutatakse üles laadida esemeid kaustast väljaanne ja juurutamine vaikemallid skripti käivitamiseks.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

Selles näites kujutatakse Visual Studio online toimingu Azure PowerShelli skripti käivitada.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet Azure ressursihaldur lugedes [Azure'i ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md).

Veel näiteid töötamise Azure'i ressursirühm projektide, lugege teemat [Deploy ja hallata Azure ressursid](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 ühendamine [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Lisateavet quickstarts HealthClinic.biz demo, leiate [Azure'i Arendaja tööriistad Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
