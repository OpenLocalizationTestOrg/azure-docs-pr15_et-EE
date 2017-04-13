<properties
    pageTitle="Installige Windows VM MongoDB | Microsoft Azure'i"
    description="Saate teada, kuidas installida Azure VM, mis luuakse klassikaline juurutamise mudeliga, kus töötab Windows Server MongoDB."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>Installige Windows VM MongoDB

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Installida ja konfigureerida MongoDB ressursihaldur juurutamise näidise, lugege [artiklit](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] on populaarsed avatud lähtekoodi, suure jõudlusega NoSQL andmebaas. Selles artiklis juhendab teid luua Windows Server virtuaalse masina (VM) [Azure klassikaline portaali][AzurePortal]. Seejärel loomine ja andmete kettal manustamine enne installimist ja konfigureerimist MongoDB VM. Kui teil on mõne olemasoleva VM Azure, mida soovite kasutada, saate liikuda otse [installimist ja konfigureerimist MongoDB](#install-and-run-mongodb-on-the-virtual-machine).


## <a name="create-a-virtual-machine-running-windows-server"></a>Windows Server töötab loomine

Järgige neid juhiseid, et luua virtuaalse masina.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Saate lisada lõpp MongoDB virtuaalse masina loomisel ja konfigureerida järgmiselt: nimega **Mongo**, kasutage **TCP** protokoll ja **27017**avalike ja privaatvõ pordid määramine.

## <a name="attach-a-data-disk"></a>Andmete kettal manustamine
Salvestusruumi pakkumine virtuaalse masina, lisada andmed kettale ja seejärel lähtestada, nii et saate seda kasutada Windows. Kui teil on juba andmed kettale, saate lisada olemasolevaid ketta või saate lisada ka tühjale kettapuhastusriista.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Juhised lähtestamisel ketas, vt "kohta: uute andmete ketta Windows Server lähtestamine", [Kuidas lisada andmed kettale Windows virtuaalse masina](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Installimine ja käivitamine MongoDB virtual arvutisse

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Kokkuvõte
Selles õpetuses te teada, kuidas luua töötab Windows Server, kaugühenduse teel ühenduse loomiseks ja andmete kettal manustada.  Saite teada, kuidas installida ja konfigureerida MongoDB virtual Windowsi-põhises arvutis. Pääsete nüüd MongoDB Windowsi-põhiste virtual arvutisse, järgides täpsemalt teemade [MongoDB dokumentatsiooni][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
