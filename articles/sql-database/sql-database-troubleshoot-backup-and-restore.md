<properties
    pageTitle="Tõrkeotsing varundamine ja taastamine Azure SQL-i andmebaasiga"
    description="Saate teada, kuidas pilve andmebaasi taastamine tõrgete ja katkestuste kasutamine Azure'i SQL-andmebaasi varukoopiate ja koopiad."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Andmebaasi taastamine ajas eelmises punktis, kustutatud andmebaasi taastamine või andmete keskmist katkestuste taastamine

SQL-andmebaasi hoiab teie andmebaasi koopiad nii, et saab taastada katkestuste ja kasutajale tõrge. Saadaolevad suvandid sõltuvad andmebaasi teenuse kiht ja valige suvandid. Vt [Äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md) üksikasjad ja kujundamine.

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Andmebaasi taastamiseks eelmises punktis ajas
1.  Klõpsake [Azure portaali](https://azure.microsoft.com/) **SQL andmebaase**.
2.  Valige loendist andmebaas ja seejärel klõpsake käsku **Taasta**.
3.  Tippige uue andmebaasi nimi, valige kuupäev ja kellaaeg taastada, ja seejärel klõpsake nuppu **loomine.**
4.  Tehke rakenduse muudatused nii, et viide uue andmebaasi. Vt [punkti ajas andmebaasi taastada](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Rakenduse kogemata kustutatud andmebaasi taastamine
1.  Klõpsake [Azure portaali](https://azure.microsoft.com/) **SQL-i serverid**.
2.  Valige server, millega majutatud loendist andmebaas.
3.  Enne Server, liikuge kerides allapoole ja käsku **Kustutatud andmebaasid**.
4.  Valige andmebaasi taastamine ja seejärel klõpsake nuppu **Loo**.
5.  Tehke rakenduse muudatused nii, et viide uue andmebaasi. Lugege teemat [Kustutatud andmebaasi taastada](sql-database-recovery-using-backups.md#deleted-database-restore).

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Piirkondliku andmekeskuse katkestuste taastamine
Standard- ja Premium andmebaasidega, kui häälestate geo kopeeritud sekundaaride, saate taastada nende sekundaaride abil. See võimaldab on vähem võimalik andmekao andmebaasi taastamine. Üksikasjad leiate [Azure'i SQL-andmebaasiga, kasutades automatiseeritud andmebaasi taastada](sql-database-disaster-recovery.md) .

Azure'i pakub varukoopiaid iga andmebaasi teises regioonis (geograafilise liigne varukoopia). Neid varukoopiaid, mida nimetatakse Geo-taastamine, saate luua uue andmebaasi, kuid on tõenäoline, et teil kogemusi andmekao, kui seda meetodit eraldi.

**Taastada andmebaasi geo-taastamine abil:**

- [Azure portaali](https://azure.microsoft.com/)nuppu **Uus**, valige **andmed ja salvestusruumi**, klõpsake käsku **SQL-andmebaasi**ja valige allikana andmebaasi **varundamine** . Üksikasjad leiate [Azure'i SQL-andmebaasiga, mis katkestuste taastada](sql-database-disaster-recovery.md) .