<properties
   pageTitle="Keskmise suurusega Host konfigureerimine VirtualBox | Microsoft Azure'i"
   description="Üksikasjalike juhiste konfigureerida vaikimisi keskmise suurusega näiteks keskmise suurusega masina ja VirtualBox abil"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Keskmise suurusega Host VirtualBox konfigureerimine

## <a name="overview"></a>Ülevaade
Selles artiklis juhendab teid konfigureerida vaikimisi keskmise suurusega näiteks keskmise suurusega masina ja VirtualBox abil. Kui kasutate [Windowsi keskmise suurusega beeta](http://beta.docker.com/), selle konfiguratsiooni ei ole vaja.

## <a name="prerequisites"></a>Eeltingimused
Järgmised tööriistad tuleb installida.

- [Keskmise suurusega tööriistad](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Keskmise suurusega kliendi konfigureerimine Windows PowerShelli abil

Keskmise suurusega kliendi konfigureerimiseks lihtsalt Avage Windows PowerShelli ja tehke järgmist.

1. Vaikimisi keskmise suurusega host eksemplari loomine

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Veenduge, et vaikimisi eksemplari on konfigureeritud ja töötab. (Näiteks nimega "vaikimisi" töötab peaksite nägema.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![keskmise suurusega-seadme ls väljund][0]
 
1. Vaikimisi seatud praeguse hostiteenuse ja konfigureerige oma shell.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Aktiivse keskmise suurusega ümbriste kuvamine Loend peab olema tühi.

    ```PowerShell
    docker ps
    ```

    ![keskmise suurusega ps väljund][1]
 
> [AZURE.NOTE]Iga kord, kui arengu arvuti taaskäivitada peate taaskäivitage oma kohaliku keskmise suurusega hosti.
> Selle tegemiseks küsimus käsuviibale järgmine käsk: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
