<properties
    pageTitle="Klassikaline Azure VM logida | Microsoft Azure"
    description="Klassikaline Azure portaali abil loodud mudel classic juurutamine Windowsi virtuaalarvuti sisse logida."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Windowsi virtuaalarvuti Azure klassikaline portaali sisse logida

Klassikaline Azure'i portaalis saate nupule **Ühenda** kaugtöölaua seansi alustamine ja logige Windows VM.

Kas soovite ühendada Linux VM? Vaadake, [Kuidas töötab Linuxi sisselogimiseks](virtual-machines-linux-mac-create-ssh-keys.md).

Õppida, kuidas [kasutada uue Azure portaali](virtual-machines-windows-connect-logon.md)administraatorina.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Video teid läbi

Siin on video teid läbi sammud õpetamisel. See hõlmab ka lõpp-punktide ja avaliku ja erasektori pordid, ühendamiseks Windows Azure VM.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Ühendust virtuaalarvuti

1. Klassikaline Azure portaali sisse logida.

2. Klõpsake **virtuaalarvutid**ja valige virtuaalne masin.

3. Käsuribal lehe allosas, klõpsake nuppu **Ühenda**.

    ![Logige virtuaalne masin](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] Kui **Connect** nupp ei ole saadaval, vaadake tõrkeotsingu näpunäiteid selle artikli lõpus.

## <a name="log-on-to-the-virtual-machine"></a>Logige virtuaalne masin

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Järgmised sammud

-   Kui nupule **Ühenda** pole aktiveeritud või sul on muid probleeme kaugtöölaua ühendus, proovige lähtestada konfiguratsiooni. Virtuaalarvuti armatuurlauale **Kiire lühidalt**, klõpsake jaotises **serveri konfiguratsiooni lähtestamine**.
-   Probleemid parooliga, proovige see lähtestada. Virtuaalarvuti armatuurlauale **Kiire lühidalt**, klõpsake **Lähtesta parool**.

Kui need soovitused ei tööta või pole vajalikku, vt [tõrkeotsing kaugtöölaua ühenduse et Windowsiga Azure virtuaalarvuti](virtual-machines-windows-troubleshoot-rdp-connection.md). See artikkel annab teile diagnoosimiseks ja ühiste probleemide lahendamine.


