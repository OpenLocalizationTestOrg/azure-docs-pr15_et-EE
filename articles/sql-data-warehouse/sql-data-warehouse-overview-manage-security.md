<properties
   pageTitle="Turvaline andmebaasi SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid turvaliseks SQL Azure'i andmebaas andmebaasi arendamise lahendusi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Turvaline andmebaasi SQL-i andmebaas

> [AZURE.SELECTOR]
- [Turvalisuse ülevaade](sql-data-warehouse-overview-manage-security.md)
- [Autentimine](sql-data-warehouse-authentication.md)
- [Krüptimise (portaal)](sql-data-warehouse-encryption-tde.md)
- [Krüptimise (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Selles artiklis tutvustatakse põhitõdesid turvaliseks andmebaasi SQL Azure'i andmebaas. Eelkõige artiklit aitavad teil alustada ressurssidega jaoks juurdepääsu piiramise andmete kaitsmiseks ja järelevalve andmebaasi.

## <a name="connection-security"></a>Ühenduse turvalisus

Ühenduse turvalisus viitab kuidas saate piirata ja turvalist ühendust andmebaasi tulemüüri reeglid ja ühenduse krüptimise abil.

Tulemüüri reeglid kasutatakse ühenduse katsete IP-aadressid, mis ei ole konkreetselt whitelisted hüljata nii server ja andmebaas. Luba ühendused rakenduse või kliendi seadme avaliku IP-aadress, peate looma serveritaseme tulemüüri reegli Azure portaali, REST API-ga või PowerShelli abil. Hea tava, tuleks piirata võimalikult palju teie serveri tulemüüri kaudu lubatud IP-aadresside vahemikud.  Juurde pääseda SQL Azure'i andmebaas, kohalikust arvutist, tagada võrgu-ja kohaliku arvuti tulemüüri võimaldab väljamineva TCP port 1433.  Lisateavet leiate [Azure'i SQL-andmebaasi tulemüüri][], [sp_set_firewall_rule][]ja [sp_set_database_firewall_rule][].

Vaikimisi on krüptitud ühendused oma SQL-andmebaas.  Muutmine ühenduse sätted keelamiseks krüptimise ignoreeritakse.

## <a name="authentication"></a>Autentimine

Kuidas tuvastada teie isikut andmebaasiga ühenduse loomisel viitab autentimist. SQL-i andmebaas toetab praegu SQL serveri autentimine kasutajanime ja parooli ning Azure Active Directory. 

Kui lõite loogiline serveri andmebaasi, määratud "serveri administraator" login kasutajanime ja parooliga. Nende mandaadi abil saate autentida mis tahes andmebaasi selles serveris andmebaasi omanik või "dbo" kaudu SQL serveri autentimine.

Hea tava, peaks teie ettevõtte kasutajad mõne muu kontoga siiski autentida kasutada. Saate rakenduse õiguste piiramiseks ja riske pahatahtlik tegevus juhuks, kui rakenduse koodis on juurdepääs SQL süst rünnak sellisel viisil. 

SQL serveri autenditud kasutaja loomiseks serverisse serveri administraator oma kasutajanimega **juhtslaidi** andmebaasiga ühenduse loomiseks ja looge uus serveri logi sisse.  Lisaks on hea mõte põhi andmebaasi SQL Azure'i andmebaas kasutajate jaoks kasutajakonto loomiseks. Juhtslaidi loomine kasutaja saab kasutaja sisselogimine tööriistadega nagu SSMS ilma andmebaasi nimi.  See lubab ka need objekti Exploreri abil saate vaadata kõiki andmebaaside SQL serveris.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Seejärel ühenduse **SQL-i andmebaas andmebaasi** serveri administraator oma kasutajanimega ja põhjal serveri login äsja loodud andmebaasi kasutaja loomine.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Kui kasutaja teeme täiendavad toimingud, nt loomise sisselogimise või luua uue andmebaasi, samuti peavad olema määratud soovitud `Loginmanager` ja `dbmanager` põhi andmebaasi rollid. Nende täiendavate rollid ja autentimine SQL-andmebaasi kohta leiate lisateavet teemast [andmebaaside haldamine ja sisselogimise Azure SQL-andmebaasis][].  Lisateavet Azure AD jaoks SQL-i andmebaas, leiate teemast [ühenduse loomine SQL-i andmete ladu, kasutamine Windows Azure Active Directory autentimist][].


## <a name="authorization"></a>Luba

Luba viitab, mida saate teha mõnda SQL Azure'i andmebaas andmebaasis ja see kontrollib oma kasutajakonto rolli rühmakuuluvus ja õigused. Hea tava, tuleks anda kasutajatele vähemalt vajalikke õigusi. SQL Azure'i andmebaas teeb see lihtne hallata rollide T-SQL-i abil.

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Serveri administraator konto, millega loote kuulub db_owner, mis on asutuse sees andmebaasi midagi tegema. Saate salvestada selle konto kasutamise skeemi uuendamine ja muude toimingute kohta. Piiratud õigustega kontoga "ApplicationUser" vähimate õiguste, rakenduse jaoks vajalik andmebaasi rakenduse kaudu suhtlemiseks.

On võimalust täpsemaks piiritlemiseks, mida kasutaja saab teha Azure'i SQL-andmebaasi.

- Varundustöö [õiguste][] abil saate määrata, milliseid toiminguid saate teha üksikute veergude, tabelite, vaadete, toiminguid ja muude objektide andmebaasi. Varundustöö õiguste abil kõige kontrolli ja anda vajalikud miinimumõigused. Varundustöö õiguste süsteem on veidi keeruline ja mõned uuring tõhusaks kasutamiseks on vaja.
- [Andmebaasi rollid][] peale db_datareader ja db_datawriter saab luua võimsamaid rakenduse Kasutajakontod või vähem võimas halduse kontod. Sisseehitatud fikseeritud andmebaasi rollid pakkuda lihtne viis anda õigused, kuid see võib põhjustada andmise rohkem õigusi, kui on vaja.
- [Salvestatud toimingute][] saab piirata toiminguid, mida tuleks andmebaasi.

Azure'i klassikaline portaalist andmebaasid ja loogiline serverid haldamine või kasutades Azure ressursihaldur API kontrollib portaali kasutaja konto rollimääranguid. Selle teema kohta leiate lisateavet teemast [Rollipõhine juurdepääsu reguleerimine Azure'i portaalis][].

