<properties
    pageTitle="Jäädvustada Linux VM mallina kasutada | Microsoft Azure'i"
    description="Saate teada, kuidas jäädvustada ja üldistada Linux-põhine Azure virtuaalse masina (VM) loodud mudeliga Azure'i ressursihaldur juurutamise pilt."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Jäädvustada Linux virtuaalse masina töötavate Azure

Järgige selle artikli üldistada ja jäädvustada oma Azure'i Linux virtuaalse masina ressursihaldur juurutamise mudeli (VM). Kui te üldistada VM, isikliku teabe eemaldamine ja VM kasutada pildi ettevalmistamine. Seejärel saate jäädvustada generalized virtuaalse kõvaketta (VHD) pilt OS, VHDs jaoks lisatud andmed ketast ja uue VM juurutuste [ressursihaldur malli](../azure-resource-manager/resource-group-overview.md) . 

VMs abil pildi loomiseks iga uue VM võrgu ressursse häälestamine ja malli (või JSON, JavaScripti Objektiesituse fail) abil saate juurutada VHD jäädvustatud pildid. Sel viisil saate ise oma praeguse tarkvara konfiguratsiooni, sarnaselt pilte kasutada Azure'i turuplatsi VM.

>[AZURE.TIP]Kui soovite luua oma olemasoleva Linux VM koopia varundamine või silumine eriotstarbelisi olekuga, lugege teemat [loomine Linux virtuaalse masina töötavate Azure koopia](virtual-machines-linux-copy-vm.md). Ja kui soovite üles laadida Linux VHD on kohapealse VM, lugege teemat [üles laadida ja luua kohandatud plaat Linux VM](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et te vastama järgmistele tingimustele:

* **Azure'i VM loodud ressursihaldur juurutamise mudeli** – kui te pole loonud Linux VM, saate kasutada [portaali](virtual-machines-linux-quick-create-portal.md), [Azure'i CLI](virtual-machines-linux-quick-create-cli.md)või [ressursihaldur Mallid](virtual-machines-linux-cli-deploy-templates.md). 

    Konfigureerige VM vastavalt vajadusele. Näiteks [lisada andmed ketast](virtual-machines-linux-add-disk.md), värskendused ja rakenduste installimine. 
* **Azure'i CLI** - installi [Azure'i CLI](../xplat-cli-install.md) kohalikus arvutis.

## <a name="step-1-remove-the-azure-linux-agent"></a>Samm 1: Eemaldage Azure'i Linux agent

Esmalt käivitage **waagent** käsk koos parameetriga **deprovision** Linuxi. See käsk kustutab failide ja andmete teha VM valmis üldistamist. Lisateavet leiate teemast [Azure Linux Agent kasutusjuhendis](virtual-machines-linux-agent-user-guide.md).

1. Ühenduse loomine oma Linux VM SSH kliendi abil.

2. Klõpsake aknas SSH tippige järgmine käsk:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Käivitage see käsk ainult VM, mida kavatsete jäädvustada pildina. Ei garanteeri, et pilt on tühi, tundlikku teavet või sobib korduvalt.

3. Tippige **y** jätkata. Saate lisada soovitud **-jõustada** parameetri kinnituse selle juhise vältimiseks.

4. Kui käsk on lõpule jõudnud, tippige **väljumine**. Selles etapis tuleb suleb SSH kliendi.

    
## <a name="step-2-capture-the-vm"></a>Samm 2: Jäädvustada VM

Azure'i CLI abil üldistada ja jäädvustada VM. Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisaldavad **myResourceGroup**, **myVnet**ja **myVM**.

5. Avage kohalikust arvutist Azure'i CLI ja [Azure tellimuse sisse logida](../xplat-cli-connect.md). 

6. Veenduge, et olete ressursihaldur režiimis.

    ```
    azure config mode arm
    ```

7. Sulgeda VM, mis teil juba on eemaldatud, kasutades järgmine käsk:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Üldistada VM järgmine käsk:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Nüüd käivitada **Azure'i vm jäädvustada** käsk, mis sisaldab VM. Järgmises näites on jäädvustatud pildi VHDs nimed algavad **MyVHDNamePrefix**ja **-t** suvand määrab malli **MyTemplate.json**tee. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]Pildi VHD failid luuakse vaikimisi sama salvestusruumi kontoga, mida kasutatakse algse VM. Kasutage *sama salvestusruumi konto* talletamiseks soovitud VHDs mis tahes uue vms loomist pilt. 

6. Jäädvustatud pildi asukoha leidmiseks avage tekstiredaktoris JSON mall. Otsige **storageProfile** **uri** **pilt** asub **süsteemi** ümbrises. Näiteks URI OS plaat on sarnane`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Samm 3: Luua VM jäädvustatud pilt
Nüüd pilt malli loomiseks kasutada Linux VM. Need juhised näitavad, kuidas kasutada Azure CLI ja JSON failimalli jäädvustatud VM virtuaalse uue võrgu loomine.

### <a name="create-network-resources"></a>Võrgu ressursse loomine

Malli kasutamiseks peate esmalt oma uue VM virtuaalse võrgu- ja NIC häälestamiseks. Soovitame, et need ressursid ressursirühma loomist asukoht, kuhu on talletatud teie VM pilt. Käskude käitamine sarnane järgmised, asendav nimesid oma ressursse ja sobiv Azure asukoht ("centralus" need käsud):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>Saada NIC ID.

JSON, salvestatud ajal jäädvustada abil pildilt VM juurutamiseks peate NIC. ID-d Saamiseks käivitage järgmine käsk:

    azure network nic show MyResourceGroup1 myNIC

**Id** väljund on sarnane`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Luua VM
Nüüd käivitage järgmine käsk luua oma VM VM pilti. **-F** parameetri abil saate määrata salvestatud malli JSON-faili tee.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

