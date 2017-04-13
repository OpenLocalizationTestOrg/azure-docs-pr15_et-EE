<properties
   pageTitle="Teenuse struktuuri ja Linux juurutamisel ümbriste | Microsoft Azure'i"
   description="Teenuse struktuuri ja keskmise suurusega ümbriste kasutamine juurutada microservice rakendused. Selles artiklis kirjeldatakse võimalusi, mis pakub teenuse struktuuri ümbriste ja juurutamise üheks klaster keskmise suurusega container pilt"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Keskmise suurusega container juurutama teenuse struktuuri

> [AZURE.SELECTOR]
- [Windowsi Container juurutamine](service-fabric-deploy-container.md)
- [Keskmise suurusega Container juurutamine](service-fabric-deploy-container-linux.md)

Selles artiklis tutvustatakse koostamise konteinerite teenused keskmise suurusega ümbriste Linux.

Teenuse struktuuri on mitu container võimalused, mis aitavad teil rakenduste mis koosnevad microservices, mis on konteiner loomine. Neid nimetatakse konteinerite teenused.

Võimaluste hulka;

- Container pilt juurutus- ja aktiveerimine
- Ressursside juhtimine
- Hoidla autentimine
- Container port host port kaardistamine
- Container-container discovery ja suhtlus
- Võimalus määrata keskkonna muutujate ja konfigureerimiseks


## <a name="packaging-a-docker-container-with-yeoman"></a>Keskmise suurusega container pakendamine yeoman abil
Kui pakendamine Linux ümbris, saate valida, kas kasutada yeoman malli või [luua rakenduse pakett käsitsi](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

Teenuse struktuuri rakendus võib sisaldada ühe või mitme ümbriste, kindla rolliga pakkuda rakenduse funktsioone. Teenuse struktuuri SDK Linuxi sisaldab [Yeoman](http://yeoman.io/) generaator, mis hõlbustab rakenduse loomine ja lisamine ümbrises. Kasutame Yeoman luua uue rakenduse ühe keskmise suurusega container *SimpleContainerApp*nimega. Saate lisada rohkem teenuseid hiljem, redigeerides loodud näidata failid.

## <a name="create-the-application"></a>Rakenduse loomine

1. Sisestage terminalis, **yo azuresfguest**.

2. Valige Framework **ümbrises** .

3. Nt SimpleContainerApp rakenduse nimi

4. Container DockerHub Repo pildi URL-i ette. See viib vormi [repo] ja [pildi nimi]

![Ümbriste teenuse struktuuri Yeoman generaator][sf-yeoman]

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

5. Malli esitatud desinstallimine skripti abil saate kustutada rakenduse ja registreerimise tühistamine rakenduse tüüp.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Järgmised sammud

- [Teenuse struktuuri ja ümbriste ülevaade](service-fabric-containers-overview.md)
- [Teenuse struktuuri kogumite Azure'i CLI kaudu suhtlemiseks](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

