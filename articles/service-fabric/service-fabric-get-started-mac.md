<properties
   pageTitle="Häälestamine operatsioonisüsteemis Mac OS X oma arenduskeskkond | Microsoft Azure'i"
   description="Installige käitusaja, SDK ja tööriistad ja loomine kohaliku arengu kobar. Pärast selle setup, saab luua rakendusi operatsioonisüsteemis Mac OS X valmis."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Häälestamine operatsioonisüsteemis Mac OS X oma arenduskeskkond

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Teenuse struktuuri rakenduste Linux kogumite Mac OS X abil saate koostada. Selles artiklis antakse ülevaade häälestamine Mac-arvutis arengu.

## <a name="prerequisites"></a>Eeltingimused

Teenuse struktuuri ei tööta opsüsteemi OS X algupäraselt. Kohalik teenuse struktuuri kobar käivitamiseks pakume eelnevalt konfigureeritud Ubuntu virtuaalse masina Vagrant ja VirtualBox abil. Enne alustamist peate:

- [Vagrant (v1.8.4 või uuem versioon)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Kohaliku VM loomine

Kohaliku VM, mis sisaldab 5-sõlm teenuse struktuuri kobar loomiseks tehke järgmist.

1. Klooni Vagrantfile repo

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Liikuge soovitud Repo kohaliku klooni

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Valikuline) VM vaikesätete muutmine

    Vaikimisi konfigureeritud kohaliku VM järgmiselt:

    - 3 GB mälu eraldatud
    - Privaatne hosti võrgu konfigureeritud IP 192.168.50.50 passthrough Host Mac-liikluse lubamine

    Saate muuta neid sätteid või muude konfiguratsiooni lisamine soovitud Vagrantfile VM. Vaadake [Vagrant dokumentatsiooni](http://www.vagrantup.com/docs) konfiguratsiooni suvandid täieliku loendi.

4. VM loomine

    ```bash
    vagrant up
    ```

    Selles etapis tuleb alla laadida eelkonfigureeritud VM pilt see kohalikult ja seejärel häälestamine kohaliku teenuse struktuuri klaster see buutimine. Peaksite seda tegema paar minutit. Kui häälestamise lõpule jõudnud edukalt, kuvatakse sõnumi väljund, mis näitab klaster käivitamine.

    ![Klaster häälestamise alustamist jälgimise VM ettevalmistamise][cluster-setup-script]

5. Test, mis klaster on seadistatud õigesti navigeerides teenuse struktuuri Explorer http://192.168.50.50:19080/Explorer (eeldades, et te säilitatakse vaikimisi privaatvõrgu IP).

    ![Teenuse struktuuri Explorer vaadata Host Mac-arvutisse][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Teenuse struktuuri lisandmooduli installimine Eclipse Neon (valikuline)

Teenuse struktuuri pakub mõne lisandmooduli Eclipse neoon IDE, mida saate lihtsustada koostamise ja Java teenuste juurutamine.

1. Eclipse, veenduge, et teil on Buildship versioon 1.0.17 või uuem versioon on installitud. Saate kontrollida versiooni installitud komponendid, valides **Spikker > installimisjuhised Alustusjuhendi**. Buildship juhiste abil saate värskendada [siin][buildship-update].

2. Teenuse struktuuri lisandmooduli installimiseks valige **Spikker > installi uus tarkvara...**

3. Sisestage tekstiväljale "Töötamine": http://dl.windowsazure.com/eclipse/servicefabric.

4. Klõpsake nuppu Lisa.

    ![Eclipse neoon lisandmooduli jaoks teenuse struktuuri][sf-eclipse-plugin-install]

5. Valige teenuse struktuuri lisandmoodul ja klõpsake nuppu edasi.

6. Jätkake, kuni installimine ja nõustuge litsentsilepinguga lõppkasutaja.

## <a name="next-steps"></a>Järgmised sammud

- [Teenuse struktuuri esimese Linuxi rakenduse loomine](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Teenuse struktuuri kobar Azure portaali loomine](service-fabric-cluster-creation-via-portal.md)
- [Teenuse struktuuri kobar, kasutades Azure ressursihaldur loomine](service-fabric-cluster-creation-via-arm.md)
- [Teenuse struktuuri rakenduse mudel mõistmine](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