Käsu väljund, küsitakse VM uus nimi, administraatori kasutajanime ja parooli ja varem loodud NIC ID-d.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

Järgmises näites on kuvatud, mida näete eduka juurutamise.

    + Malli konfiguratsioone ja parameetrite lähtestamine
    + Loomise juurutamise teave: loodud malli juurutamise xxxxxxx
    + Juurutuse lõpuleviimiseks andmete ootamine: DeploymentName: MyDeployment andmed: ResourceGroupName: MyResourceGroup1 andmed: ProvisioningState: õnnestus andmete: ajatempli: xxxxxxx andmete: režiimi: suureneva andmete: nimi tüüp väärtus


    data:    ------------------  ------------  -------------------------------------

    andmed: vmName stringi myNewVM


    andmed: vmSize stringi Standard_D1


    andmed: adminUserName stringi myAdminuser


    andmed: adminPassword SecureString määratlemata


    andmed: networkInterfaceId stringi /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic teave: rühma juurutamise loomine nupp OK

### <a name="verify-the-deployment"></a>Juurutamise kontrollimine

Nüüd SSH virtuaalse masina loodud kinnitamiseks juurutus- ja uue VM kasutuselevõtt. SSH ühenduse leida IP-aadress VM, mille lõite käivitage järgmine käsk:

    azure network public-ip show MyResourceGroup1 myIP

Käsu väljund on loetletud avaliku IP-aadressi. Vaikimisi saate ühendada Linux VM SSH port 22 järgi.

## <a name="create-additional-vms"></a>Täiendavad VMs loomine
Jäädvustatud pildi ja malli abil saate juurutada täiendavaid VMs, kasutades eelmises jaotises kirjeldatud juhiseid. Muude suvandite luua VMs pildi sisaldavad Kiirjuhend malli abil või käsu **Azure'i vm loomine** .

### <a name="use-the-captured-template"></a>Jäädvustatud malli kasutamine

Jäädvustatud pildi ja malli kasutamiseks toimige järgmiselt (üksikasjalik eelmises jaotises)

* Veenduge, et teie VM pilt on sama mis hostib teie VM VHD salvestusruumi kontole.
* Malli JSON-failist koopia ja määrake OS ketas uue VM VHD (või VHDs) kordumatu nimi. Näiteks on **storageProfile**, klõpsake jaotises **vhd**, **uri**, määrata **tavalise nimetusega osDisk** VHD sarnane kordumatu nimi`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Sama või muu virtuaalse võrgu loomine Võrguadapter.
* Juurutamine muudetud malli JSON-faili abil, mille seadistasite virtuaalse võrgu ressursirühma luua.

### <a name="use-a-quickstart-template"></a>Kiirjuhend malli kasutamine

Kui soovite võrgu häälestamine automaatselt kui loote VM pilt, saate nende ressursside malli. Näiteks vt GitHub [101-vm-kaudu – kasutaja-image malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) . See mall loob VM kohandatud pilt ja vajalikud virtuaalse võrgu, avaliku IP-aadress ja NIC ressursid. Ülevaadet Azure'i portaalis malli abil, vaadake, [Kuidas luua virtuaalse masina kohandatud pildil oleva ressursihaldur malli abil](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Azure'i vm loomiseks käsu

Tavaliselt on kõige lihtsam ressursihaldur malli abil saate luua VM pilt. Siiski saate selle VM _kindlasti_ **– Q** **Azure'i vm loomine** käsu abil luua (**--pilt-urn**) parameeter. Kui seda meetodit kasutada ka edasi **-d** (**--os-kettale-vhd**) parameetri uue VM OS .vhd faili asukoha määramiseks. See fail on vhds ümbrises salvestusruumi konto pilt VHD faili talletuskoht. Käsk kopeerib selle VHD automaatselt-ümbrisest **vhds** uue VM.

Enne käivitamist **Azure'i vm loomine** pildiga, tehke järgmist:

1.  Ressursi rühma loomine või olemasoleva ressursi rühma kasutuselevõtuks tuvastamine.

2.  Loo uus VM avaliku IP-aadressi ressursi ja NIC ressursi. Luua virtuaalse võrgu, avaliku IP-aadressi, ja NIC CLI abil, leiate käesoleva artikli. (**Azure'i vm loomine** saate luua ka Võrguadapter, kuid tuleb läbida täiendavad parameetrid virtuaalse võrgu ja alamvõrgu.)


Käivitage käsk, mille edastab URI-d nii uue OS VHD faili ja olemasoleva pildi. Selles näites suurus Standard_A1 VM luuakse ala Ida-US.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Täiendavad käsk Suvandid, käivitage `azure help vm create`.

## <a name="next-steps"></a>Järgmised sammud

Oma VMs CLI haldamise kohta leiate teemast tööülesannete [Deploy ja Azure ressursihaldur mallide ja CLI Azure'i virtuaalmasinates haldamine](virtual-machines-linux-cli-deploy-templates.md).
