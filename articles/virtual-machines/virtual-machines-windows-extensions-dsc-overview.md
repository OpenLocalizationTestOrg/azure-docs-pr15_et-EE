<properties
   pageTitle="Maakond konfiguratsiooni Azure ülevaate soovitud | Microsoft Azure'i"
   description="Ülevaade Microsoft Azure'i laiend sündmuseohjuri PowerShelli soovitud riik konfiguratsiooni kasutamise kohta. Sh eeltingimused, arhitektuur, cmdlet-käsud."
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

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Azure'i soovitud riik konfiguratsiooni laiend sündmuseohjuri tutvustus #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft Azure'i taristu teenused on Azure VM Agent ja seotud laiendid. VM laiendid on tarkvara komponendid, mis laiendavad VM ja hõlbustada mitmesuguste VM haldamise toimingute kohta. Näiteks VMAccess laiend saab kasutada ka administraatori parooli lähtestamine või kohandatud skript laiend saab käivitada skripti VM.

Selles artiklis tutvustatakse PowerShelli soovitud oleku konfiguratsioon (DSC) laiendamine Azure'i vms Azure'i PowerShelli SDK osana. Saate üles laadida ja rakendada PowerShelli DSC konfigureerimine on lubatud laiendiga PowerShelli DSC Azure'i VM uued cmdlet-käsud. PowerShelli DSC laiend kõnesid üheks PowerShelli DSC jõustada vastuvõetud DSC konfiguratsiooni VM. See funktsioon on saadaval Azure portaali kaudu.

## <a name="prerequisites"></a>Eeltingimused ##
**Kohalikus arvutis** Suhelda Azure VM laiend, peate kasutama kas Azure portaali või Azure PowerShelli SDK. 

**Külalisena Agent** Azure'i VM, mis konfigureeritakse DSC konfiguratsioon peab olema OS, mis toetab kas Windows Management Framework (WMF) 4.0 ja 5.0. Toetatud OS versioonid täieliku loendi leiate [DSC laiend versiooniajalugu](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Terminite ja mõisted ##
Sellest juhendist eeldab tundmine järgmist põhimõtet:

Konfiguratsiooni - DSC konfiguratsiooni dokumendi. 

Sõlm - eesmärki, DSC konfigureerimine. Selles dokumendis "sõlme" alati viitab mõni Azure VM.

Otsingukonfiguratsiooni andmete - lisamine .psd1 faili andmeid sisaldav keskkonna konfiguratsioon

## <a name="architectural-overview"></a>Arhitektuuri ülevaade ##

Azure'i DSC laiendamine kasutab Azure VM Agent raames esitamisel, Jõusta ja aruandluseks töötava Azure VMs DSC konfiguratsioone. DSC laiend eeldab, et vähemalt konfiguratsiooni dokumendi ja Azure PowerShelli SDK või Azure portaali kaudu esitatud parameetrite kogumi sisaldavad ZIP-faili.

Kui pikendamise nimetatakse esimest korda, see töötab installimise käigus. Selle protsessi installib versiooni, kuvatakse Windows Management Framework (WMF) järgmise loogika abil:

1. Kui Azure VM OS on Windows Server 2016, pole suureks. Windows Server 2016 on juba installitud PowerShelli uusim versioon.
2. Kui soovitud `wmfVersion` atribuut on määratud, selle WMF versioon on installitud, v.a juhul, kui see ei ühildu selle VM OS.
3. Kui pole `wmfVersion` atribuut pole määratud, WMF Viimane kohaldatav versioon on installitud.

WMF installimiseks uuesti. Taaskäivitage arvuti, pärast laiendamist allalaadimist määratud ZIP-faili selle `modulesUrl` atribuut. Kui selles asukohas on Azure'i bloobimälu, SAS luba saab määrata selle `sasToken` atribuudi failile juurdepääsu. Pärast ZIP alla ja lahti, funktsiooni konfigureerimine määratletud `configurationFunction` käitatakse RM faili loomiseks. Pikendamise töötab seejärel `Start-DscConfiguration -Force` loodud RM-faili kohta. Pikendamise sisaldab väljundi ja kirjutab selle tagasi Azure'i olek kanal. Sellest punktist DSC LCM tegeleb jälgimise ja parandamise nagu tavaliselt. 

## <a name="powershell-cmdlets"></a>PowerShelli cmdlet-käsud ##

PowerShelli cmdlet-käskude saab ARM või ASM paketti, avaldamine ja DSC laiend juurutuste jälgimine. Järgmised cmdlet-käsud loendis on ASM moodulid, kuid "Azure" saate asendada "AzureRm" ARM mudeli kasutamine. Näiteks `Publish-AzureVMDscConfiguration` kasutab ASM, kus `Publish-AzureRmVMDscConfiguration` kasutab ARM. 

`Publish-AzureVMDscConfiguration`konfiguratsioonifailis võtab, otsib sõltuv DSC ressursside ja loob konfigureerimine ja DSC jõustada konfiguratsiooni vajalikud ressursid sisaldavad ZIP-faili. Saate luua ka kohalikult kasutada funktsiooni `-ConfigurationArchivePath` parameeter. Muul juhul avaldab Azure'i bloobimälu ZIP-faili ja kinnitage SAS luba.

