<properties
   pageTitle="Luua ka sisemise laadi koormusetasakaalustusteenuse Azure'i CLI klassikaline juurutamise mudeli kasutamine | Microsoft Azure'i"
   description="Saate teada, kuidas luua ka sisemise laadi koormusetasakaalustusteenuse Azure'i CLI klassikaline juurutamise mudeli kasutamine"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Azure'i CLI abil on sisemine laadi koormusetasakaalustusteenuse (klassikaline) loomise alustamiseks

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Luua ka sisemise koormuse koormusetasakaalustusteenuse virtuaalmasinates määramine

Mõne sisemise koormuse koormusetasakaalustusteenuse seadmine ja serverid, mis saadab sellele oma liikluse loomiseks peab tehke järgmist.

1. Looge eksemplari sisemise laadimine tasakaalustamiseks mis liiklust olema laadi koormusetasakaalustusega Sea serverite lõikes lõpp-punktile.

1. Lisage vastav virtuaalmasinates, mida saavad sissetuleva liikluse lõpp-punktid.

1. Konfigureerige serverid, et saata liikluse olema koormus tasakaalustatud saatma oma liikluse virtuaalse IP (VIP) aadressil sisemise laadimine tasakaalustamiseks astme.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Samm-sammult luua ka sisemise laadi koormusetasakaalustusteenuse CLI abil

Sellest juhendist näitab, kuidas luua sisemise koormuse koormusetasakaalustusteenuse, mis põhineb ülaltoodud stsenaarium.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../../articles/xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.

2. Käsu **azure config režiimi** aktiveerimiseks klassikaline režiim, nagu allpool näidatud.

        azure config mode asm

    Oodatav väljund:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Lõpp-punkti loomine ja laadimine koormusetasakaalustusteenuse määramine

Stsenaarium eeldab virtual masinad "DEG1" ja "DB2" nimega "mytestcloud" pilveteenus. Nii virtuaalmasinates kasutavad virtuaalse võrgust, mida nimetatakse minu "testvnet" koos alamvõrgu "alamvõrgu-1".

Sellest juhendist luua sisemise koormuse koormusetasakaalustusteenuse Sea abil port 1433 privaatne Port ja 1433 kohaliku Port.

See on levinud stsenaariumi, kus teil on SQL-i virtuaalmasinates tagasi lõpuks on sisemine laadi koormusetasakaalustusteenuse abil tagada andmebaasi serverid ei esitatud abil ühiskasutusse anda avaliku IP-aadressi.


### <a name="step-1"></a>Samm 1

Sisemise koormuse koormusetasakaalustusteenuse, mis Kasuta hulga loomiseks `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Parameetreid kasutatakse:

**-r** - pilvepõhise teenuse nimi<BR>
**-n** - sisemise koormuse koormusetasakaalustusteenuse nimi<BR>
**-t** - alamvõrgu nimi (sama alamvõrgu virtuaalmasinates, tuleb lisada koormusetasakaalustusteenuse sisemise laadi järgi)<BR>
**-** - (valikuline) lisada staatilise privaatne IP-aadress<BR>

Tutvuge `azure service internal-load-balancer --help` lisateabe saamiseks.

Saate sisemise koormuse koormusetasakaalustusteenuse atribuudid käsk `azure service internal-load-balancer list` *pilvepõhise teenuse nimi*.

Siin järgib väljund näide:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Samm 2

Saate konfigureerida sisemise koormuse koormusetasakaalustusteenuse määramine, kui lisate esimese lõpp-punkti. Seostab lõpp-punkti, virtuaalse masina ja juures pordi sisemise koormuse koormusetasakaalustusteenuse häälestada selles juhises.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Parameetreid kasutatakse:

**-k** - virtual kohalikus arvutis port<BR>
**-t** - juures port<BR>
**-r** - juures Protocol (protokoll)<BR>
**-e** - juures intervall sekundites<BR>
**-f** - ajalõpp intervall sekundites <BR>
**-i** - sisemise koormuse koormusetasakaalustusteenuse nimi <BR>


## <a name="step-3"></a>Samm 3

Laadi koormusetasakaalustusteenuse konfigureerimine abil kontrollida `azure vm show` *virtuaalse masina nimi*

    azure vm show DB1

Väljund on järgmised:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Vastuseks Kaug töölaua lõpp-punkti loomine virtuaalse masina jaoks

Saate luua Remote'i töölaua endpoint võrguliiklust avaliku port suunata kohaliku port teatud virtuaalse masina kasutamiseks `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Laadi koormusetasakaalustusteenuse virtuaalse masina eemaldamine

Virtuaalse masina eemaldamiseks on sisemine laadi koormusetasakaalustusteenuse määratud seotud lõpp-punkti kustutamine. Kui lõpp-punkti on eemaldatud, virtuaalse masina ei kuulu laadi koormusetasakaalustusteenuse, määrake enam.

 Ülaltoodud näites saate eemaldada käsu abil loodud virtuaalse masina "DEG1" kaudu sisemise koormuse koormusetasakaalustusteenuse "ilbset" lõpp-punkti `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Tutvuge `azure vm endpoint --help` lisateabe saamiseks.


## <a name="next-steps"></a>Järgmised sammud

[Laadi koormusetasakaalustusteenuse jaotuse režiimi kasutamine allika IP osaleja konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)