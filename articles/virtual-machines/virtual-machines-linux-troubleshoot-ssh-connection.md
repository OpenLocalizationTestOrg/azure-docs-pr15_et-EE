<properties
    pageTitle="SSH VM ühendusprobleemide tõrkeotsing | Microsoft Azure'i"
    description="Kuidas tõrkeotsing nagu "SSH ühendus nurjus" või "SSH ühendus tagasi lükatud" Azure VM, mis töötab Linux."
    keywords="SSH ühendus keeldus, ssh viga, Azure'i ssh, SSH ühendus katkes"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Azure'i Linux VM, nurjub, tõrkeid, või on tagasi lükatud SSH ühenduste tõrkeotsing
On erinevad põhjused, et Secure Shell (SSH) tõrgete SSH ühenduse tõrkeid, või SSH on tagasi lükatud, kui proovite luua ühenduse Linux virtuaalse masina (VM). See artikkel aitab teil leida ja parandada probleeme. Azure'i portaalis Azure CLI või VM Accessi laiend Linuxi abil saate ühendusega seotud probleemide lahendamiseks ja tõrkeotsing.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Kui vajate rohkem abi, mis tahes hetkel selle artikli, võtke ühendust [MSDN-i Azure](http://azure.microsoft.com/support/forums/)ja virnas ületäitumise Foorumid Azure eksperdid. Teise võimalusena saate faili Azure'i tugiteenuse taotluse. Minge [Azure tugiteenuste sait](http://azure.microsoft.com/support/options/) ja valige **tootetugi**. Azure tugiteenuste kasutamise kohta lisateavet, lugege [Microsoft Azure'i toetavad FAQ](http://azure.microsoft.com/support/faq/).


## <a name="quick-troubleshooting-steps"></a>Tõrkeotsingu kiirtoimingud
Tõrkeotsingu iga toimingu järel proovige uuesti, et VM.

1. Lähtesta SSH konfigureerimine.
2. Lähtestage kasutaja mandaat.
3. Veenduge, et [Võrgu turberühma](../virtual-network/virtual-networks-nsg.md) reeglid lubavad SSH liikluse.
    - Veenduge, et SSH liikluse (vaikimisi TCP port 22) on olemas võrgu turberühma reegli.
    - Te ei saa kasutada pordi ümbersuunamise / kaardistamine on Azure laadi koormusetasakaalustusteenuse kasutamata.
4. [VM ressursi seisundi](../resource-health/resource-health-overview.md)kontrollimine. 
    - Veenduge, et VM aruanded nimega on terve.
    - Kui teil on lubatud buutimine diagnostika, kontrollige VM on aru, buutimine tõrgete logid.
5. Taaskäivitage VM.
6. Juurutage uuesti VM.

Edasi lugedes veel tõrkeotsingujuhiseid ja selgitused.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Saadaval võimalusi SSH ühenduse probleemide tõrkeotsing

Saate lähtestada identimisteabe või SSH konfiguratsiooni, kasutades ühte järgmistest:

- [Azure portaali](#using-the-azure-portal) - hästi, kui teil on vaja kiiresti lähtestamine SSH konfiguratsiooni või SSH võti ja te pole installitud Azure tööriistu.
- [Azure'i CLI käsud](#using-the-azure-cli) – kui olete juba käsureal, kiiresti lähtestada SSH konfiguratsiooni või mandaat.
- [Azure'i VMAccessForLinux laiend](#using-the-vmaccess-extension) - loomine ja json määratlus failid lähtestada SSH konfiguratsiooni või kasutaja mandaat uuesti kasutada.

Tõrkeotsingu iga toimingu järel proovige oma VM uuesti ühendust luua. Kui te ikka ei saa ühendust luua, proovige järgmist toimingut.


## <a name="using-the-azure-portal"></a>Azure'i portaalis
Azure'i portaalis kiiresti lähtestamiseks SSH konfiguratsiooni või kasutaja mandaat ilma tööriistu kohalikku arvutisse installida.

Valige oma VM Azure portaali. Liikuge kerides jaotiseni **tugi + tõrkeotsing** ja valige **parooli lähtestada** , nagu järgmises näites:

![Lähtesta SSH konfiguratsiooni või õige mandaat Azure'i portaalis](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>Lähtesta SSH konfigureerimine
Esimese sammuna valige `Reset SSH configuration only` **režiimi** rippmenüüst menüü nagu eelmisel pildil, seejärel klõpsake nuppu **Lähtesta** . Kui see toiming on lõpule jõudnud, proovige uuesti oma VM avamiseks.

### <a name="reset-ssh-credentials-for-a-user"></a>Lähtestage kasutaja mandaat SSH
Olemasoleva kasutaja mandaat, valige kas `Reset SSH public key` või `Reset password` nagu eelmisel pildil **režiimi** rippmenüü kaudu. Määrake kasutajanimi ja SSH võti või uus parool ja seejärel klõpsake nuppu **Lähtesta** .

Saate luua kasutaja sudo privileegidega VM selle menüü kaudu. Sisestage uue kasutajanime ja parooli seotud või SSH võti ja seejärel klõpsake nuppu **Lähtesta** .


## <a name="using-the-azure-cli"></a>Azure'i CLI abil
Kui te pole seda veel teinud, [installige Azure'i CLI ja Azure tellimuse ühendada](../xplat-cli-install.md). Veenduge, et te kasutate ressursihaldur režiimi järgmiselt:

```
azure config mode arm
```

Kui olete loonud ja üles laadida kohandatud Linuxi plaat, veenduge, et [Microsoft Azure Linuxi Agent](virtual-machines-linux-agent-user-guide.md) versioon 2.0.5 või uuem versioon on installitud. Galerii piltide abil loodud vms, on juurdepääs sellele laiendile juba installinud ja konfigureerinud teie eest.

### <a name="reset-ssh-configuration"></a>Lähtesta SSH konfigureerimine
Ise konfiguratsiooni SSHD võib olla valesti konfigureeritud või teenuse ilmnes tõrge. Saate lähtestada SSHD veendumaks, et ise konfiguratsiooni SSH on lubatud. Lähtestamise SSHD peaks olema tõrkeotsingu kõigepealt võtmist.

Järgmises näites taastab SSHD kohta nimega VM `myVM` ressursirühma nimega `myResourceGroup`. Kasutage oma VM ja ressursside rühma nimed järgmiselt:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Lähtestage kasutaja mandaat SSH
Kui SSHD kuvatakse korralikult, saate tööandja kasutaja parooli lähtestada. Järgmises näites taastab mandaadi `myUsername` määratud väärtusele `myPassword`, nimega VM `myVM` sisse `myResourceGroup`. Kasutage oma väärtused järgmiselt:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Kui SSH võtme autentimist kasutades, saate lähtestada võti SSH konkreetse kasutaja. Järgmises näites värskendab SSH võti talletatud `~/.ssh/azure_id_rsa.pub` kasutajale nimega `myUsername`, nimega VM `myVM` sisse `myResourceGroup`. Kasutage oma väärtused järgmiselt:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>Kasutades VMAccess laiend
VM Accessi laiend Linuxi loeb json-failis, mis määratleb toimingute teostamiseks. Neid toiminguid kaasata lähtestamise SSHD, on SSH võti lähtestamine või kasutaja lisamine. Saate siiski kasutada Azure CLI helistamiseks VMAccess laiend, kuid te saate kasutada json failid üle mitme VMs, kui soovitud. Seda moodust võimaldab teil luua hoidla json-faile, mis seejärel kutsuda antud stsenaariumid.

### <a name="reset-sshd"></a>SSHD lähtestamine
Looge fail nimega `PrivateConf.json` sisu on järgmine:

```bash
{  
    "reset_ssh":"True"
}
```

Azure'i CLI abil, siis helistate funktsiooni `VMAccessForLinux` lähtestamiseks ühendust SSHD määramisega json faili laiend. Järgmises näites taastab SSHD nimega VM `myVM` sisse `myResourceGroup`. Kasutage oma väärtused järgmiselt:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Lähtestage kasutaja mandaat SSH
Kui SSHD kuvatakse korralikult, saate lähtestada tööandja kasutaja mandaat. Kasutaja parooli lähtestamiseks looge fail nimega `PrivateConf.json`. Järgmises näites taastab mandaadi `myUsername` määratud väärtusele `myPassword`. Sisestage järgmised read sisse oma `PrivateConf.json` faili, kasutades oma väärtusi:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Või lähtestage kasutaja SSH võti, esmalt looge fail nimega `PrivateConf.json`. Järgmises näites taastab mandaadi `myUsername` määratud väärtusele `myPassword`, nimega VM `myVM` sisse `myResourceGroup`. Sisestage järgmised read sisse oma `PrivateConf.json` faili, kasutades oma väärtusi:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Pärast json faili loomist Azure'i CLI helistamiseks kasutada funktsiooni `VMAccessForLinux` lähtestada oma SSH kasutajatunnust, määrates json faili laiend. Järgmises näites taastab mandaat nimega VM `myVM` sisse `myResourceGroup`. Kasutage oma väärtused järgmiselt:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Taaskäivitage VM
Kui teil on SSH konfiguratsiooni ja kasutajale identimisteabe lähtestamine või ilmnes tõrge tehes, proovige taaskäivitada VM aadressi aluseks Arvuta probleemid.

### <a name="azure-portal"></a>Azure'i portaal
Azure'i portaalis VM, valige oma VM ja klõpsake soovitud ***taaskäivitamiseks** nuppu, nagu järgmises näites:

![Taaskäivitage VM Azure portaalis](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure'i CLI
Järgmises näites taaskäivitamist nimega VM `myVM` ressursirühma nimega `myResourceGroup`. Kasutage oma väärtused järgmiselt:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>VM ümberkorraldamine
Te saate ümberkorraldamine VM teise sõlme Azure'is, mis võib parandada aluseks oleva võrgu probleemidest. Ümbersuunamine VM kohta leiate teavet teemast [ümberkorraldamine virtuaalse masina uue Azure sõlme](virtual-machines-windows-redeploy-to-new-node.md).

> [AZURE.NOTE] Kui see toiming on lõpule jõudnud, efemeersed ketta andmed lähevad kaotsi ja dünaamiline IP-aadressid, mis on seotud virtuaalse masina värskendatakse.

### <a name="azure-portal"></a>Azure'i portaal
Juurutage uuesti VM Azure portaalis, valige oma VM ja liikuge kerides jaotiseni **tugi + tõrkeotsing** . Klõpsake nuppu **Juurutage uuesti** , nagu järgmises näites:

![Azure'i portaalis VM ümberkorraldamine](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure'i CLI
Järgmises näites redeploys nimega VM `myVM` ressursirühma nimega `myResourceGroup`. Kasutage oma väärtused järgmiselt:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>Klassikaline juurutamise mudeli abil loodud VMs

Proovige järgmisi toiminguid lahendamiseks vms klassikaline juurutamise mudeli abil loodud kõige levinum SSH ühenduse tõrkeid. Iga toimingu järel proovige uuesti, et VM.

- Lähtesta Kaugpöördusteenuse [Azure portaali](https://portal.azure.com). Azure'i portaalis, valige oma VM ja klõpsake nuppu **Lähtesta Remote …** .

- Taaskäivitage VM. [Azure'i portaalis](https://portal.azure.com), valige oma VM ja klõpsake **uuesti** nuppu.

    -VÕI-

    [Azure'i klassikaline portaalis](https://manage.windowsazure.com)valige **virtuaalmasinates** > **eksemplari** > **taaskäivitama**.

- Juurutage uuesti uue Azure sõlme VM. Juurutage uuesti VM kohta leiate teavet teemast [ümberkorraldamine virtuaalse masina uue Azure sõlme](virtual-machines-windows-redeploy-to-new-node.md).

    Kui see toiming on lõpule jõudnud, efemeersed ketta andmed lähevad kaotsi ja dünaamiline IP-aadressid, mis on seotud virtuaalse masina värskendatakse.

- Järgige juhiseid teemas [Kuidas lähtestada parooli või Linuxi-põhiste virtuaalmasinates SSH](virtual-machines-linux-classic-reset-access.md) :
    - Lähtestage parool või SSH võti.
    - _Sudo_ kasutajakonto loomine.
    - Lähtesta SSH konfigureerimine.

- Märkige ruut soovitud VM ressursi tervis platvormi probleemidest.<br>
  Valige oma VM ja liikuge kerides allapoole **sätted** > **Seisundi kontrollimine**.


## <a name="additional-resources"></a>Lisaressursid

- Kui olete SSH siiani õnnestunud oma VM pärast juhiste, lugege [üksikasjalikumat tõrkeotsingujuhiseid](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) täiendavad juhised probleemi lahendamiseks üle vaadata.

- Rakenduse access tõrkeotsingu kohta leiate lisateavet teemast [tõrkeotsing juurdepääsu rakenduse Azure virtuaalne arvutis töötab](virtual-machines-linux-troubleshoot-app-connection.md)

- Klassikaline juurutamise mudeli abil loodud virtuaalmasinates tõrkeotsingu kohta lisateabe saamiseks vaadake, [Kuidas lähtestada parooli või Linuxi-põhiste virtuaalmasinates SSH](virtual-machines-linux-classic-reset-access.md).