Selle cmdlet-käsu loodud ZIP-faili on .ps1 konfigureerimise skripti juurtasemel arhiivikausta. Ressursid on mooduli kausta paigutatakse arhiivikausta. 

`Set-AzureVMDscExtension`serveripoolse PowerShelli DSC laiend seaded VM konfiguratsiooni objekti, mida saab seejärel rakendada mõne Azure VM koos `Update-AzureVM`.

`Get-AzureVMDscExtension`toob DSC pikendamise oleku kindla VM. 

`Get-AzureVMDscExtensionStatus`toob jõustatud DSC laiend sündmuseohjuri DSC konfiguratsiooni olekut. Seda toimingut saate sooritada ühe VM või rühma vms.

`Remove-AzureVMDscExtension`eemaldab antud virtuaalse masina laiendi sündmuseohjuri. Selle cmdlet-käsu ei **Eemalda konfiguratsiooni,** desinstallige WMF või virtuaalse masina rakendatud sätete muutmist. See eemaldab ainult laiend sündmuseohjuri. 

**Peamised erinevused ASM ja ARM cmdletid**

- Sünkroonsed on ARM cmdlet-käsud. ASM cmdlet-käsud on asünkroonne.
- ResourceGroupName, VMName, ArchiveStorageAccountName, versioon ja asukoht on uus kõik parameetrid.
- ArchiveResourceGroupName on uus valikuline parameeter on ARM. See parameeter saate määrata, kui teie konto salvestusruumi kuulub erinevate ressursirühm, kui üks kus luuakse virtuaalse masina.
- ConfigurationArchive nimetatakse ArchiveBlobName rühmas
- Ümbrisenimi nimetatakse ArchiveContainerName rühmas
- StorageEndpointSuffix nimetatakse ArchiveStorageEndpointSuffix rühmas
- Parameetrit automaatne uuendamine on lisatud ARM lubamiseks automaatset värskendamist, laiend sündmuseohjuri nimega uusim versioon ja kui see on saadaval. See parameeter on võivad põhjustada Nnote taaskäivitamisega VM WMF uue versiooni väljaandmisel. 


## <a name="azure-portal-functionality"></a>Azure'i portaali funktsioonid ##
Liikuge sirvides klassikaline VM. Klõpsake jaotises Sätted -> üldist nuppu "laiendid." Luuakse uus paan. Klõpsake "Lisa" ja valige PowerShelli DSC.

Portaali peab sisestatud.
**Konfiguratsiooni moodulid või skripti**: See väli on kohustuslik. Nõuab .ps1 faili, mis sisaldab konfigureerimise skripti või .ps1 konfiguratsiooni skripti juurkausta ja kõik sõltuvad ressursside mooduli kaustad ZIP ZIP-faili. See saab luua selle `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` kaasatud SDK Azure PowerShelli cmdlet-käsk. ZIP-faili laaditakse teie kasutaja bloobimälu tagatud SAS luba. 

**Andmete PSD1 konfiguratsioonifail**: See väli on vabatahtlik. Kui konfiguratsioonist nõuab .psd1 konfiguratsiooni andmefaili, kasutada selle välja selle valimiseks ja üleslaadimine kasutaja bloobimälu kui see on tagatud SAS luba. 
 
**Konfiguratsiooni Module-Qualified nimi**: .ps1 faile võib olla mitu konfiguratsiooni funktsioone. Sisestage nimi, millele järgneb konfigureerimise .ps1 skripti lisamine '\' ja konfiguratsiooni funktsiooni nime. Näiteks kui teie .ps1 script on nimi "configuration.ps1" ja konfiguratsioon on "IisInstall", sisestate:`configuration.ps1\IisInstall`

**Konfiguratsiooni argumente**: Kui funktsiooni konfigureerimine on argumenti, sisestage need siit vormingus `argumentName1=value1,argumentName2=value2`. Märkus. see vorming on muus vormingus kui kuidas konfiguratsiooni argumendid aktsepteeritakse PowerShelli cmdlet-käskude või ressursihaldur Mallid kaudu. 

## <a name="getting-started"></a>Alustamine ##

Azure'i DSC laiend võtab DSC konfigureerimine dokumentide ja jõustab need Azure VMs kohta. Lihtne näide konfiguratsiooni järgib. Salvestage see kohalik "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Järgmised toimingud paigutamiseks määratud VM IisInstall.ps1 skripti, käivitada konfiguratsiooni ja oleku aruande.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Logimine ##

Logide paigutatakse:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[versiooninumber]

## <a name="next-steps"></a>Järgmised sammud ##

PowerShelli DSC, [külastage PowerShelli dokumentatsiooni](https://msdn.microsoft.com/powershell/dsc/overview)kohta lisateavet. 

Uurige [Azure'i ressursihaldur malli DSC pikendamise eest](virtual-machines-windows-extensions-dsc-template.md
). 

Lisafunktsioone leidmiseks saate hallata PowerShelli DSC, [sirvige PowerShelli Galerii](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) ressursside DSC.

Läbimise üheks konfiguratsioone tundliku parameetrite kohta täpsema teabe saamiseks vt [haldamine mandaadi turvaliselt DSC laiend sündmuseohjuri](virtual-machines-windows-extensions-dsc-credentials.md).
