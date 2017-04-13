<properties
   pageTitle="Mõne ASP.net-i Core Linux keskmise suurusega container juurutada remote keskmise suurusega host | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada Visual Studio Tools for keskmise suurusega juurutada keskmise suurusega ümbris töötavate Azure keskmise suurusega Host Linux VM ASP.net-i Core web app"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Keskmise suurusega remote host on ASP.net-i container juurutamiseks

## <a name="overview"></a>Ülevaade
Keskmise suurusega on kerge container otsimootori sarnase mõned viisid virtuaalse masina, mille abil saate host rakendused ja teenused.
Selles õpetuses juhendab teid keskmise suurusega Host Azure PowerShelli kaudu ASP.net-i Core rakenduse juurutamine [Visual Studio 2015 tööriistad keskmise suurusega](http://aka.ms/DockerToolsForVS) laiend abil.

## <a name="prerequisites"></a>Eeltingimused
Selle õpetuse lõpuleviimiseks on vaja järgmist:

- Azure keskmise suurusega Host VM loomine, nagu on kirjeldatud [Azure keskmise suurusega-seadme kasutamine](./virtual-machines/virtual-machines-linux-docker-machine.md)
- [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129) installimine
- [Microsoft ASP.net-i Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- Installige [Visual Studio 2015 tööriistad keskmise suurusega - eelvaade](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. ASP.net-i Core web rakenduse loomine
Järgmised toimingud juhendab teid lihtsa ASP.net-i Core rakenduse, mida kasutatakse selles õpetuses loomine.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2 keskmise suurusega toe lisamine

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Kasutage DockerTask.ps1 PowerShelli skripti 

1.  Avage PowerShelli käsuviibas juurkaust projekti abil. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Kinnitage serveri host töötab. Peaksite nägema olekus = töötab 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Kui kasutate keskmise suurusega beeta, ei oma hosti siin loetletud.

1.  Rakenduse abil koostada parameetrit - koostamine

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Kui kasutate keskmise suurusega beeta, jätke välja argumendi - arvutisse
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Käivitage rakendus, kasutades parameetrit - käivitamine

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Kui kasutate keskmise suurusega beeta, jätke välja argumendi - arvutisse
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Kui keskmise suurusega lõpule jõudnud, peaksite nägema järgmine sarnase tulemuse:

    ![Saate vaadata oma rakenduse][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
