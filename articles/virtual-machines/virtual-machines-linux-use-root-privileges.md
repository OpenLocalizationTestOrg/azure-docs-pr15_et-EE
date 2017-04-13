<properties 
    pageTitle="Kasutage administraatoriõigusi Linuxi | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada administraatoriõigusi Linux virtuaalse masina Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Linux virtuaalmasinates Azure administraatoriõigusi kasutamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Vaikimisi on `root` Linux virtuaalmasinates Azure kasutaja on keelatud. Kasutajad saavad käivitada käsud administraatoriõigustega, kasutades funktsiooni `sudo` käsk. Siiski kogemusi võivad erineda sõltuvalt sellest, kuidas süsteem on ette valmistatud.

1. **SSH võti ja parooli OR parooli ainult** - virtuaalse masina ette valmistatud kas sertifikaadiga (`.CER` faili) või SSH võti kui ka parooli, või ainult kasutajanime ja parooli. Sel juhul `sudo` küsib kasutaja parooli enne käsu andmist.

2. **Ainult SSH võti** - virtuaalse masina on ette valmistatud sertifikaadiga (`.cer`, `.pem`, või `.pub` faili) või SSH võti, kuid ei parool.  Sel juhul `sudo` **ei** Küsi enne selle käsu kasutaja parooli.


## <a name="ssh-key-and-password-or-password-only"></a>SSH võti ja parooli või parooli ainult

SSH võti või parool autentimist kasutades Linux virtuaalse masina sisse logida ja seejärel käivitage käskude abil `sudo`, näiteks:

    # sudo <command>
    [sudo] password for azureuser:

Kui küsitakse kasutaja parooli. Pärast parooli sisestamist `sudo` käivitab käsu koos `root` õigusi.

Saate lubada passwordless sudo, redigeerides funktsiooni `/etc/sudoers.d/waagent` faili, näiteks:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

See muudatus võimaldab passwordless sudo kasutaja "azureuser".

## <a name="ssh-key-only"></a>SSH võti ainult

Logige Linux virtuaalse masina SSH võtme autentimist ja seejärel käivitage käskude abil `sudo`, näiteks:

    # sudo <command>

Kui kasutaja on **pole** teil paluda parooli. Pärast vajutades `<enter>`, `sudo` käivitab käsu koos `root` õigusi.

 
