<properties
   pageTitle="Luua esimese teenuse struktuuri rakenduse Linux kasutamine C# | Microsoft Azure'i"
   description="Luua ja juurutada teenuse struktuuri kasutav C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Esimese Azure teenuse struktuuri rakenduse loomine

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Teenuse struktuuri pakub koostamise teenuste Linux .NET Core nii Java SDK-d. Selles õpetuses vaatame, kuidas luua rakenduse Linuxi ja koostada teenuse C# (.NET tuum).

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist veenduge, et teil on [oma Linux arenduskeskkond häälestamine](service-fabric-get-started-linux.md). Kui kasutate opsüsteemi Mac OS X, saate [häälestada Linux ühe-karbi keskkonnas virtuaalse masina Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Rakenduse loomine

Teenuse struktuuri rakendus võib sisaldada ühe või mitme teenuseid, kindla rolliga pakkuda rakenduse funktsioone. Teenuse struktuuri SDK Linuxi sisaldab [Yeoman](http://yeoman.io/) generaator, mis on lihtne loomiseks esimese teenust ja veel hiljem lisada. Kasutame Yeoman ühe teenuse rakenduse loomiseks.

1. Terminalis, tippige järgmine käsk alustada hoone tellingud:`yo azuresfcsharp`

2. Rakenduse nimi.

3. Valige oma esimese teenuse tüüp ja nime. Selleks et selles õpetuses, valisime näitleja töökindlat.

  ![Teenuse struktuuri Yeoman generaator C#][sf-yeoman]

>[AZURE.NOTE] Suvandite kohta leiate lisateavet teemast [teenuse struktuuri programmeerimise mudeli ülevaade](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Rakenduse koostamine

Teenuse struktuuri Yeoman mallid sisaldavad Koosta skripti, mille abil saate luua rakenduse kaudu terminal (pärast liikumine kausta).

  ```bash
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>Rakenduse juurutamine

Kui rakendus on ehitatud, peate selle juurutama kohaliku klaster Azure'i CLI abil.

1. Ühenduse loomine kohaliku teenuse struktuuri kobar.

    ```bash
    azure servicefabric cluster connect
    ```

2. Installi skripti esitatud malli abil saate kopeerida soovitud klaster pilt poest rakenduse pakett, registreerida rakenduse tüüp ja rakenduse eksemplari loomine.

    ```bash
    ./install.sh
    ```

3. Avage brauser ja liikuge teenuse struktuuri Exploreri veebisaidil http://localhost:19080/Explorer (Asenda localhost privaatne IP VM kui Vagrant abil operatsioonisüsteemis Mac OS X).

4. Laiendage rakenduste ja pange tähele, et nüüd kirje oma rakenduse tüüp ja teine kõigepealt seda tüüpi.

## <a name="start-the-test-client-and-perform-a-failover"></a>Käivitage test klient ja teha Tõrkesiirde

Näitleja projektide ei tee midagi oma. Nende jaoks on vaja mõne muu teenuse või kliendi sõnumite saatmiseks. Näitleja Mall sisaldab lihtne test skripti, mille abil saate suhelda näitleja teenuse.

1. Käivitage skript abil vaadake kasuliku väljundi näitleja teenuse kuvamiseks.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Teenuse struktuuri Exploreris otsimiseks majutusteenuse näitleja teenuse peamine koopia. Pildil on sõlm 3.

    ![Peamine koopia teenuse struktuuri Exploreris otsimine][sfx-primary]

3. Klõpsake eelmises etapis leiate sõlme, siis valige menüü toimingud **Desaktiveeri (uuesti)** . See toiming taaskäivitamist üks viis sõlmed kohaliku klaster sunnib Tõrkesiirde töötavate teise sõlme teise koopia. Kui selle toimingu tähelepanu väljund testi klient ja pange tähele, et näidiku jätkuvalt vaatamata on tõrkesiire inkrementida.


## <a name="next-steps"></a>Järgmised sammud

- [Lisateavet usaldusväärne osalejate kohta](service-fabric-reliable-actors-introduction.md)
- [Teenuse struktuuri kogumite Azure'i CLI kaudu suhtlemiseks](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
