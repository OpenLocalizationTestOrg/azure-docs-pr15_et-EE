<properties
    pageTitle="SQL Server Azure'i turvakaalutlused | Microsoft Azure'i"
    description="Selles teemas viitab ressursse, mis on loodud mudeliga klassikaline juurutamise ja annab üldised juhised turvaliseks SQL serveris, kus töötab mõni Azure virtuaalse masina."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
   editor=""    
   tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>SQL Server Azure'i Virtuaalmasinates turvakaalutlused
 
See teema sisaldab üldise turvalisuse juhised, mis aitavad luua turvaline juurdepääs SQL Serveri eksemplari on Azure VM. Siiski parema kaitse Azure SQL serveri andmebaasi eksemplaride tagamiseks soovitame juurutada Azure traditsiooniline kohapealse turvalisus tavad lisaks turvalisuse põhitõed.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


SQL serveri turvalisus tavade kohta lisateabe saamiseks vt [SQL Server 2008 R2 parimad - funktsionaalseid ja haldustoimingute](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure'i vastab mitu valdkonna eeskirjad ja standardid, mis võimaldab teil luua virtuaalse masina töötab SQL serveriga ühilduv lahenduse. Azure nõuetele vastavuse kohta leiate teemast [Azure Usalduskeskus](https://azure.microsoft.com/support/trust-center/).

Järgmine loend annab ülevaate turvalisus soovitused, mida tuleks käsitleda kui konfigureerimise ja ühenduse SQL Server Azure'i VM-i eksemplari.

## <a name="considerations-for-managing-accounts"></a>Haldamise kontod asjaoluga:

- Luua kordumatu kohaliku administraatorikonto, mis on nimega **administraator**.

- Kõigi kontode jaoks kasutada keerukate keerukad paroolid. Lisateavet selle kohta, kuidas luua keeruka parooli, leiate teemast [näpunäited keerukate paroolide loomine](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) artikkel.

- Vaikimisi valib Azure SQL serveri virtuaalse masina häälestamise ajal Windowsi autentimist. Seetõttu **SA** login on keelatud ja häälestuse on määratud parool. Me soovitame **SA** login peaks olema ei saa kasutada või lubatud. Järgnevalt on alternatiivne strateegiad SQL sisselogimine soovi.
    - Mis on süsteemiadministraator liikmelisuse SQL-i konto loomine.
    - Kui kasutate **SA** sisselogimist, login lubamine ja nimetage see ümber ja määrata uus parool.
    - Mõlema variandi, mis olid eelpool jaoks on vaja muuta autentimisrežiim **SQL serveri ja Windowsi autentimisrežiimis**. Lisateavet leiate teemast [Muutmine serveri autentimisrežiim](https://msdn.microsoft.com/library/ms188670.aspx).

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Kaalutlused tagamiseks Azure virtuaalse masina ühendused

- Kaaluge [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) virtuaalmasinates avaliku RDP pordid asemel haldamiseks.

- [Võrgu turberühma](../virtual-network/virtual-networks-nsg.md) (NSG) abil saate lubada või keelata võrguliikluse virtual arvuti. Kui soovite kasutada mõnda NSG ja on lõpp ACL juba olemas, eemaldage esmalt ACL-i lõpp-punkti. Täpsemat teavet selle kohta, kuidas seda teha, [Haldamine Accessi juhtelement on loetletud (ACL) jaoks lõpp-punktid PowerShelli abil](../virtual-network/virtual-networks-acl-powershell.md).

- Kui kasutate lõpp-punktid, eemaldada mis tahes lõpp-punktid virtual arvutisse, kui te ei kasuta neid. Juhised lõpp-punktid ACL-ID abil, vt [haldamine ACL lõpp-punkti](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Krüptitud ühendust suvand lubamine eksemplar SQL serveri andmebaasi mootor Azure'i Virtuaalmasinates. SQL serveri eksemplar allkirjastatud serdi konfigureerimine. Lisateabe saamiseks vt [Lubamine krüptitud ühendused andmebaasimootor](https://msdn.microsoft.com/library/ms191192.aspx) ja [Ühenduse stringi süntaks](https://msdn.microsoft.com/library/ms254500.aspx).

- Kui teie virtuaalmasinates tuleks kasutada ainult teatud võrgust, kasutage Windowsi tulemüüri juurdepääsu piiramiseks teatud IP-aadresside või alamvõrku.

## <a name="next-steps"></a>Järgmised sammud

Kui te olete huvitatud ümber jõudluse põhitõed, lugege teemat [Jõudluse head tavad SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-performance.md).

Muud teemad, mis töötab SQL Server Azure'i VMs seotud leiate [Azure'i Virtuaalmasinates ülevaade SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).
