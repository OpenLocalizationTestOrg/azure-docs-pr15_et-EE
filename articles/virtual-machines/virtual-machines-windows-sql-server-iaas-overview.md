<properties
    pageTitle="Ülevaade: SQL Server Azure'i Virtuaalmasinates | Microsoft Azure'i"
    description="Lisateavet SQL serveri täielik versioonide käivitamiseks Azure virtuaalse arvutites. Saada kõik SQL serveri VM pilte ja seotud sisu otselingid."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Ülevaade: SQL Server Azure'i Virtuaalmasinates

Selles teemas kirjeldatakse võimalused töötab SQL Server Azure'i virtuaalmasinates (VMs) koos [portaali piltide linke](#option-1-create-a-sql-vm-with-per-minute-licensing) ja ülevaate [tavalised toimingud](#manage-your-sql-vm).

>[AZURE.NOTE] Kui olete juba tuttav SQL serveri ja soovite lihtsalt teada, kuidas kasutada SQL serveri VM, lugege teemat [sätte SQL serveri virtuaalse masina Azure portaali](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Ülevaade
Kui olete oma andmebaasi administraatorilt või arendaja, Azure VMs võimalda asutusesisese SQL serveri töökoormus ja rakenduste pilveteenusesse teisaldada. Järgmises videos antakse tehniline ülevaade SQL Server Azure'i VMs.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Video hõlmab järgmisi teemasid:

|Aeg|Ala|
|---|---|
| 00:21 | Mis on Azure VMs? |
| 01:45 | Turvalisus |
| 02:50 | Ühenduvus |
| 03:30 | Salvestusruumi töökindlust ja jõudlust |
| 05:20 | VM suurused |
| 05:54 | Kõrge-saadavus ja SLA |
| 07:30 | Konfiguratsiooni tugi |
| 08:00 | Jälgimine |
| 08:32 | SQL Server 2016 VM loomine |

>[AZURE.NOTE] Video keskendub SQL Server 2016, kuid pakub Azure'i VM piltide mitme versiooni SQL Server 2008, 2012, 2014 ja 2016. 

## <a name="scenarios"></a>Stsenaariumid
On palju põhjuseid, mida saate majutada oma andmete Azure. Kui teie taotlus on Azure teisaldada, see parandab jõudlust liikumiseks ka andmed. Kuid olemas on ka muid eeliseid. Teil on automaatselt juurdepääs mitme andmekeskuste globaalne võrgusoleku- ja katastroofi taaskasutamiseks. Andmed on tugevalt turvatud ja püsival.

SQL Server töötab Azure VMs on üks võimalus Azure relatsiooniliste andmete talletamiseks. See on hea valik mitu põhjust. Näiteks soovite konfigureerida Azure VM samamoodi kui võimalik, et mõne asutusesisese SQL serveri arvutisse. Ja te soovite käivitada sama andmebaasiserveri täiendavad rakendused ja teenused. On kaks peamist ressursse, mis aitavad teil veelgi stsenaariumid ja kasutuse läbi mõtlema.

 - [SQL Server Azure'i virtuaalmasinates](https://azure.microsoft.com/services/virtual-machines/sql-server/) annab ülevaate head võtted SQL serveri kasutamine Azure VMs. 
 - [Valige SQL Server suvand pilv: (PaaS) Azure SQL-i andmebaasi või SQL Server Azure'i VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) pakub SQL-andmebaas ja SQL Server töötab VM üksikasjalik võrdlus.

## <a name="create-a-new-sql-vm"></a>Luua uue SQL-i VM
Järgmistest jaotistest leiate Azure'i portaali otselingid SQL serveri virtuaalse masina Galerii pilte. Sõltuvalt teie valitud pildi saate maksta SQL serveri litsentsimise kulud minutis alusel või saate tuua oma litsents (BYOL).

Leiate üksikasjalikud juhised selle protsessi õpetuse, [sätte SQL serveri virtuaalse masina Azure'i portaalis](virtual-machines-windows-portal-sql-server-provision.md). Ka, lugege läbi [jõudluse head tavad SQL serveri VMs](virtual-machines-windows-sql-performance.md), mis selgitab, kuidas vastav masina suurus ja muud funktsioonid, mis on saadaval ettevalmistamise ajal valimiseks.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Variant 1: Luua SQL-i VM minutis litsentsimise
Järgmises tabelis toodud saadaval SQL serveri pildid galeriis virtuaalse masina maatriks. Klõpsake linki Uus SQL-i VM teie määratud versioon, edition ja operatsioonisüsteemi loomise alustamiseks.

|Versioon|Operatsioonisüsteem|Väljaande|
|---|---|---|
|**SQL-I 2016**|Windows Server 2012 R2|[Ettevõtte](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2) [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2) [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [arendaja](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [kiire](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL-I 2014 SP1**|Windows Server 2012 R2|[Ettevõtte](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL-I 2014**|Windows Server 2012 R2|[Ettevõtte](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 HOOLDUSPAKETI SP3**|Windows Server 2012 R2|[Ettevõtte](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Ettevõtte](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Ettevõtte](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL-I 2008 R2 HOOLDUSPAKETI SP3**|Windows Server 2008 R2|[Ettevõtte](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 HOOLDUSPAKETT SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Variant 2: Looge SQL-i VM olemasoleva litsentsiga
Samuti saate tuua oma litsents (BYOL). Selle stsenaariumi korral maksate ilma mis tahes täiendavate SQL serveri litsentsimise VM. Kasutada oma litsentsi, kasutage SQL serveri versiooni, versioonide ja operatsioonisüsteemid allpool maatriks. Portaalis, on need pildi nimed eesliide **{BYOL}**.

|Versioon|Operatsioonisüsteem|Väljaande|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] BYOL VM piltide kasutamiseks peab teil olema ja Enterprise lepingu [Litsentsi mobiilsus Software Assurance Azure kaudu](https://azure.microsoft.com/pricing/license-mobility/). Samuti vajate kehtivat litsentsi versioon/väljaanne SQL Server, mida soovite kasutada. Peate [BYOL vajalikku teavet Microsoftile](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) teie VM ettevalmistamise **10** päeva jooksul.

## <a name="manage-your-sql-vm"></a>Teie SQL-i VM haldamine
Pärast ettevalmistamise oma SQL serveri VM, on mitu valikuline haldamisega seotud toiminguid. Palju aspekte, konfigureerimine ja haldamine täpselt samamoodi, nagu haldate asutusesisese SQL serveri eksemplar SQL serveri. Kuid on mõned tööülesanded Azure. Järgmistes jaotistes rõhutada neid valdkondi tuleks koos linkidega lisateabe juurde.

### <a name="connect-to-the-vm"></a>Ühenduse loomine VM
Üks kõige halduse Põhitoimingud on ühenduse loomiseks oma SQL serveri VM tööriistad, nt SQL Server Management Studio (SSMS) kaudu. Oma uue SQL serveri VM kaudu ühenduse loomise juhised leiate teemast [ühenduse loomine abil SQL serveri virtuaalse masina Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Andmete migreerimine
Kui teil on olemasoleva andmebaasiga, peaksite teisaldamiseks äsja ettevalmistatud SQL-i VM. Migreerimise Valikud ja juhiseid loendi leiate teemast [on Azure VM SQL serveri andmebaasi migreerimine](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Kõrge-saadavus konfigureerimine
Kui vajate kõrge-saadavus, kaaluge konfigureerimine SQL serveri kättesaadavus rühmad. See hõlmab mitme Azure VMs virtuaalse võrku. Azure'i portaalis on malli, mis teile selle konfiguratsiooni häälestab. Lisateavet leiate teemast [konfigureerimine ressursihaldur Azure'i virtuaalmasinates kättesaadavusrühma](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Kui soovite oma kättesaadavus rühma ja seotud kuulajale käsitsi konfigureerimine, lugege teemat [Konfigureerimine Kättesaadavusrühmad Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Teiste kättesaadavuse peaksite arvesse võtma, vt [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Andmete varundamine
Azure'i VMs saate [Automaatse varundamise](virtual-machines-windows-sql-automated-backup.md), mis loob regulaarselt varukoopiaid andmebaasi bloobimälu ära. Saate kasutada ka käsitsi selle meetodi. Lisateabe saamiseks lugege teemat [kasutamine Azure SQL serveri varundamiseks ja taastamiseks](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Kõik varundus ja taaste suvandid ülevaate leiate teemast [varundus ja taaste SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Värskenduste automatiseerimine
Azure'i VMs saate kasutada [Automatiseeritud lappimine](virtual-machines-windows-sql-automated-patching.md) kavandada hoolduse installimine Windowsi jaoks ja SQL serveri värskendused automaatselt.

### <a name="customer-experience-improvement-program-ceip"></a>Klientide programmikasutuskogemuse täiustamise kava (CEIP)
Klientide programmikasutuskogemuse täiustamise kava (CEIP) on vaikimisi lubatud. See saadab perioodiliselt aruandeid Microsoft SQL serveri täiustamiseks. Ei ole halduse ülesanne nõutav koos CEIP juhul, kui soovite välja lülitada pärast ettevalmistamise. Saate kohandada või keelata CEIP-s, luues ühenduse kaugtöölaua VM. Käivitage **SQL Serveri tõrge ja Poliitikakasutuse aruannete** kasuliku. Järgige keelamiseks teatamine. 

Lisateavet leiate jaotisest CEIP [Aktsepteerida litsentsitingimused läbi](https://msdn.microsoft.com/library/ms143343.aspx) teema. 

## <a name="next-steps"></a>Järgmised sammud
SQL Server Azure'i virtuaalmasinates [Uuri Õppeteema](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) .

Kas on veel küsimusi? Esmalt lugege teemat [SQL Server Azure'i Virtuaalmasinates KKK](virtual-machines-windows-sql-server-iaas-faq.md). Kuid lisada ka oma küsimused või kommentaarid suhelda Microsoft ja ühenduse SQL-i VM teemasid lõppu.
