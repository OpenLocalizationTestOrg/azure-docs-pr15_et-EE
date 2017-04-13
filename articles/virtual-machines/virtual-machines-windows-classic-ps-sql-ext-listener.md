<properties
    pageTitle="Konfigureerimine on välised kuulajale alati klõpsake kättesaadavus rühmade jaoks | Microsoft Azure'i"
    description="Selles õpetuses juhendab teid Azure, millele pääseb väliselt, kasutades avaliku seotud pilveteenuses virtuaalse IP-aadress on alati klõpsake kättesaadavus rühma kuulajale loomise juhiseid."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Azure'i mõne välise kuulajale alati klõpsake kättesaadavus rühmade konfigureerimine

> [AZURE.SELECTOR]
- [Sisemise kuulajale](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Välise kuulajale](virtual-machines-windows-classic-ps-sql-ext-listener.md)

Selles teemas näidatakse, kuidas konfigureerida on kuulajale alati klõpsake kättesaadavus rühma mis Internetis väliselt juurdepääsetav. See on võimalik selle pilveteenuses **Avaliku virtuaalse IP (VIP)** aadressi koos kuulajale seostamise kaudu.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Kättesaadavus rühma võivad sisaldada koopiad, mis on kohapealse ainult Azure ainult või ühendada nii kohapealse ja Azure hübriidjuurutuse jaoks. Azure'i koopiad võivad asuda samas piirkonnas või mitme piirkondade mitme virtuaalse võrgu (VNets) abil. Allpool juhistes eeldatakse, et teil on juba [konfigureeritud kättesaadavus rühma](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , kuid on konfigureeritud on kuulajale.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Välise kuulajatele seadmine

Kui võtate kasutusele cloud teenuse kese VIP aadressilt, võtke arvesse järgmisi juhiseid kättesaadavus rühma kuulajale Azure kohta.

- Kättesaadavus rühma kuulajale on toetatud Windows Server 2008 R2, Windows Server 2012 ja Windows Server 2012 R2.

- Erinevate pilveteenuses, kui üks, mis sisaldab teie kättesaadavus rühma VMs peab asu klientrakendust. Azure'i ei toeta otseselt serveri tagasi klient ja server sama pilveteenuses.

- Selles artiklis toodud juhiseid Kuva vaikimisi konfigureerimine ühe kuulajale kasutada pilvepõhise teenuse virtuaalse IP (VIP) aadress. Siiski, on võimalik reserveerida ja luua mitu VIP-aadressi jaoks oma pilveteenuses. See võimaldab teil järgige selle artikli mitme kuulajatele, mis on seotud erinevate VIP loomiseks. Mitu VIP-aadressi loomise kohta leiate teemast [Mitme VIP pilveteenuses kohta](../load-balancer/load-balancer-multivip.md).

- Kui loote on kuulajale keskkonnas hübriid, olema kohapealse võrgu-saidilt VPN Azure virtuaalse võrguga lisaks avaliku Interneti-ühendus. Kui Azure alamvõrgu kättesaadavus rühma kuulajale on kättesaadav ainult vastava pilveteenuse avaliku IP-aadress.

- Ei toetata luua mõne välise kuulajale sama pilveteenusesse, kus teil on mõni sisemine kuulajale sisemise laadi koormusetasakaalustusteenuse (ILB) abil.

## <a name="determine-the-accessibility-of-the-listener"></a>Valitud hõlbustusfunktsioonide kuulajale määratlemine

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

See artikkel keskendub loomise kuulajale, mis kasutab **välise koormuse tasakaalustamiseks**. Kui soovite kuulajale, mis on privaatne, virtuaalse võrku, näete versiooni käesoleva artikli, mis on [kuulajale koos ILB](virtual-machines-windows-classic-ps-sql-int-listener.md) häälestamise juhised

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Luua otse serveriga saatja koormusetasakaalustusega VM lõpp-punktid

