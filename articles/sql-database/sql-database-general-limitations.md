<properties
   pageTitle="Juhised ja SQL Azure'i andmebaasi üldine piirangud"
   description="Sellel lehel kirjeldatakse üldised piirangud Azure'i SQL-andmebaasi, samuti interoperability ja tugi."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>Juhised ja SQL Azure'i andmebaasi üldine piirangud

Sellest teemast leiate juhised üldised piirangud ja Azure SQL-i andmebaasi. Piirmäära, ressursside haldamine ja tugiteenuste aru, leiate [täiendavaid materjale](#additional-guidelines) selle teema lõpus.

## <a name="connectivity-and-authentication"></a>Ühenduvus ja autentimine

  - Ei toetata Windowsi autentimist. Vaadake teemat [andmebaase ja Azure SQL-andmebaasis sisselogimise haldamine](sql-database-manage-logins.md). Azure Active Directory autentimine on toetatud siiski teatud piirangud. Teemast [Azure Active Directory autentimise SQL-andmebaasiga ühendust luua](sql-database-aad-authentication.md).

  - Microsoft Azure'i SQL-andmebaasi toetab tabelina esitatud andmed voo (TDS) protokolli kliendi 7.3 või uuem versioon.

  - Ainult TCP/IP ühendused on lubatud.

  - SQL Server 2008 SQL serveri brauserit ei toetata, kuna Microsoft Azure'i SQL-andmebaasi ei ole dünaamiline pordid, ainult port 1433.

## <a name="sql-server-agentjobs"></a>SQL Serveri Agent/tööde haldamine

Microsoft Azure'i SQL-andmebaasi ei toeta SQL Server Agent, aga elastne töö abil saate käivitada töö üle ühe palju andmebaasidele. Elastne tööde kohta leiate lisateavet teemast [elastne tööde haldamine](sql-database-elastic-jobs-overview.md).

## <a name="sql-server-collation-support"></a>SQL serveri võrdlemine tugi

Microsoft Azure'i SQL-andmebaasi kasutavad vaikimisi andmebaasi võrdlemine on **SQL_LATIN1_GENERAL_CP1_CI_AS**, kus **LATIN1_GENERAL** on inglise (USA), **CP1** on koodi lehe 1252, **CI** on väiketähed ja **AS** on rõhk tundliku. Ei ole võimalik muuta V12 andmebaaside võrdlemine. Funktsiooni võrdlemine seadmise kohta leiate lisateavet teemast [EKSEMPLARHAAVAL (Transact-SQL-i)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Nime andmise nõuded

Teatud kasutaja nimed ei ole lubatud turvalisuse põhjustel. Te ei saa kasutada järgmisi nimesid:

 - **administraator**
 - **administraator**
 - **külalisena**
 - **juur**
 - **sa**

Kõigi uute objektide nimed peavad vastama identifikaatorite SQL serveri reeglid. Lisateavet leiate teemast [identifikaatorid](https://msdn.microsoft.com/library/ms175874.aspx).

Lisaks kasutajanime ja kasutaja nimed ei tohi sisaldada soovitud \ märk (Windowsi autentimist ei toetata).

## <a name="additional-guidelines"></a>Täiendavad juhised

- Käesolevas artiklis toodud üldised piirangud, lisaks SQL-andmebaasi on teatud serveriressursikvootide ja põhinevad teie **teenuse kiht**piirangud. Teenuse astme ülevaate leiate teemast [SQL-andmebaasi teenuse astme](sql-database-service-tiers.md).

- Muud SQL-andmebaasi piirangud, leiate [Azure'i SQL-i andmebaasi ressursi piirangud](sql-database-resource-limits.md).

- Turvalisus seotud suuniste teemast [Azure SQL-i andmebaasi turvalisuse juhised ja piirangud](sql-database-security-guidelines.md).

- Muu seotud ala ümbritseb mis Azure SQL-andmebaas on SQL Server, nt SQL Server 2014 ja SQL Server 2016 kohapealse versioonidega ühilduvuse. Azure'i SQL-andmebaasi V12 uusim versioon on teinud palju parandusi selle ala. Lisateavet leiate teemast [mis on uut rakenduses SQL-i andmebaasi V12](sql-database-v12-whats-new.md).

- Juht-saadavus ja SQL-andmebaasi toe kohta leiate teavet teemast [Andmeühenduste teegid SQL-andmebaas ja SQL Server](sql-database-libraries.md).
