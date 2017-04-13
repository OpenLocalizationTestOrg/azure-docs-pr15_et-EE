<properties
    pageTitle="Levinud probleemide ühenduse Azure SQL-andmebaasiga"
    description="Juhised ära tunda ja lahendada Azure SQL-i andmebaasi ühendus levinud tõrkeid."
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

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Azure'i SQL-andmebaasiga ühenduse tõrkeotsing

Azure'i SQL-andmebaasi ühendus nurjub, kuvatakse [tõrketeade](sql-database-develop-error-messages.md). See artikkel kehtib keskse teema, mis aitab teil Azure'i SQL-andmebaasi ühenduvuse probleemide tõrkeotsing. See toob [ühise põhjustab](#cause) ühenduvusega probleeme, soovitab [tõrkeotsingu tööriista](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , mis aitab teil identiteedi probleem, ja pakutakse tõrkeotsingujuhiseid [siirdamiseks tõrgete](#troubleshoot-transient-errors) ja [püsivate või - mööduv tõrgete](#troubleshoot-the-persistent-errors)lahendamiseks. Lõpetuseks, loetletakse [leiate Azure'i SQL-andmebaasi ühenduvusprobleemide kõik oluline artiklitest](#all-topics-for-azure-sql-database-connection-problems).

Kui teil tekib ühenduvusega probleeme, proovige tõrkeotsing toiminguid, mis on selles artiklis kirjeldatud.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Põhjus

Ühendusega seotud probleemide võib olla põhjustatud järgmistest:

- Tõrge rakendamiseks kujundus juhised ja head tavad kujundamise käigus.  Vt [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md) alustada.
- Azure'i SQL-andmebaasi ümberkonfigureerimine
- Tulemüüri sätted
- Ühenduse ajalõpp
- Vale sisselogimisteave
- Lubatud jõudnud mõned Azure'i SQL-andmebaasi ressursid

Üldiselt Azure SQL-andmebaasiga ühenduse probleemid võib liigitada järgmiselt:

- [Siirdamiseks tõrked (lühiajaline või vahelduv)](#troubleshoot-transient-errors)
- [Püsivate või -mööduv tõrked (vead regulaarselt korduvaks)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Proovige Azure'i SQL-andmebaasi ühenduvuse probleemide tõrkeotsing

Kindla ühenduse tõrke ilmnemisel proovige [seda tööriista](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), mis aitavad teil kiiresti identiteedi ja teie probleemi lahendada.

## <a name="troubleshoot-transient-errors"></a>Siirdamiseks tõrkeotsing
Kui teie taotlus on praegu ajutine tõrkeid, lugege järgmistest teemadest näpunäiteid selle kohta, kuidas vähendada nende vigade ja tõrkeotsing.

- [Tõrkeotsingu andmebaasi &lt;x&gt; serveris &lt;y&gt; ei ole saadaval (tõrge: 40613)](sql-database-troubleshoot-connection.md)
- [Tõrkeotsing, diagnoosimine ja SQL-i andmebaasi SQL-i ühenduse tõrgete ja siirdamiseks tõrgete vältimiseks](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Püsivate tõrkeotsing (-mööduv vead)

Rakenduse rikub Azure SQL-i andmebaasiga ühenduse loomiseks, näitab tavaliselt probleeme ühte järgmistest:

- Tulemüüri konfigureerimine. SQL Azure'i andmebaasi või kliendipoolne tulemüür blokeerib ühendused Azure'i SQL-andmebaasi.
- Võrgu ümberkonfigureerimine kliendi poolel: näiteks uus IP-aadress või puhverserverit.
- Kasutaja tõrge: näiteks valesti tipitud ühenduse parameetreid, näiteks serveri nime ühendusstring.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Püsivate ühenduvusprobleemide lahendamiseks

1.  [Tulemüüri reeglid](sql-database-configure-firewall-settings.md) seadistatud lubama kliendi IP-aadress.
2.  Klõpsake kõigi tulemüürid klient ja Interneti-ühendus, veenduge, et port 1433 on avatud Väljaminevad ühendused. Vaadata täiendavaid kursoreid [konfigureerimine Windowsi tulemüüri lubada SQL serveri juurdepääsu](https://msdn.microsoft.com/library/cc646023.aspx) .
3.  Kontrollige oma ühendusstring ja muude ühenduse sätted. [Ühenduvuse probleemide teema](sql-database-connectivity-issues.md#connections-to-azure-sql-database)jaotisest ühendusstring.
4.  Märkige ruut teenuste seisundi armatuurlaud. Kui arvate, et on piirkondliku katkestuste, vaadata, [taastada ka katkestuste](sql-database-disaster-recovery.md) juhised uue alale taastada.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Kõigi teemade Azure'i SQL-andmebaasi ühendus probleemid

Järgmises tabelis on loetletud iga ühenduse probleemi teema, mis kehtib Azure'i SQL-andmebaasi teenuse.


| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 1 | [Azure'i SQL-andmebaasiga ühenduse tõrkeotsing](sql-database-troubleshoot-common-connection-issues.md) | See on sihtleht ühenduvuse tõrkeotsingu Azure SQL-andmebaasis. See kirjeldab tuvastamine ja siirdamiseks tõrgete ja püsivate või -mööduv tõrgete lahendamine Azure'i SQL-andmebaasi. |
| 2 | [Tõrkeotsing, diagnoosimine ja SQL-i andmebaasi SQL-i ühenduse tõrgete ja siirdamiseks tõrgete vältimiseks](sql-database-connectivity-issues.md) | Saate teada, kuidas tõrkeotsing, diagnoosimine ja vältida siirdamiseks tõrge Azure SQL andmebaasis või SQL-i ühenduse tõrge. |
| 3 | [Üldised siirdamiseks viga käsitlemine juhised](best-practices-retry-general.md) | Annab üldised juhised siirdamiseks viga töötlemine Azure SQL-andmebaasiga ühenduse loomisel. |
| 4 | [Microsoft Azure'i SQL-andmebaasi ühendusprobleemide tõrkeotsing](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Selle tööriista abil identiteedi probleemi lahendada ühenduse tõrkeid. |
| 5 | [Tõrkeotsing: "andmebaasi &lt;x&gt; serveris &lt;y&gt; pole praegu saadaval. Proovige hiljem ühendus"tõrge](sql-database-troubleshoot-connection.md) | Kirjeldab, kuidas tuvastada ja lahendada 40613 tõrge: "andmebaasi &lt;x&gt; serveris &lt;y&gt; pole praegu saadaval. Proovige uuesti ühendust hiljem." |
| 6 | [SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes: andmebaasi ühenduse tõrge ja ka muude probleemide](sql-database-develop-error-messages.md) | SQL-i tõrkekoodide kohta pakub SQL-andmebaasi klientrakendustes, nt levinud andmebaasi ühenduse tõrkeid, andmebaasi Kopeeri probleemid ja üldisi tõrkeid. |
| 7 | [Azure'i SQL-andmebaasi jõudluse juhiseid ühe andmebaaside jaoks](sql-database-performance-guidance.md) | Antakse juhiseid, et aidata teil otsustada, milline teenuse kiht sobib teie rakendus. Soovitused rakenduse parimal viisil Azure'i SQL-andmebaasi häälestamine pakub. |
| 8 | [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md) | Sisaldab linke koodinäiteid jaoks eri tehnoloogiaid, mille abil saate ühendada ja suhelda Azure'i SQL-andmebaasi. |
| 9 | Üleminek versioonile Azure'i SQL-andmebaasi v12 lehe ([Azure'i portaalis](sql-database-upgrade-server-portal.md), [PowerShelli](sql-database-upgrade-server-powershell.md)) | Antakse juhiseid olemasoleva Azure SQL-i andmebaasi V11 servers ja andmebaaside uuendamise Azure SQL-i andmebaasi v12 Azure portaali või PowerShelli abil. |


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i SQL-andmebaasi jõudlusega seotud probleemide tõrkeotsing](sql-database-troubleshoot-performance.md)
- [Azure'i SQL-andmebaasi õigused probleemide tõrkeotsing](sql-database-troubleshoot-permissions.md)
- [Kõik teemad teenuse Azure SQL-andmebaas](sql-database-index-all-articles.md)
- [Microsoft Azure'i dokumentatsiooni otsimine](http://azure.microsoft.com/search/documentation/)
- [Azure'i SQL-andmebaasi teenusega uusimate värskenduste kuvamine](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md)
- [Üldised siirdamiseks viga käsitlemine juhised](../best-practices-retry-general.md)
- [Andmeühenduste teegid SQL-andmebaas ja SQL Server](sql-database-libraries.md)
- [Azure'i SQL-andmebaasi kasutamise õppeteema](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Õppeteema kasutamise elastne andmebaasi funktsioonid ja tööriistad](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
