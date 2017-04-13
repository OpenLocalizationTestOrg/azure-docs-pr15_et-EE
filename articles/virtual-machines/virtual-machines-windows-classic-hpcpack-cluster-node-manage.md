<properties
 pageTitle="Hallata HPC Pack kobar Arvuta sõlmed | Microsoft Azure'i"
 description="Lisateavet PowerShelli skripti tööriistad lisamine, eemaldamine, käivitamine ja peatamine HPC Pack kobar Arvuta sõlmed Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Arvu ja kättesaadavus on HPC Pack klaster Azure Arvuta sõlmed haldamine

Kui olete loonud mõne HPC Pack kobar Azure VM, võiksite hõlpsalt lisada, eemaldada, (sätted) käivitamine või peatamine (deprovision) arvu Arvuta sõlm VMs klaster viisid. Nende toimingute tegemiseks käivitage Azure PowerShelli skripti, mis installitakse pea sõlme VM. Nende skriptide aitavad teil määrata arv ja teie HPC Pack kobar ressursside nii, et saate määrata kulude.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Eeltingimused

* **Azure'i VM HPC Pack kobar** - vähemalt abil luua ka HPC Pack kobar klassikaline juurutamise mudeli HPC Pack 2012 R2 Update 1. Näiteks saate praeguse HPC Pack VM pildi Azure'i turuplatsi ja on Azure PowerShelli skripti abil automatiseerida juurutamise. Teavet ja eeltingimused, leiate teemast [loomine mõne HPC kobar HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Pärast juurutuse leida sõlme haldus skriptide % KRMS\_kausta Avaleht % pea sõlme prügikast. Peate käivitama iga skriptide administraatorina.

* **Azure'i avalda sätete fail või halduse serdi** - peate tegema järgmist pea sõlme:

    * **Sätete faili importimine on Azure avaldada**. Selle tegemiseks käivitage pea sõlme järgmised Azure PowerShelli cmdlet-käsud:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Azure'i halduse serdi pea sõlme konfigureerimine**. Kui teil on CER-fail, importige see poes CurrentUser\My serti ja seejärel käivitage järgmine Azure PowerShelli cmdlet-käsk Azure keskkonnas (AzureCloud või AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>MSDN VMs lisamine

Arvuta sõlmed **Lisa-HpcIaaSNode.ps1** skripti lisada.

### <a name="syntax"></a>Süntaks
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parameetrid

* **Teenuse nimi** – pilveteenuses nime teenuse selle uue MSDN VMs lisatakse.

* **ImageName** - Azure VM pildi nimi, mida on võimalik saada Azure klassikaline portaali või Azure PowerShelli cmdlet-käsk **Get-AzureVMImage**. Pilt peab vastama järgmistele nõuetele.

    1. Windowsi operatsioonisüsteemi peab olema installitud.

    2. HPC Pack peab olema installitud Arvuta sõlm roll.

    3. Pilt peab olema privaatne pilt kategoorias kasutaja pole avaliku Azure VM pilt.

* **Arvu** - arv MSDN VMs lisada.

* **InstanceSize** - MSDN VMs suurust.

* **DomainUserName** - domeeni kasutajanimi, mida kasutatakse liituda uue VMs domeeni.

* **DomainUserPassword** - domeeni kasutaja parooli.

* **NodeNameSeries** (valikuline) - failinimede mustri Arvuta sõlmed. Vorming peab olema &lt; *juursertimisprogrammi\_nimi*&gt;&lt;*käivitamine\_arvu*&gt;%. Näiteks MyCN % 10% tähendab sari, alates MyCN11 Arvuta sõlm nimed. Kui pole määratud, kasutatakse skripti nimetamine sarja HPC klaster konfigureeritud sõlme.

### <a name="example"></a>Näide

Järgmises näites lisab 20 suurus suur MSDN VMs on pilvepõhise teenuse *hpcservice1*, VM pilt *hpccnimage1*põhjal.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>MSDN VMs eemaldamine

Arvuta sõlmed **Eemalda-HpcIaaSNode.ps1** skripti eemaldada.

### <a name="syntax"></a>Süntaks

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parameetrid

* **Nimi** – kobar sõlmed nimed eemaldada. Metamärkide on toetatud. Parameetri nimi on nimi. Te ei saa määrata **nime** ja **sõlm** parameetrid.

* **Sõlm** - The HpcNode objekti sõlmed eemaldada, mida saab HPC PowerShelli cmdlet-käsk [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)kaudu. Selle parameetri nimi ei sõlm. Te ei saa määrata **nime** ja **sõlm** parameetrid.

* **DeleteVHD** (valikuline) - säte kustutamine seotud ketast vms, mis on eemaldatud.

* **Jõusta** (valikuline) – et HPC sõlmed ühenduseta enne nende eemaldamist.

* **Kinnitage** (valikuline) – Küsi enne käsu andmist.

* **WhatIf** - kirjeldamiseks, mis juhtub, kui te täitmiseks käsu tegelikult käsu seadmine.

### <a name="example"></a>Näide

Järgmises näites jõustab ühenduseta sõlmed algab nimedega *HPCNode-CN -* ja nende eemaldab sõlmed ja nende seotud ketast.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>MSDN VMs käivitamine

Alustage Arvuta sõlmed **Algus-HpcIaaSNode.ps1** skripti.

### <a name="syntax"></a>Süntaks

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parameetrid

* **Nimi** – nimed kobar sõlmed alustada. Metamärkide on toetatud. Parameetri nimi on nimi. Te ei saa määrata **nime** ja **sõlm** parameetrid.

* **Sõlm**- The HpcNode objekti sõlmed alustada, mida saab HPC PowerShelli cmdlet-käsk [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)kaudu. Selle parameetri nimi ei sõlm. Te ei saa määrata **nime** ja **sõlm** parameetrid.

### <a name="example"></a>Näide

Järgmises näites alustab sõlmed nimed, mis algab *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>MSDN VMs peatamine

Arvuta sõlmed **Peata-HpcIaaSNode.ps1** skripti lõpetada.

### <a name="syntax"></a>Süntaks

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parameetrid


* **Nimi**– nimed kobar sõlmed lõpetada. Metamärkide on toetatud. Parameetri nimi on nimi. Te ei saa määrata **nime** ja **sõlm** parameetrid.

* **Sõlm** - The HpcNode objekti sõlmed lõpetada, mida saab HPC PowerShelli cmdlet-käsk [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)kaudu. Selle parameetri nimi ei sõlm. Te ei saa määrata **nime** ja **sõlm** parameetrid.

* **Jõusta** (valikuline) – et enne lõpetamist need HPC sõlmed ühenduseta.

### <a name="example"></a>Näide

Järgmises näites jõustab ühenduseta sõlmed algab nimedega *HPCNode-CN -* ja seejärel lõpetab sõlmed.

Lõpeta – HPCIaaSNode.ps1 – nimi HPCNodeCN-*-Jõusta

## <a name="next-steps"></a>Järgmised sammud

* Kui soovite automaatselt kasvata või Kahanda kobar sõlmed vastavalt praeguse töökoormus töökohtade ja tööülesannete klaster võimalus, lugege teemat [automaatselt kasvada ja vähendage vastavalt kobar töökoormus Azure HPC Pack kobar ressursside](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
