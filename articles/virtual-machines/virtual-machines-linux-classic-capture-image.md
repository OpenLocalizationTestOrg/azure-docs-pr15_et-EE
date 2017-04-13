<properties
    pageTitle="Pildi Linux VM | Microsoft Azure'i"
    description="Saate teada, kuidas Linux-põhine Azure virtuaalse masina (VM) loodud mudeliga klassikaline juurutamise pildistamiseks."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Kuidas jäädvustada pildina klassikaline Linux virtuaalse masina

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](virtual-machines-linux-capture-image.md).

Selles artiklis kirjeldatakse, kuidas jäädvustada klassikaline Azure virtuaalse masina töötab Linux loomiseks muud virtuaalmasinates pilt. Sellel pildil sisaldab OS kettale ja andmete ketast virtual masina külge. See ei sisalda võrgu konfiguratsioonist, seega tuleb teil konfigureerida, mis muud virtuaalmasinates loomisel pilt.

Azure'i salvestab **pildid**, klõpsake jaotises pilt koos üleslaadimist pilte. Piltide kohta leiate lisateavet teemast [Kohta virtuaalse masina pilte Azure] [].

## <a name="before-you-begin"></a>Enne alustamist

Nende juhiste korral eeldatakse, et olete juba loonud mõne Azure virtuaalse masina klassikaline juurutamise mudeli ja konfigureeritud operatsioonisüsteemi, sh manustamise mis tahes andmete ketast. Kui teil on vaja luua VM, lugege [Linux virtuaalse masina loomise kohta] [].


## <a name="capture-the-virtual-machine"></a>Jäädvustada virtuaalse masina

1. [Ühenduse loomine virtuaalse masina](virtual-machines-linux-mac-create-ssh-keys.md) abil SSH kliendi oma valik.

2. Klõpsake aknas SSH tippige järgmine käsk. Väljund `waagent` võivad veidi erineda sõltuvalt selle kasuliku versioon:

    `sudo waagent -deprovision+user`

    See käsk proovib eemaldada süsteemist ja veenduge, et see sobib reprovisioning. See toiming hõlmab järgmisi toiminguid:

    - Eemaldab SSH võtmed (kui Provisioning.RegenerateSshHostKeyPair on konfiguratsioonifailis "y")
    - Kustutab /etc/resolv.conf nimeserveri konfigureerimine
    - Eemaldab selle `root` kasutaja parooli/jne/vari (kui Provisioning.DeleteRootPassword on konfiguratsioonifailis "y")
    - Eemaldab vahemälus talletatud DHCP klient liisingute
    - Lähtestab hostinime localhost.localdomain
    - Kustutab viimase ettevalmistatud kasutaja konto (saadud /var/lib/waagent) **ja seotud andmed**.

    >[AZURE.NOTE] Deprovisioning kustutab failide ja andmete "üldistada" pilt. Käivitage see käsk ainult virtual arvutisse, mida kavatsete jäädvustada uue pildi mallina. Ei garanteeri, et pilt on tühi, tundlikku teavet või sobib korduvalt kolmandatele isikutele.


3. Tippige **y** jätkata. Saate lisada soovitud `-force` parameetri kinnituse selle juhise vältimiseks.

4. Tippige **välju** SSH kliendi sulgemiseks.

    >[AZURE.NOTE] Ülejäänud juhistes eeldatakse, et teil on juba [installitud Azure'i CLI](../xplat-cli-install.md) klientarvutis. [Azure'i klassikaline portaali] []saab teha ka järgmised toimingud.

5. Avage teie klientarvutist Azure'i CLI ja Azure tellimuse sisse logida. Lisateabe saamiseks vt [ühenduse loomine: Azure'i CLI Azure tellimuse](../xplat-cli-connect.md).

6. Veenduge, et olete Teenusehaldus režiimis.

    `azure config mode asm`

7. Virtuaalne arvuti, mis on juba eemaldatud eelmistes toimingutes koos sulgeda:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] Saate teada, kasutades teie tellimus loodud virtuaalmasinates`azure vm list`

8. Kui virtuaalse masina lõpetatakse, jäädvustada käsk Pilt:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Tippige _uue pildi nime_asemel soovitud pildi nimi. See käsk loob generalized OS pilt. Funktsiooni `-t` soovitada kustutab algse virtuaalse masina.

9.  Uus pilt on nüüd saadaval pilte, mida saab kasutada mis tahes uue virtuaalmasinates konfigureerimiseks loendit. Saate vaadata selle käsu abil:

    `azure vm image list`

    [Azure'i klassikaline portaali] [], kuvatakse see loendis **pildid** .

    ![Pildi jäädvustada eduka](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Järgmised sammud
Pilt on valmis virtuaalmasinates loomiseks kasutada. Saate kasutada Azure CLI käsk `azure vm create` ja pildi nime, mille lõite. Käsk üksikasjad leiate [Azure'i CLI klassikaline juurutamise mudeli kasutamine](../virtual-machines-command-line-tools.md) . Teine võimalus luua kohandatud virtuaalse masina meetodil **Galeriist** ja valige pilt, mille lõite [Azure klassikaline portaali] [] abil. Lisateabe saamiseks vaadake, [Kuidas luua kohandatud virtuaalse masina] [] .

**Vt ka:** [Azure'i Linux Agent kasutusjuhend](virtual-machines-linux-agent-user-guide.md)

[Azure'i klassikaline portaal]: http://manage.windowsazure.com
[Azure virtuaalse masina piltide kohta]: virtual-machines-linux-classic-about-images.md
[Kuidas luua kohandatud virtuaalse masina]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Kuidas luua Linux virtuaalse masina]: virtual-machines-linux-classic-create-custom.md
