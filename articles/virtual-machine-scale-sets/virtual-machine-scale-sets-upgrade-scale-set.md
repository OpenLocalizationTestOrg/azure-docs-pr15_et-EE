<properties
    pageTitle="Rakenduse virtuaalse masina skaala komplekti juurutamine | Microsoft Azure'i"
    description="Virtuaalse masina skaala komplekti rakenduse juurutamine"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Versioonitäienduse virtuaalse masina skaala määramine

Selles artiklis kirjeldatakse, kuidas saate ärirakenduseks OS värskendust ja Azure virtuaalse masina ulatusega ilma mis tahes seadmine. Sellega kaasneb värskendust OS versioon või SKU OS muutmise või URI kohandatud pildi muutmine. Värskendamine ilma tööseisakute värskendamise virtuaalmasinates ühe korraga või rühmade (nt ühe viga domeeni korraga) asemel kõik korraga. Nii saab hoida mis tahes virtuaalmasinates, mis pole uuendatakse töötab.

Mitmetähenduslikkuse vältimiseks vaatame eristada OS update, võite teha kolme tüüpi:

- Versiooni või SKU platvormi pildi muutmist. Näiteks muutmise Ubuntu 14.04.2-LTS versioon: 14.04.201506100 14.04.201507060, või muuta 16.04.0-LTS/latest Ubuntu 15.10/Viimased SKU-ga. Selles artiklis käsitletakse seda stsenaariumi.

- URI, mis viitab uue versiooni kohandatud pildi muutmise ehitatud (**Atribuudid** > **virtualMachineProfile** > **storageProfile** > **tavalise nimetusega osDisk** > **pilt** > **uri**). Selles artiklis käsitletakse seda stsenaariumi.

- Lappimine OS alates sees virtuaalse masina (näiteks võib turvalisus paik installimist ja Windows Update'i). See stsenaarium on toetatud, kuid ei hõlma käesolevas artiklis.

Kaks esimest suvandid on toetatud käesoleva artikli nõuetele. Peate luua uue skaala seadmine käivitada kolmas valik.

Virtuaalse masina skaala komplekti osana on [Azure teenuse struktuuri](https://azure.microsoft.com/services/service-fabric/) kobar juurutatud siin ei käsitleta.

Tavaline järjestuse muutmise OS versioon/SKU platvormi pildi või kohandatud pildi URI näeb välja järgmine:

1. Saada virtuaalse masina skaala määramine mudel.

2. Saate muuta versioon, SKU-ga või URI mudelis olev väärtus.

3. Värskendage mudel.

4. Tehke *manualUpgrade* Helista virtuaalmasinates skaala määramine kohta. See toiming on oluline, kui *upgradePolicy* on seatud **käsitsi** oma skaala komplekti ainult. Kui see on seatud **Automaatne**, kõik virtuaalmasinates on uuendatud korraga, põhjustades tööseisakute.


Selle tausta teabe meeles pidada, vaatame, kuidas saanud värskendada PowerShelli ja REST API abil määrata skaala versiooni. Nendes näidetes hõlmama platvormi pilt, kuid selles artiklis antakse piisavalt saab kohandada selle protsessi kohandatud pilt.

## <a name="powershell"></a>PowerShelli##

Selles näites värskendab Windowsi virtuaalse masina skaala määramine 4.0.20160229 uus versioon. Pärast värskendamist mudelit, on see värskendus ühe virtuaalse masina eksemplari korraga.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Kui värskendate kohandatud pildi muutmise platvormi pildi versioon asemel URI, asendage "määrata uue versiooni" rea umbes järgmine:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>REST API-ga

Siin on paar Python näited, kus kasutatakse Azure REST API hakkama värskendust OS versioon. Mõlemad teha saamiseks skaala määramine mudel, uuendatud mudeli panna, millele järgneb kerge [azurerm](https://pypi.python.org/pypi/azurerm) teegi Azure'i REST API ümbris funktsioonide abil. Ta vaadata ka virtuaalse masina eksemplarid vaadete tuvastamiseks virtuaalmasinates Värskenda Domeen.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) on Python skripti, mis kasutatakse hakkama versiooniuuenduse OS töötava virtuaalse masina skaala määrata ühe update domain korraga.

![Vmssupgrade skripti virtuaalmasinates või on Värskenda Domeen](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

See skript võimaldab teil valida kindla virtuaalmasinates värskendada või määrata mõne Värskenda Domeen. See toetab muutmise platvormi pildi versioon või muutmise URI kohandatud pildi.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) on üldotstarbeline editor virtuaalse masina skaala komplekti, mis näitab virtuaalse masina oleku heatmap kui ühe rea tähistab ühe update domain. Muu hulgas saate värskendada mudeli skaala hulga jaoks uus versioon, SKU-ga või kohandatud pildi URI ja valige vea domeenide versioonile. Kui te seda teete, on kõik selle värskenduse domeeni virtuaalmasinates versioonile uus mudel. Teise võimalusena saate teha jooksva versiooniuuenduse paketi suurusest oma valik.  

Järgmine pilt kuvatakse mudeli seadmine Ubuntu 14.04-2LTS versioon 14.04.201507060 skaala. See tööriist on lisatud palju võimalusi toimunud sellel kuvatõmmisel.

![Vmsseditor mudeli ulatusega Ubuntu 14.04-2LTS määramine](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Pärast seda, kui klõpsate **täiendada** , ja seejärel **Saada üksikasjad**, käivitage virtuaalmasinates rakenduses UD 0 värskendamiseks.

![Vmsseditor kuvatakse värskenduse pooleli.](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)
