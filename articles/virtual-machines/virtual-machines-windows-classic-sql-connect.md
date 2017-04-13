<properties
    pageTitle="Ühenduse loomine SQL serveri virtuaalse masina (klassikaline) | Microsoft Azure'i"
    description="Saate teada, kuidas ühenduse SQL Server Azure'i virtuaalse masina töötab. Selles teemas kasutab klassikaline juurutamise mudel. Stsenaariumid erineda sõltuvalt Võrgu konfigureerimisel ja kliendi asukohta."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Ühenduse loomine SQL serveri virtuaalse masina Azure (klassikaline juurutus)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-sql-connect.md)
- [Klassikaline](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Ülevaade

Selles teemas kirjeldatakse, kuidas ühenduse loomine SQL serveri eksemplar Azure virtuaalne arvutis töötab. See hõlmab teatud [Üldine ühenduvuse stsenaariumid](#connection-scenarios) ning seejärel on [üksikasjalikult kirjeldatud konfigureerimine on Azure VM SQL serveri Ühenduvus](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Kui kasutate ressursihaldur VMs, teemast [ühenduse loomine abil SQL serveri virtuaalse masina Azure ressursihaldur abil](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Ühenduse stsenaariumid

Nii, nagu klient ühendub SQL Server töötab virtuaalse masina erineb olenevalt kliendi ja seadme/võrgunduse konfiguratsiooni asukohta. Need stsenaariumid on järgmised.

- [SQL serveriga sama pilveteenuses](#connect-to-sql-server-in-the-same-cloud-service)
- [SQL serveriga Interneti kaudu](#connect-to-sql-server-over-the-internet)
- [SQL serveriga virtuaalse samasse võrku](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Enne, kui ühendate kõik need meetodid, peate järgima [ühenduvuse konfigureerimine selle artikli juhiseid](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>SQL serveriga sama pilveteenuses

Mitme virtuaalmasinates saab luua sama pilveteenuses. Selle stsenaariumi virtuaalmasinates, vt [virtuaalmasinates virtuaalse võrgu või pilvepõhises teenusega ühenduse loomise](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). See stsenaarium on, kui ühe virtual arvutisse Klient proovib luua ühenduse SQL serveris virtual mõnda teise arvutisse sama pilveteenuses.

Selle stsenaariumi korral saate ühendada VM **nimi** (näidatud ka **Arvuti nimi** või **hostname** portaalis) abil. See on nimi, mis on esitatud VM loomise ajal. Näiteks kui teie nimi oma SQL-i VM **mysqlvm**, kliendi VM sama pilveteenuses võib kasutada järgmist ühendusstringi ühenduse:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>SQL serveriga Interneti kaudu

Kui soovite oma SQL serveri andmebaasi mootor ühenduse Interneti kaudu, peate looma virtuaalse masina lõpp-punkti sissetulevate TCP suhtlemiseks. Selles etapis Azure konfigureerimine suunab TCP pordi liiklust TCP pordi, mis on juurdepääsetav virtuaalse masina.

Ühendust Interneti kaudu, peate kasutama funktsiooni VM DNS-i nimi ja VM lõpp-punkti pordi number (konfigureeritud selle artikli). Otsige üles DNS-i nimi, liikuge Azure'i portaal ja valige **virtuaalmasinates (klassikaline)**. Valige oma virtuaalse masina. **DNS-i nimi** kuvatakse jaotises **Ülevaade** .

Näiteks kaaluge klassikaline virtuaalse masina nimega **mysqlvm** **mysqlvm7777.cloudapp.net** DNS-i nimi ja, **57500**VM lõpp-punkti. Eeldades, et Ühenduvus õigesti konfigureeritud, saab järgmine ühendusstringi juurdepääs kõikjal masina virtual internetis:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Kuigi see lubab connectivity kliendi Interneti kaudu, see ei tähenda, et igaüks, saate luua ühenduse SQL serveris. Väljaspool kliendid on õige kasutajanime ja parooli. Suurema turvalisuse tagamiseks, ärge kasutage tuntud pordi 1433 avaliku virtuaalse masina lõpp-punkti. Ja võimaluse korral arvesse võtta mõne ACL lisamine oma lõpp-punkti piirata liikluse ainult klientide te lubate. Juhised lõpp-punktid ACL-ID abil, vt [haldamine ACL lõpp-punkti](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Pole tähtis, et kui selle meetodi abil saate suhelda SQL Server Azure'i andmekeskuse kõigi väljaminevate andmete on tavaline [hinnad Väljamineva meili andmeedastuse kohta](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>SQL serveriga virtuaalse samasse võrku

[Virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) võimaldab täiendavad stsenaariumid. Saate luua ühenduse VMs virtuaalse samasse võrku, isegi juhul, kui need on olemas eri pilveteenustega. Ja [- saidilt VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md), saate luua hübriid arhitektuur, mis ühendab VMs kohapealse võrgud ja seadmed.

Virtuaalne võrkude võimaldab teil oma Azure VMs domeeniga liita. See on ainus võimalus kasutada SQL serveri Windowsi autentimist. Teiste stsenaariumide ühenduse nõudmine SQL-i autentimise kasutajanimed ja paroolid.

Kui te ei kavatse domeenikeskkonna ja Windowsi autentimise konfigureerimine, peate järgige selle artikli avaliku lõpp-punkti või SQL-i autentimise ja sisselogimise konfigureerimiseks. Selle stsenaariumi korral saab ühenduse SQL serveri eksemplar SQL serveri VM nime määramisega ühendusstring. Järgmises näites eeldatakse, et Windowsi autentimine on konfigureeritud ka ja et kasutajale on antud juurdepääs SQL serveri eksemplar.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Juhised on Azure VM SQL serveri ühenduvuse konfigureerimine

Järgmised sammud kirjeldavad kuidas luua ühenduse SQL serveri eksemplar SQL Server Management Studio (SSMS) abil Interneti kaudu. Samu juhiseid rakendada arvuti SQL serveri virtual kättesaadavaks oma taotluste asutusesiseselt ja Azure tegemine.

Enne SQL serveri eksemplar teise VM või Interneti kaudu ühendust luua, peate tegema järgmised toimingud jaotistest kirjeldatud.

- [TCP lõpp-punkti jaoks virtuaalse masina loomine](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Windowsi tulemüüri avatud TCP-pordid](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL serveri kuulata TCP-protokolli konfigureerimine](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Kombineeritud režiimi autentimist kasutava SQL serveri konfigureerimine](#configure-sql-server-for-mixed-mode-authentication)
- [SQL serveri autentimine sisselogimise loomine](#create-sql-server-authentication-logins)
- [DNS-i virtuaalse masina nimi](#determine-the-dns-name-of-the-virtual-machine)
- [Ühenduse andmebaasimootor ühest arvutist teise](#connect-to-the-database-engine-from-another-computer)

Ühenduse tee on summeeritud, järgmisel joonisel:

![Ühenduse loomine SQL serveri virtuaalse masina](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Järgmised sammud

Kui te plaanite kasutada Kättesaadavusrühmad kättesaadavuse ja katastroofiabi, kaaluge rakendamisel on kuulajale. Andmebaasi klientrakendustega kuulajale, mitte otse mõnda SQL Serveri eksemplari. Kuulajale marsruudib kliendid kättesaadavus rühma peamine koopia. Lisateabe saamiseks vaadake [konfigureerimine on ILB kuulajale Kättesaadavusrühmad Azure'i jaoks](virtual-machines-windows-classic-ps-sql-int-listener.md).

See on oluline kõigi turvalisus head tavad läbivaatus SQL Server Azure'i virtual arvutis töötab. Lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates turvakaalutlused](virtual-machines-windows-sql-security.md).

SQL Server Azure'i virtuaalmasinates [Uuri Õppeteema](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) . 

Muud teemad, mis töötab SQL Server Azure'i VMs seotud, vt [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md).
