<properties
   pageTitle="Vastastikuste laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli kasutamine Azure CLI Interneti loomise alustamiseks | Microsoft Azure'i"
   description="Saate teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli kasutamine Azure CLI"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Vastastikuste laadi koormusetasakaalustusteenuse (klassikaline) CLI Azure'i Interneti loomise alustamiseks

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Samuti saate [teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse Azure'i ressursihaldur abil](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Samm-sammult loomise Interneti vastastikuste laadi koormusetasakaalustusteenuse CLI abil

Sellest juhendist näitab, kuidas luua Interneti laadi koormusetasakaalustusteenuse ülaltoodud stsenaariumi.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../../articles/xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.

2. Käsu **azure config režiimi** aktiveerimiseks klassikaline režiim, nagu allpool näidatud.

        azure config mode asm

    Oodatav väljund:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Lõpp-punkti loomine ja laadimine koormusetasakaalustusteenuse määramine

Stsenaarium eeldab virtuaalmasinates "web1" ja "web2" on loodud.
Sellest juhendist loob laadi koormusetasakaalustusteenuse määramine, avaliku Port pordi 80 ja pordi 80 kohaliku Port abil. Pordi juures on ka konfigureeritud pordi 80 ja laadi koormusetasakaalustusteenuse määramine "lbset" nimega.


### <a name="step-1"></a>Samm 1

Esimese lõpp-punkti loomine ja laadimine koormusetasakaalustusteenuse Kasuta hulga `azure network vm endpoint create` virtual masina "web1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Parameetreid kasutatakse:

**-k** - virtual kohalikus arvutis port<br>
**-o** - Protocol (protokoll)<BR>
**-t** - juures port<BR>
**– b** – laadi koormusetasakaalustusteenuse nimi<BR>

## <a name="step-2"></a>Samm 2

Lisage teine virtuaalse masina "web2" Laadi koormusetasakaalustusteenuse määramine.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Samm 3

Laadi koormusetasakaalustusteenuse konfigureerimine abil kontrollida `azure vm show` .

    azure vm show web1

Väljund on järgmised:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Vastuseks Kaug töölaua lõpp-punkti loomine virtuaalse masina jaoks

Saate luua Remote'i töölaua endpoint võrguliiklust avaliku port suunata kohaliku port teatud virtuaalse masina kasutamiseks `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Laadi koormusetasakaalustusteenuse virtuaalse masina eemaldamine

Teil on seotud laadi koormusetasakaalustusteenuse, virtuaalse masina määrata lõpp-punkti kustutamine. Kui lõpp-punkti on eemaldatud, virtuaalse masina ei kuulu laadi koormusetasakaalustusteenuse, määrake enam.

 Ülaltoodud näites saate eemaldada loodud virtuaalse masina "web1" lõpp-punkti laadi koormusetasakaalustusteenuse "lbset" käsu `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Saate uurida haldamiseks lõpp-punktid, kasutades käsku rohkem suvandeid`azure vm endpoint --help`


## <a name="next-steps"></a>Järgmised sammud

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-ps.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)

