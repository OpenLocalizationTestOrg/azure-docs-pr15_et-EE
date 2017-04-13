<properties
   pageTitle="Läbides mandaadi abil DSC Azure | Microsoft Azure'i"
   description="Ülevaade turvaliselt läbib mandaadi abil PowerShelli soovitud oleku konfiguratsioon Azure'i virtuaalmasinates"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Azure'i DSC laiend sündmuseohjuri läbib identimisteave #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Selles artiklis antakse ülevaade soovitud riik konfiguratsiooni laiend Azure. DSC laiend sündmuseohjuri ülevaate leiate [Azure'i soovitud riik konfiguratsiooni laiend sündmuseohjuri tutvustus](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Läbides identimisteave
Konfiguratsiooni protsessi osana teil võib vaja häälestage Kasutajakontod, juurdepääsu teenustele või installida programmi kasutaja kontekstis. Järgmiste toimingute tegemiseks peate mandaati. 

DSC võimaldab parameetritega konfiguratsioone identimisteabe läks konfiguratsiooni ja turvaliselt talletatud RM failid. Azure'i laiend sündmuseohjuri lihtsustab mandaadi haldamine esitada sertifikaatide automaatne haldamine. 

Võtke arvesse järgmist DSC konfigureerimise skripti, mis loob uue kasutajakonto määratud parool.

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

See on oluline kaasata *sõlm localhost* konfiguratsiooni osana. Kui see lause on puudu, järgmised toimingud ei tööta laiend sündmuseohjuri spetsiaalselt otsitakse sõlm localhost lause. Ka on oluline kaasata typecast *[PsCredential]*, nagu see teatud tüüpi käivitab laiend mandaadi krüptimiseks. 

See skript avaldamine bloobimälu:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Azure'i DSC laiend määramiseks ja sisestage mandaat.

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Kuidas on tagatud identimisteave
See kood küsib mandaati. Kui see on esitatud, see on salvestatud mälu lühidalt. Kui see on avaldatud `Set-AzureVmDscExtension` cmdlet-käsk, edastatakse https-VM, kus Azure talletab selle krüptitud kettal, kohaliku VM serdiga. Seejärel on lühidalt mällu dekrüptida ja uuesti krüptitud DSC edastamiseks.

Selline käitumine erineb [turvaline konfiguratsioone ilma laiend sündmuseohjuri abil](https://msdn.microsoft.com/powershell/dsc/securemof). Azure'i keskkonnas annab viis edastavad konfiguratsiooni andmeid turvaliselt kaudu serdid. DSC laiend sündmuseohjuri kasutamisel ei ole vaja $CertificatePath või mõne $CertificateID / ConfigurationData $Thumbprint kirje.


## <a name="next-steps"></a>Järgmised sammud ##

Azure'i DSC laiend sündmuseohjuri kohta leiate lisateavet teemast [Azure soovitud riik konfiguratsiooni laiend sündmuseohjuri tutvustus](virtual-machines-windows-extensions-dsc-overview.md). 

Uurige [Azure'i ressursihaldur malli DSC pikendamise eest](virtual-machines-windows-extensions-dsc-template.md).

PowerShelli DSC, [külastage PowerShelli dokumentatsiooni](https://msdn.microsoft.com/powershell/dsc/overview)kohta lisateavet. 

Lisafunktsioone leidmiseks saate hallata PowerShelli DSC, [sirvige PowerShelli Galerii](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) ressursside DSC.
