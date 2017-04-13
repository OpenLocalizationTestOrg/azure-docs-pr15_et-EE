<properties
   pageTitle="SQL Azure'i andmebaas autentimise | Microsoft Azure'i"
   description="Azure Active Directory (AAD) ja SQL serveri autentimine SQL Azure'i andmebaas."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>SQL Azure'i andmebaas autentimine

> [AZURE.SELECTOR]
- [Turvalisuse ülevaade](sql-data-warehouse-overview-manage-security.md)
- [Autentimine](sql-data-warehouse-authentication.md)
- [Krüptimist (portaal)](sql-data-warehouse-encryption-tde.md)
- [Krüptimise (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Ühenduse loomine SQL-i andmebaas, et peate läbima turvalisus mandaadi autentimise eesmärgil. Ühenduse tuvastamisel teatud ühenduse sätted on konfigureeritud loomise päringu seansi osana.  

Turvalisus ja teie andmebaas ühenduste lubamise kohta leiate lisateavet teemast [Secure andmebaasi SQL-i andmebaas][].

## <a name="sql-authentication"></a>SQL-i autentimine
Ühenduse SQL-i andmebaas, peate sisestama järgmise teabe:

- Täielikult kvalifitseeritud serverinimi
- SQL-i autentimise määramine
- Kasutajanimi
- Parooli
- Vaikimisi andmebaasi (valikuline)

Vaikimisi loob ühenduse *juhtslaidi* andmebaasi ja pole andmebaasi kasutaja. Andmebaasiga ühenduse loomiseks oma kasutaja, saate teha ühte järgmistest.

- Määrake vaikimisi andmebaasi serverisse registreerumist SQL serveri objekti Explorer SSDT, SSMS, või oma rakenduse ühendusstring. Näiteks lisada InitialCatalog parameeter ODBC-ühendus.
- Tõstke esile kasutaja andmebaasi enne SSDT seansi loomine.

> [AZURE.NOTE] Andmebaasi ühenduse muutmise Transact-SQL-lause **kasutamine MyDatabase;** ei toetata. Ühenduse loomine SQL-i andmebaas koos SSDT juhiseid, lugege artiklit [päringu Visual Studio abil][] .

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD) autentimine

[Azure Active Directory] [ What is Azure Active Directory] autentimine on ühenduse loomine Microsoft Azure'i SQL-i andmebaas, kasutades identiteedid Azure Active Directory (Azure AD). Azure Active Directory autentimine, saate ühes keskses kohas hallata andmebaasi kasutajate identiteet ja muude Microsofti teenuste ühes keskses kohas. Keskse ID haldamise ühe koha, SQL-i andmebaas kasutajate haldamine ja lihtsustab õiguste haldust. 

### <a name="benefits"></a>Eelised

Azure Active Directory eelised on järgmised.

- SQL serveri autentimine alternatiiv pakub.
- Aitab peatada kasutaja identiteedid leviku andmebaasi serverites.
- Lubab parooli pööre ühes kohas
- Andmebaasi õiguste abil välise (AAD) rühmade haldamine.
- Kõrvaldab võimaldab integreeritud Windowsi autentimist ja muud liiki autentimine, ei toeta Azure Active Directory salvestamist paroolid.
- Kasutab sisalduvad andmebaasi kasutajad autentida identiteedid andmebaasi tasemel.
- Luba autentimise toetab rakenduste ühenduse SQL-i andmebaas.
- SQL Server Management Studio toetab mitmikautentimise Active Directory ühes kohas autentimise kaudu. Mitmikautentimise kirjeldust vt [SSMS kasutajatugi Azure AD MFA SQL-andmebaas ja SQL-i andmebaas](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory on veel suhteliselt uus ja on mõned piirangud. Veenduge, et Azure Active Directory on hea sobib teie keskkonnas, leiate [Azure'i AD funktsioonid ja piirangud][], spetsiaalselt täiendavad peaksite arvesse võtma.

### <a name="configuration-steps"></a>Konfigureerimistoimingute

Järgmiste juhiste abil Azure Active Directory autentimise konfigureerimine.

1. Luua ja määrata Azure Active Directory
2. Valikuline: Seostada või mis on seostatud tellimuse Azure active directory muutmine
3. Loomine SQL Azure'i andmebaas Azure Active Directory administraator.
4. Teie klientarvutite konfigureerimine
5. Vastendatud Azure AD identiteedid andmebaasi keskkonnas andmebaasi kasutajate loomine
6. Ühenduse loomine oma andmebaas, kasutades Azure AD identiteedid

Praegu ei kuvata SSDT objekti Exploreri Azure Active Directory kasutajad. Lahendusena saate vaadata nende kasutajate [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
  
### <a name="find-the-details"></a>Otsige üksikasjad
- Täitke üksikasjalikud juhised. Üksikasjalikud juhised konfigureerimine ja kasutamine Azure Active Directory autentimine on peaaegu identsed Azure'i SQL-andmebaasi ja Azure SQL-i andmebaas. Üksikasjalikud juhised [ühenduse loomine SQL-andmebaasi või SQL-i andmete ladu, kasutades Azure Active Directory autentimise](../sql-database/sql-database-aad-authentication.md)teema.
- Luua kohandatud andmebaasi rollid ja kasutajate lisamine rollid. Seejärel anda Varundustöö õigusi rollid. Lisateabe saamiseks lugege teemat [Alustamine andmebaasi mootor õigused](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Järgmised sammud

Päringute teie andmebaas Visual Studio ja muude rakendustega käivitamise leiate [päringu Visual Studio abil][].

<!-- Article references -->
[Turvaline andmebaasi SQL-i andmebaas]: ./sql-data-warehouse-overview-manage-security.md
[Päring koos Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure'i AD funktsioonid ja piirangud]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
