<properties
    pageTitle="Azure Linux VMs abil VMAccess laiend juurdepääsu lähtestamine | Microsoft Azure'i"
    description="Lähtesta juurdepääsu Azure Linux VMs VMAccess laiend abil."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Kasutajate haldamine, SSH, ja märkige või parandada ketast Azure Linux VMs VMAccess laiend kasutamise kohta

Selles artiklis kirjeldatakse, kuidas Azure'i VMAcesss laiend abil kontrollida või parandamine kettal, lähtestamine kasutajate juurdepääsu, hallata kasutajakontosid SSHD konfiguratsiooni Linux. See artikkel nõuab tehke järgmist.

- Azure'i konto ([saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure'i CLI](../xplat-cli-install.md) sisse logitud `azure login`.

- Azure'i CLI _peab olema_ Azure'i ressursihaldur režiimi `azure config mode arm`.

## <a name="quick-commands"></a>Kiirülevaate käsud

On kaks võimalust VMAccess kasutamine oma Linux VMs.

- Kasutades Azure CLI ja parameetrid.
- Kasutades töötlemata JSON-faile, mis VMAccess töötleb ja siis tegutseda.

Kiirülevaate käsk jaotise, on meil kasutada Azure CLI `azure vm reset-access` meetod. Järgmistes näidetes käsu, asendage väärtused, mis sisaldavad "näide" väärtustega oma keskkonnas.

## <a name="create-a-resource-group-and-linux-vm"></a>Ressursirühm ja Linux VM loomine

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Luua Debian VM

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Root parooli lähtestamine

Root parooli lähtestamiseks tehke järgmist.

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH võti lähtestamine

Kasutaja-root SSH võti lähtestamiseks tehke järgmist.

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Kasutaja loomine

Kasutaja loomiseks tehke järgmist.

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Kasutaja eemaldamine

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>SSHD lähtestamine

SSHD konfiguratsiooni lähtestamiseks tehke järgmist.

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Üksikasjaliku selgituse

### <a name="vmaccess-defined"></a>Määratletud VMAccess:

Ketas, oma Linux VM on kujutatud tõrkeid. Oma Linux VM kuidagi root parooli lähtestamine või kogemata kustutanud oma SSH privaatvõti. Kui mis juhtus andmekeskuse päeva, peaksite drive seal ja avage toomiseks serveri konsooli KVM. Mõelge Azure'i VMAccess laiend selle KVM parameeter, mis võimaldab kasutada konsooli Linux juurdepääsu lähtestamine või tehke kettapuhastusriista taseme hooldustööd.

Üksikasjaliku selgituse, me VMAccess, mis kasutab töötlemata JSON failide pikk kujul kasutada.  Need VMAccess JSON-failid saate nimetatakse ka Azure mallide põhjal.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Märkige või parandada Linux VM ketas VMAccess abil

VMAccess abil saate teha mõnda fsck oma Linux VM kettal käivitada.  Saate teha ka kettale sisse ja ketas parandamine, kasutades mõnda VMAccess.

Märkige ruut ja seejärel parandamine ketas kasutage seda VMAccess skripti:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Käivita VMAccess script:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Lähtestada kasutajate juurdepääsu Linux VMAccess abil

Kui teil on juurdepääs root oma Linux VM kaotanud, võite käivitada VMAccess skripti root parool lähtestada.

Kasutage root parooli lähtestamiseks selles VMAccess skripti:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Käivita VMAccess script:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Kasutaja-root SSH võti lähtestamiseks kasutage seda VMAccess skripti:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Käivita VMAccess script:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>VMAccess abil saate hallata kasutajakontosid Linux

VMAccess on Python skript, mida saab kasutada ilma logimine ja sudo või root konto abil oma Linux VM kasutajate haldamine.

Kasutaja loomiseks kasutada seda VMAccess skripti:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Käivita VMAccess script:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Kasutaja kustutamine kasutada seda VMAccess skripti.

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Käivita VMAccess script:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>SSHD konfiguratsiooni VMAccess abil

Kui Linux VMs SSHD konfiguratsiooni muudatuste tegemine ja sulgege SSH ühendus enne muudatuste kontrollimiseks, mida ei ole SSH'ing uuesti sisse.  VMAccess saab kasutada lähtestamiseks SSHD konfiguratsiooni eduka konfigureerimise ilma sisse loginud üle SSH.

Lähtestada SSHD konfiguratsiooni kasutada seda VMAccess skripti:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Käivita VMAccess script:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Järgmised sammud

Värskendamise Linux abil Azure'i VMAccess laiendid on üks meetod töötab Linux VM muudatusi teha.  Oma Linux VM buutimine muutmiseks saate kasutada ka tööriistu, nagu pilveteenuses – käivitamise ja Azure mallid.

[Virtuaalse masina laiendid ja funktsioonide kohta](virtual-machines-linux-extensions-features.md)

[Loome Linux VM laiendiga Azure'i ressursihaldur Mallid](virtual-machines-linux-extensions-authoring-templates.md)

[Pilveteenuse – käivitamise abil saate kohandada Linux VM loomise ajal](virtual-machines-linux-using-cloud-init.md)
