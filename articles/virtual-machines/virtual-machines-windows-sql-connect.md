<properties
    pageTitle="Ühenduse loomine SQL serveri virtuaalse masina (ressursihaldur) | Microsoft Azure'i"
    description="Saate teada, kuidas ühenduse SQL Server Azure'i virtuaalse masina töötab. Selles teemas kasutab klassikaline juurutamise mudel. Stsenaariumid erineda sõltuvalt Võrgu konfigureerimisel ja kliendi asukohta."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Ühenduse loomine SQL serveri virtuaalse masina Azure (ressursihaldur)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-sql-connect.md)
- [Klassikaline](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Ülevaade

Selles teemas kirjeldatakse, kuidas ühenduse loomine SQL serveri eksemplar Azure virtuaalne arvutis töötab. See hõlmab teatud [Üldine ühenduvuse stsenaariumid](#connection-scenarios) ning seejärel on [üksikasjalikult kirjeldatud konfigureerimine on Azure VM SQL serveri Ühenduvus](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel. Selles artiklis tavaversiooni kuvamiseks teemast [ühenduse loomine abil SQL serveri virtuaalse masina Azure'i klassikaline kohta](virtual-machines-windows-classic-sql-connect.md).

Kui te eelistaksite täielik walk-through ettevalmistamise ja ühenduvuse, lugege teemat [Azure SQL serveri virtuaalse masina ettevalmistamise](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Ühenduse stsenaariumid

Nii, nagu klient loob ühenduse SQL Server töötab virtuaalse masina erineb olenevalt kliendi ja seadme/võrgunduse konfiguratsiooni asukohta. Need stsenaariumid on järgmised.

- [SQL serveriga Interneti kaudu](#connect-to-sql-server-over-the-internet)
- [SQL serveriga virtuaalse samasse võrku](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>SQL serveriga Interneti kaudu

Kui soovite oma SQL serveri andmebaasi mootor ühenduse Interneti kaudu, on nõutav, nt konfigureerida tulemüüri, SQL-i autentimise lubamine ja konfigureerimine oma võrgu turberühma võrgu turberühma reegli Port 1433 TCP liikluse lubamiseks peavad teil mitu toimingut.

Kui kasutate portaali ettevalmistamise SQL serveri virtuaalse masina pilt koos ressursihaldur, järgmist on tehtud kui valite **avaliku** SQL-i ühenduvuse suvand:

![Avaliku SQL-i ühenduvuse suvand ettevalmistamise ajal](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Kui see ei ettevalmistamise ajal, siis saate käsitsi konfigureerida SQL serveri ja teie virtuaalmasinates [käsitsi konfigureerida Ühenduvus selle artikli juhiseid](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm)järgides.

>[AZURE.NOTE] Automaatselt ei võimalda virtuaalse masina pildi SQL Server Express Editioni kohta. Express väljaanne, peate kasutama SQL Serveri konfigureerimishaldur [käsitsi](#configure-sql-server-to-listen-on-the-tcp-protocol) lubamise kohta VM loomise järel.

Kui see on tehtud, mis tahes kliendi Interneti-ühendusega saate luua ühenduse SQL serveri eksemplar, määrates kas virtuaalse masina avaliku IP-aadress või DNS-i määratud IP-aadressi silt. Kui SQL serveri pordi 1433, peate selle määrata ühendusstring.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Kuigi see lubab connectivity kliendi Interneti kaudu, see ei tähenda, et igaüks, saate luua ühenduse SQL serveris. Väljaspool kliendid on õige kasutajanime ja parooli. Suurema turvalisuse tagamiseks, saate vältida tuntud pordi 1433. Näiteks kui SQL serveri kuulata pordi 1500 konfigureeritud ja proper tulemüür ja võrgu turvalisuse rühma reeglid, saanud ühendust, lisades pordi number serveri nime nagu järgmises näites:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Pole tähtis, et kui selle meetodi abil saate suhelda SQL Server Azure'i andmekeskuse kõigi väljaminevate andmete on tavaline [hinnad väljaminev andmeedastuse kohta](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>SQL serveriga virtuaalse samasse võrku

[Virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) võimaldab täiendavad stsenaariumid. Saate luua ühenduse VMs virtuaalse samasse võrku, isegi juhul, kui need on olemas erinevate ressursside rühmad. Ja [- saidilt VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md), saate luua hübriid arhitektuur, mis ühendab VMs kohapealse võrgud ja seadmed.

Virtuaalne võrkude võimaldab teil oma Azure VMs domeeniga liita. See on ainus võimalus kasutada SQL serveri Windowsi autentimist. Ühenduse teiste stsenaariumide vajavad SQL-i autentimise kasutajanimed ja paroolid.

Portaali kasutamisel ettevalmistamise SQL serveri virtuaalse masina pilt koos ressursihaldur proper tulemüüri reeglid virtuaalse võrgu teatis on häälestus, kui valite **Privaatne** SQL-i ühenduvuse suvandi. Kui see ei ettevalmistamise ajal, siis saate käsitsi konfigureerida SQL serveri ja teie virtuaalmasinates [käsitsi konfigureerida Ühenduvus selle artikli juhiseid](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm)järgides. Kuid kui kavatsete domeenikeskkonna ja Windowsi autentimise konfigureerimine, pole vaja järgige selle artikli SQL-i autentimise ja sisselogimise konfigureerimiseks. Samuti pole vaja konfigureerida võrgu turberühma reeglid juurdepääsuks Interneti kaudu.

>[AZURE.NOTE] Automaatselt ei võimalda virtuaalse masina pildi SQL Server Express Editioni kohta. Express väljaanne, peate kasutama SQL Serveri konfigureerimishaldur [käsitsi](#configure-sql-server-to-listen-on-the-tcp-protocol) lubamise kohta VM loomise järel.

Eeldades, et olete konfigureerinud virtuaalse võrgu DNS-i, saate luua ühenduse SQL serveri eksemplar SQL serveri VM arvuti nimi määrates ühendusstring. Järgmises näites ka eeldab, et Windowsi autentimine on konfigureeritud ka ja et kasutajale on antud juurdepääs SQL serveri eksemplar.

    "Server=mysqlvm;Integrated Security=true"

Pange tähele, et selle stsenaariumi korral saate ka määrata VM IP-aadress.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Juhised käsitsi konfigureerimine on Azure VM SQL serveri Ühenduvus

Järgmised toimingud näitavad, kuidas käsitsi häälestamine ühenduvuse SQL serveri eksemplar ja seejärel soovi korral ühendage Internetis SQL Server Management Studio (SSMS) abil. Pange tähele, et neid juhiseid paljude on valmis teile vastav SQL serveri Ühenduvus suvandite valimise portaalis.

Enne SQL serveri eksemplar teise VM või Interneti kaudu ühendust luua, peate tegema järgmised toimingud jaotistest kirjeldatud.

- [Windowsi tulemüüri avatud TCP-pordid](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL serveri kuulata TCP-protokolli konfigureerimine](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Kombineeritud režiimi autentimist kasutava SQL serveri konfigureerimine](#configure-sql-server-for-mixed-mode-authentication)
- [SQL serveri autentimine sisselogimise loomine](#create-sql-server-authentication-logins)
- [Võrgu turberühma Sissetulev reegel konfigureerimine](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [DNS-i jaoks avaliku IP-aadressi silt konfigureerimine](#configure-a-dns-label-for-the-public-ip-address)
- [Ühenduse andmebaasimootor ühest arvutist teise](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Järgmised sammud

Ettevalmistamise juhend koos järgmist Ühenduvus, leiate teemast [Azure SQL serveri virtuaalse masina ettevalmistamise](virtual-machines-windows-portal-sql-server-provision.md).

SQL Server Azure'i virtuaalmasinates [Uuri Õppeteema](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) .

Muud teemad, mis töötab SQL Server Azure'i VMs seotud, vt [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md).
