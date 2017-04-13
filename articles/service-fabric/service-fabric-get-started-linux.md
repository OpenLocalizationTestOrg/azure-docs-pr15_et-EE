<properties
   pageTitle="Häälestada oma arenduskeskkond Linux | Microsoft Azure'i"
   description="Installige käitusaja ja SDK ja loomine kohaliku arengu kobar Linux. Pärast selle setup, saab luua rakendusi valmis."
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
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Teie arenduskeskkond Linux ettevalmistamine


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Juurutada ja [Azure teenuse struktuuri rakendusi](service-fabric-application-model.md) käivitada Linuxi arengu teie arvutis, installige käitusaja ja levinud SDK. Samuti saate installida Java ja .NET Core valikuline SDK-d.

## <a name="prerequisites"></a>Eeltingimused
### <a name="supported-operating-system-versions"></a>Toetatud operatsioonisüsteemide versioonidega
Arengu on toetatud järgmised operatsioonisüsteemi versiooni:

- Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Värskendage oma apt allikad

SDK ja seotud käitusaja paketi kaudu apt-get installimiseks peate esmalt värskendama apt allikate.

1. Avage terminal.
2. Teenuse struktuuri repo lisamiseks loendisse allikatest.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Saate lisada oma apt Kadakast uue GPG võti.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Värskendage oma paketi loendite äsja lisatud hoidlate põhjal.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Installimine ja häälestamine SDK

Kui allikate on värskendatud, saate installida SDK.

1. Installige teenuse struktuuri SDK pakett. Teil palutakse installimise kinnitamiseks ja nõus litsentsi lepingu.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Käivitage SDK setup skript.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Azure'i platvormidel CLI häälestamine

[Azure'i platvormidel CLI] [ azure-xplat-cli-github] sisaldab käske nendega teenuse struktuuri üksustega kogumite ja rakendused. See põhineb Node.js seega [veenduge, et olete installinud sõlm] [ install-node] enne jätkamist allpool toodud juhiseid.

1. Klooni github repo arengu teie arvutis.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Lülitage sisse kloonitud repo alla ja installige soovitud CLI sõltuvused sõlm Package Manager (npm) abil.

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Luua nimeviidana kloonitud Repo kaustast Prügikast/Azure'i /usr/bin/azure nii, et see lisatakse teie tee ja käsud on saadaval, mis tahes kataloogist.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Lõpuks luba automaatne lõpetamine teenuse struktuuri käsud.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Saate seadistada kohalikku kobar

Kui kõik on edukalt installitud, peaks oskama kohaliku kobar käivitamine.

1. Käivitage skript kobar häälestus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Avage veebibrauser ja liikuge http://localhost:19080/Explorer. Kui klaster on käivitanud, peaksite nägema teenuse struktuuri Exploreri armatuurlaud.

    ![Teenuse struktuuri Exploreri Linux][sfx-linux]

Selles etapis teil on võimalik juurutada valmismallide teenuse struktuuri rakenduse paketid või Külastajate ümbriste või Külastajate täitmisfailid põhjal uusi faile. Luua uusi teenuseid kasutades Java või .NET Core SDK-d, järgige alltoodud juhiseid valikuline häälestus.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Installimine on Java SDK ja Eclipse neoon lisandmoodul (valikuline)

Java SDK pakub teeke ja mallid, mis on vaja luua teenuse struktuuri teenuseid kasutades Java.

1. Installige Java SDK pakett.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Käivitage SDK setup skript.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Eclipse lisandmooduli installimiseks teenuse struktuuri sees Eclipse neoon IDE.

1. Eclipse, veenduge, et teil on Buildship versioon 1.0.17 või uuem versioon on installitud. Saate kontrollida versiooni installitud komponendid, valides **Spikker > installimisjuhised Alustusjuhendi**. Buildship juhiste abil saate värskendada [siin][buildship-update].

2. Teenuse struktuuri lisandmooduli installimiseks valige **Spikker > installi uus tarkvara...**

3. Sisestage tekstiväljale "Töötamine": http://dl.windowsazure.com/eclipse/servicefabric

4. Klõpsake nuppu Lisa.

    ![Eclipse lisandmoodul][sf-eclipse-plugin]

5. Valige teenuse struktuuri lisandmoodul ja klõpsake nuppu edasi.

6. Jätkake, kuni installimine ja nõustuge litsentsilepinguga lõppkasutaja.

## <a name="install-the-net-core-sdk-optional"></a>Installige .NET Core SDK (valikuline)

.NET Core SDK pakub teeke ja mallid, mis on vaja luua teenuse struktuuri teenuseid kasutades platvormidel .NET Core.

1. Installige .NET Core SDK pakett.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Käivitage SDK setup skript.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Järgmised sammud

- [Linux esimese Java rakenduse loomine](service-fabric-create-your-first-linux-application-with-java.md)

- [Teie arenduskeskkond OSX ettevalmistamine](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
