<properties
    pageTitle="Andmebaaside venitamine lubatud taastamine | Microsoft Azure'i"
    description="Saate teada, kuidas taastada venitamine\-lubatud andmebaasid."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Venitamine lubatud andmebaasi taastamine

Andmebaasi taastamine mitut tüüpi tõrkeid, tõrgete ja katastroofide vajaduse korral on tagatud taastada.

Varundamise kohta lisateabe saamiseks vaadake [andmebaasi varukoopia venitamine lubatud](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Varundus on ainult üks osa täieliku kättesaadavuse ja järjepidevuse ärilahenduse. Kõrge-saadavus kohta lisateabe saamiseks vt [Kõrge-saadavus lahendusi](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>SQL serveri andmete taastamine
Riistvara tõrke või rikutud taastamiseks taastamine venitada\-lubatud SQL serveri andmebaasi varukoopia põhjal. Kasutage SQL serveri taastamine viise, mida praegu kasutada saate jätkata. Lisateavet leiate teemast [taastamine ja taastamise ülevaade](https://msdn.microsoft.com/library/ms191253.aspx).

Kui taastate SQL Serveri andmebaasiga, peate salvestatud protseduur **sys.sp_rda_reauthorize_db** taastada venitada vahelise ühenduse\-lubatud SQL Serveri andmebaas ja Azure kaugandmebaasiga. Lisateavet leiate teemast [SQL Serveri andmebaas ja Azure kaugandmebaasiga vahelise ühenduse taastamine](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database).

## <a name="restore-your-remote-azure-data"></a>Remote Azure andmete taastamine

### <a name="recover-a-live-azure-database"></a>Reaalajas Azure'i andmebaasi taastamine
SQL serveri andmebaasi venitamine teenuse klõpsake Azure hetktõmmiseid kõik reaalajas andmed vähemalt iga 8 tunni Azure'i salvestusruumi hetktõmmiseid abil. Nende näidete hallatakse 7 päevaks. See võimaldab teil taastada andmed üks vähemalt 21 punkti ajal kui viimase hetktõmmise võeti kuni viimase 7 päeva jooksul.

Reaalajas Azure'i andmebaasi taastamine varasemasse aega, kasutades Azure portaali, teha järgmisi toiminguid.

1. Azure portaali sisse logida.
2. Klõpsake ekraani vasakus servas valige **SIRVI** ja seejärel valige **SQL andmebaase**.
3. Avage andmebaas ja valige see.
4. Andmebaasi tera ülaosas nuppu **Taasta**.
5. Saate määrata uue **andmebaasi nimi**, valige **Taastamise** ja seejärel klõpsake nuppu **Loo**.
6. Andmebaasi taastamine protsessi alguses ja saab jälgida **teatised**.

### <a name="recover-a-deleted-azure-database"></a>Kustutatud Azure'i andmebaasi taastamine
SQL serveri venitamine andmebaasi Azure kulub andmebaasi hetktõmmise enne andmebaasi katkeb ja säilitab 7 päevaks. Pärast teate, enam jääb hetktõmmiseid reaalajas andmebaasist. Nii saate taastada kustutatud andmebaasi punkti, kui see on kustutatud.

Kustutatud Azure'i andmebaasi taastamine punkt, kui see on kustutatud, kasutades Azure portaali, teha järgmisi toiminguid.

1. Azure portaali sisse logida.
2. Klõpsake ekraani vasakus servas valige **SIRVI** ja seejärel valige **SQL-i serverid**.
3. Liikuge oma serveri ja valige see.
4. Liikuge kerides allapoole jaotiseni oma serveri blade toiminguid, klõpsake paani **Kustutatud andmebaasid** .
5. Valige kustutatud andmebaasi, mille soovite taastada.
5. Määrake uue **andmebaasi nimi** ja klõpsake nuppu **Loo**.
6. Andmebaasi taastamine protsessi alguses ja saab jälgida **teatised**.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>SQL Serveri andmebaas ja Azure kaugandmebaasiga vahelise ühenduse taastamine

1.  Kui kavatsete erineva nimega või teises regioonis taastatud Azure andmebaasiga ühenduse loomiseks, käivitage salvestatud protseduur [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) eelmise Azure'i andmebaasi ühenduse katkestamine.  

2.  Salvestatud protseduur [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) kohaliku venitamine uuesti käivitada\-lubatud andmebaasi Azure'i andmebaas.  

    -   Pakkuda olemasoleva andmebaasiga, kus otsinguulatuseks on määratud mandaat on sysname või on muutuv märk\(128\) väärtus. \(Ärge kasutage varchar\(max\).\) Saate vaadata identimisteavet nime vaates **sys.database\_rakendatud\_mandaadi**.  

    -   Saate määrata, kas remote andmed kopeerida ja luua ühenduse Kopeeri (soovitatav).  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Vt ka

[Haldamine ja venitamine andmebaasi tõrkeotsing](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Ja SQL serveri andmebaasi taastamine](https://msdn.microsoft.com/library/ms187048.aspx)
