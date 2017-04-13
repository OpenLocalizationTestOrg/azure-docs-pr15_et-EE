<properties
    pageTitle="SQL Server Azure'i Virtuaalmasinates KKK | Microsoft Azure'i"
    description="Sellest artiklist leiate vastused korduma kippuvatele küsimustele, mis töötab SQL Server Azure'i VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server Azure'i Virtuaalmasinates KKK

Sellest teemast leiate vastused kõige levinum küsimusi töötab [SQL Server Azure'i Virtuaalmasinates](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

1. **Kuidas luua mõne Azure virtuaalse masina SQL serveriga?**

    On selleks kaks võimalust. Lihtsaim lahendus on luua virtuaalse masina, mis sisaldab SQL serveri. Azure'i kasutajaks ja loomine SQL-i VM portaalis, juhendi leiate [sätte virtuaalse masina SQL Server Azure'i portaal](virtual-machines-windows-portal-sql-server-provision.md). Teil on ka võimalus käsitsi installimine SQL serveri VM ja diagrammide korduvkasutamine on kohapealse litsentsi koos [Litsentsi mobiilsus Software Assurance Azure kaudu](https://azure.microsoft.com/pricing/license-mobility/).

1. **Mis vahe VMs SQL-i ja SQL-andmebaasi teenus on?**

    Põhimõtteliselt töötab SQL Serveri arvutis Azure virtuaalne erineb pole, mis töötab SQL serveri remote andmekeskuses. [SQL-andmebaasi](../sql-database/sql-database-technical-overview.md) pakub seevastu andmebaasi-kui-a-service. SQL-andmebaasiga, kus teil pole juurdepääsu oma andmebaasi majutavad masinad. Täielik võrdlus, lugege teemat [Valige SQL Server suvand pilv: (PaaS) Azure SQL-i andmebaasi või SQL Server Azure'i VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Kuidas migreerida minu asutusesisese SQL serveri andmebaasi pilve?**

    Azure'i virtuaalarvuti luua esmalt SQL serveri eksemplar. Seejärel migreerida oma kohapealse andmebaaside selle eksemplari. Andmete migreerimise strateegiad, vt [SQL Server on Azure VM SQL serveri andmebaasi migreerimine](virtual-machines-windows-migrate-sql.md).

2. **Saate muuta installitud funktsioonid või sama VM on teine SQL serveri eksemplar installida?**

    Jah. SQL serveri installikandjalt asub kaustas **C** -ketas. Käivita **Setup.exe** sinna lisada uue SQL Serveri eksemplari või muud seadmesse installitud funktsioonid SQL serveri muuta.

3. **Kuidas täiendada versiooniks SQL Server Azure'i VM-i uus versioon/väljaanne?**

    Praegu ei ole versiooniuuenduse SQL Server töötab ka Azure VM. Luua uue Azure virtuaalse masina soovitud SQL serveri versioon/Edition ja seejärel uus server, kasutades standard [andmete migreerimise meetodite abil](virtual-machines-windows-migrate-sql.md)oma andmebaasi migreerimine.

4. **Kuidas installida SQL serveri eksemplar litsentsitud on Azure VM?**

    SQL serveri installikandjalt kopeerimiseks Windows Server VM ja seejärel installida SQL serveri VM. Litsentside põhjust, peab teil olema [Litsentsi mobiilsus Software Assurance Azure kaudu](https://azure.microsoft.com/pricing/license-mobility/).

5. **Kas teil on SQL-i kulud VM maksma, kui seda kasutatakse ainult puhkerežiimis olev/Tõrkesiirde?**

    Kui loote SQL-i VM Galerii, peab teil olema eraldi litsentsi valige SQL-i VM ja selle hinnad on sama. Kui installite SQL serveri virtual arvutisse, kuhu [Litsentsi mobiilsus](https://azure.microsoft.com/pricing/license-mobility/)käsitsi, on teil võimalus on üks tasuta passiivne SQL-i eksemplari Tõrkesiirde. Palun vaadake üle kitsenduste ja.

6. **Kuidas värskenduste ja hoolduspakettide rakendatakse SQL serveri VM?**

    Virtuaalmasinates anna saate juhtida hosti kohapeal, sh, millal ja kuidas värskendused. Operatsioonisüsteemi, saate rakendada käsitsi Windowsi värskenduste või saate lubada ajastamise teenust nimega [Automatiseeritud lappimine](virtual-machines-windows-classic-sql-automated-patching.md). Automaatne lappimine installe värskendusi, mis on tähistatud oluline, sh SQL serveri värskenduste kategooria. Muude valikuliste värskenduste SQL serveri peab olema installitud käsitsi.

7. **On võimalik luua konfiguratsioone virtuaalse masina galeriis (nt Windows 2008 R2 + SQL Server 2012) ei kuvata?**

    Ei. Virtuaalse masina Galerii pilte, mis sisaldavad SQL serveri, valige üks esitatud pildid.

9. **Kuidas installida SQL-i Andmeriistad minu Azure VM?**

    Laadige alla ja installige [Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313)SQL-i Andmeriistad.

## <a name="resources"></a>Ressursid

SQL Server Azure'i Virtuaalmasinates ülevaate saamiseks vaadake video [Azure VM on parim platvorm, SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Samuti saate hea Sissejuhatus teema, [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).

Muud ressursid järgmised.

- [SQL Server Azure'i portaal virtuaalse masina ettevalmistamine](virtual-machines-windows-portal-sql-server-provision.md)
- [SQL Server Azure'i VM andmebaasi migreerimine](virtual-machines-windows-migrate-sql.md)
- [Kõrge-saadavus ja avariitaastet SQL Server Azure'i virtuaalmasinates](virtual-machines-windows-sql-high-availability-dr.md)
- [Jõudluse SQL Server Azure'i Virtuaalmasinates head tavad](virtual-machines-windows-sql-performance.md)
- [Rakenduse mustrite ja SQL Server Azure'i Virtuaalmasinates arengu strateegiad](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