Välise koormusetasakaalustuseks kasutab virtuaalne pilveteenuses, mis hostib teie VMs avaliku virtuaalse IP-aadress. Seega pole vaja luua või konfigureerida laadi koormusetasakaalustusteenuse sel juhul.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Kopeerige PowerShelli skripti all tekstiredaktoris ja seadke muutujaga vastavalt teie keskkonnas (vaikesätted on toodud mõned parameetrid). Pange tähele, et kättesaadavus rühma ulatub Azure regioonid, peate käivitama skripti üks kord iga andmekeskuses pilveteenuses ja sõlmed, mis asuvad selle andmekeskuses.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Kui olete seadistanud muutujaid, kopeerige skripti tekstiredaktoris üheks seansi Azure PowerShelli seda käivitamiseks. Kui endiselt kuvatakse viip >>, tippige uuesti veendumaks, et skript käivitub ENTER.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Veenduge, et KB2854082 on installitud vajaduse korral

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Avage tulemüüri portide kättesaadavus rühma sõlmed

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Kättesaadavus rühma kuulajale loomine

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Välise koormusetasakaalustuseks jaoks peate hankima pilveteenuses, mis sisaldab teie koopiad avaliku virtuaalse IP-aadress. Azure'i klassikaline portaali sisse logida. Liikuge pilveteenuses, mis sisaldab teie kättesaadavus rühma VM. **Armatuurlaua** vaate avamine

3. Pange tähele kuvatud jaotises **avalike virtuaalse IP (VIP) aadress**. Kui teie lahendus ulatub VNets, korrake seda toimingut iga pilveteenuses, mis sisaldab VM, mis hostib koopia.

4. Üks VMs, kopeerige PowerShelli skripti all tekstiredaktoris ja seadke muutujate eespool märgitud väärtused.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Üks kord teil on seatud muutujaid, avage administraatoriõigustes Windows PowerShelli aken, siis kopeerige script editor teksti ja kleepida selle käivitamiseks seansi Azure PowerShelli. Kui endiselt kuvatakse viip >>, tippige uuesti veendumaks, et skript käivitub ENTER.

1. Korrake seda toimingut iga VM. See skript konfigureerib IP-aadressi ressursi pilveteenusesse IP-aadress ja määrab muude parameetrite nagu pordi juures. Kui IP-aadressi ressursi ühendamise, saate seda siis vastata küsitlused juures Port loodud selles õpetuses koormusetasakaalustusega lõpp.

## <a name="bring-the-listener-online"></a>Tuua kuulajale võrgus

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Järeltegevuse üksused

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testige kättesaadavus rühma kuulajale (jooksul sama VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testige kättesaadavus rühma kuulajale (Interneti)

Väljaspool virtuaalse võrgu kaudu kuulajale juurdepääsuks peate kasutama väline/avalik koormusetasakaalustuseks (selles teemas kirjeldatud) asemel ILB, mis on sama VNet sees ainult kättesaadavad. Ühendusstringi, saate määrata pilvepõhise teenuse nimi. Näiteks kui oleksite pilveteenus koos nimi *mycloudservice*, sqlcmd lause oleks järgmiselt:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Erinevalt eelmises näites SQL-i autentimise tuleb kasutada, kuna helistaja ei saa kasutada Interneti kaudu Windowsi autentimist. Lisateavet leiate teemast [alati sisse kättesaadavus rühma Azure VM: klientrakenduse ühenduvuse stsenaariumid](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Kui SQL-i autentimist kasutades veenduge, et luua sama kasutajanime mõlemad koopiad. Kättesaadavus levirühmadega sisselogimise tõrkeotsingu kohta lisateabe saamiseks näha, [Kuidas logimine vastendada või kasutada sisaldab SQL-andmebaasi ühenduse muude koopiatega ja kättesaadavus andmebaaside vastendamine kasutaja](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Kui on alati koopiad teise alamvõrku, peate määrama kliendid **MultisubnetFailover = True** ühendusstring. Tulemuseks paralleelselt ühenduse katsete teise alamvõrku koopiad. Pange tähele, et sel juhul sisaldab rist-regioon alati klõpsake kättesaadavus rühma juurutamise.

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
