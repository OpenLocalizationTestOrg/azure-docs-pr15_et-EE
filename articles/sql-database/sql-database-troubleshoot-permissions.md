<properties
    pageTitle="Kuidas teha administraatori ülesannete, nt administraatori parooli lähtestamine | Microsoft Azure'i"
    description="Kirjeldab, kuidas SQL-andmebaasis levinud haldustoimingute sooritamiseks. Näiteks administraatori parooli lähtestamine, mis ja juurdepääsu keelamine."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="administraatori parooli lähtestamine"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Kuidas teha levinud haldustoiminguid, näiteks Azure'i SQL-andmebaasi administraatori parooli lähtestamine
Selles teemas kiirtoimingud abil anda ja juurdepääsu Azure SQL-andmebaasi eemaldamine. Põhjalikumat teavet, lugege teemat:

- [Andmebaasid ja sisselogimise Azure'i SQL-andmebaasi haldamine](sql-database-manage-logins.md)
- [SQL-andmebaasi turvamine](sql-database-security.md)
- [Turbekeskus andmebaasimootor SQL Server ja Azure SQL-andmebaas](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Kui soovite loogilise serveri administraatori parooli lähtestamine

- [Azure portaali](https://portal.azure.com) käsku **SQL-i serverid**, valige loendist server ja seejärel klõpsake nuppu **Lähtesta parool**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Et aidata kindel ainult volitatud IP aadressid on lubatud serverit
- Vt [kohta: tulemüüri sätete konfigureerimine SQL-andmebaasi](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>Kasutaja andmebaasi keskkonnas andmebaasi kasutajate loomiseks
- Kasutage [Luua kasutaja](https://msdn.microsoft.com/library/ms173463.aspx) aruande ja vaadata [Sisalduvad andmebaasi kasutajate - teha oma andmebaasi Portable](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Suletud andmebaasi kasutajate autentimiseks oma Azure Active Directory abil
- Teemast [Azure Active Directory autentimist kasutades SQL-andmebaasiga ühenduse loomine](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>Täiendavad sisselogimise kõrge suurte õigustega kasutajad virtuaalse põhi andmebaasi loomiseks
- Kasutage [Luua LOGIN](https://msdn.microsoft.com/library/ms189751.aspx) avalduse ja jaotisest haldamise sisselogimise [andmebaaside haldamine](sql-database-manage-logins.md) ja Azure'i SQL-andmebaasi sisselogimise üksikasju.