## <a name="encryption"></a>Krüptimine

Azure SQL-i andmete ladu läbipaistev krüptimise (TDE) kaitseb pahatahtlik tegevus ohtu täites reaalajas krüptimine ja dekrüptimine oma andmete ülejäänud.  Andmebaasi krüptimisel krüptitakse seotud varukoopiate ja tehingulogi faile nõudmata rakenduste muudatused. TDE krüptib kogu andmebaasi salvestusruumi sümmeetriline võti nimega andmebaasi krüptovõtme abil. SQL-andmebaasis on kaitstud andmebaasi krüptovõtme sisemine Serveri sert. Sisseehitatud Serveri sert on kordumatu iga SQL-andmebaasi server. Microsoft pöörab need serdid automaatselt iga 90 päeva. SQL-i andmebaas on krüptimisalgoritmi on AES 256. TDE üldise kirjelduse leiate [Läbipaistvaid andmete krüptimine][].

Saate oma andmebaasi [Azure portaali] krüptida[ Encryption with Portal] või [T-SQL-i][Encryption with TSQL].

## <a name="next-steps"></a>Järgmised sammud

Lisateavet ja näiteid ühenduse SQL-i andmebaas erinevate protokollide, vt [ühenduse loomine SQL-i andmebaas][].

<!--Image references-->

<!--Article references-->
[Ühenduse loomine SQL-andmebaas]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Ühenduse loomine SQL-i andmebaas Azure Active Directory autentimist kasutades]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure'i SQL-andmebaasi tulemüüri]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Andmebaasi rollid]: https://msdn.microsoft.com/library/ms189121.aspx
[Andmebaasid ja sisselogimise Azure'i SQL-andmebaasi haldamine]: https://msdn.microsoft.com/library/ee336235.aspx
[Õigused]: https://msdn.microsoft.com/library/ms191291.aspx
[Salvestatud toimingute]: https://msdn.microsoft.com/library/ms190782.aspx
[Läbipaistva andmete krüptimine]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Rollipõhine juurdepääsu reguleerimine Azure'i portaalis]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
