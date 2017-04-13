<properties
    pageTitle="Kopeerige SQL Azure'i andmebaas | Microsoft Azure'i"
    description="Azure'i SQL-andmebaasi koopia loomine"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Kopeerige Azure'i SQL-andmebaas

> [AZURE.SELECTOR]
- [Ülevaade](sql-database-copy.md)
- [Azure'i portaal](sql-database-copy-portal.md)
- [PowerShelli](sql-database-copy-powershell.md)
- [T-SQL-IS](sql-database-copy-transact-sql.md)

Azure'i [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md) abil saate luua SQL-andmebaasi koopia. Andmebaasi koopia kasutab funktsiooni geo-dispersioonanalüüs samal tehnoloogial. Kuid erinevalt geo-dispersioonanalüüs see lõpeb dispersioonanalüüs link, mis, kui külvist etapp on lõpule viidud. Seetõttu on kopeerimine andmebaasi hetktõmmise lähteandmebaasi Kopeeri taotluse ajast.  
Saate luua eri server või sama serveri andmebaasi koopia. Taseme- ja jõudluse teenusetaseme (hinnakirjad taseme) andmebaasi koopia on sama, mis vaikimisi allika andmebaasi. API kasutamisel saate valida erinevaid jõudlusega sees sama teenuse kiht (väljaanne). Kui koopia on lõpule jõudnud, muutub Kopeeri töökorras, mis ei sõltu andmebaasi. Selles etapis saate täiendamine või alandada selle mis tahes edition. Sisselogimise, kasutajad ja õigused saab hallata ükshaaval.  

Andmebaasi kopeerimisel sama loogilise server sama sisselogimise saab kasutada nii andmebaasid. Peamine abil saate kopeerida andmebaasi turvalisuse muutub andmebaasi omanik (DBO) uue andmebaasi. Andmebaasi koopia kopeeritakse kõik andmebaasi kasutajad, nende õiguste ja turbe sisaldab identifikaatori (sid).  

Andmebaasi kopeerimisel eri loogilise server muutub põhisumma uues serveris turvalisuse uue andmebaasi andmebaasi omanik. Kui kasutate [sisalduvad andmebaasi kasutajate](sql-database-manage-logins.md) jaoks andmepääsu tagab nii põhi- ja andmebaasid on alati sama kasutajatunnust nii, et pärast lõpetamist Kopeeri kohe pääsete sellele sama identimisteabega. Kui kasutate [Azure Active Directory](../active-directory/active-directory-whatis.md), saate haldamise mandaadi Kopeeri vaja täielikult eemaldada. Siiski andmebaasi kopeerimisel uue serveri login vastavalt juurdepääs üldiselt ei tööta kuna selle sisselogimise ei ole uut serveris. Teemast [Azure SQL-i andmebaasi turvasätete pärast avariitaastet](sql-database-geo-replication-security-config.md) haldamise andmebaasi kopeerimisel eri loogilise server sisselogimise kohta leiate. 

Klõpsake kopeeritavat SQL-andmebaasi vajate järgmist:

- Azure'i tellimuse. Kui teil on vaja Azure tellimuse lihtsalt **Tasuta PROOVIVERSIOON** selle lehe ülaosas nuppu ja siis naaske artiklit lõpuleviimiseks.
- SQL-andmebaasi kopeerida. Kui te ei ole SQL-andmebaasi, luua ühe selles artiklis toodud juhiseid järgides: [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md).

## <a name="next-steps"></a>Järgmised sammud

- Leiate Azure'i portaalis andmebaasi kopeerimiseks [kopeerige Azure'i portaalis Azure SQL-i andmebaas](sql-database-copy-portal.md) .
- Teemast [Azure SQL-andmebaasi koopia PowerShelli kaudu](sql-database-copy-powershell.md) kopeerimiseks andmebaasi PowerShelli kaudu.
- Vaadake [Kopeeri Azure'i SQL-andmebaasiga abil T-SQL-i](sql-database-copy-transact-sql.md) abil Transact-SQL-i andmebaasi kopeerimiseks.
- [SQL Azure'i andmebaasi turvasätete avariitaastet pärast](sql-database-geo-replication-security-config.md) andmebaasi kopeerimisel eri loogilise server kasutajate ja sisselogimise haldamise kohta leiate teemast.



## <a name="additional-resources"></a>Lisaressursid

- [Sisselogimise haldamine](sql-database-manage-logins.md)
- [Ühenduse loomine SQL-andmebaasi SQL Server Management Studio ja valimi T-SQL-päringu tegemine](sql-database-connect-query-ssms.md)
- [Andmebaasi eksportimine mõnda BACPAC](sql-database-export.md)
- [Äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md)
- [SQL-andmebaasi dokumentatsioon](https://azure.microsoft.com/documentation/services/sql-database/)
