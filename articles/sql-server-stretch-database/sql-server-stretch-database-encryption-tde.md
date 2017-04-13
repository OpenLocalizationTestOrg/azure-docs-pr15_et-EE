<properties
   pageTitle="SQL serveri venitus andmebaasi Azure läbipaistvaid andmete krüptimine (TDE) | Microsoft Azure'i"
   description="SQL serveri venitus andmebaasi Azure läbipaistvaid andmete krüptimine (TDE)"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Venitus andmebaasi Azure läbipaistvaid andmete krüptimine (TDE) lubamine
> [AZURE.SELECTOR]
- [Azure'i portaal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Läbipaistev krüptimise (TDE) kaitseb pahatahtlik tegevus ohtu nõudmata rakenduse muudatusi reaalajas krüptimine ja dekrüptimine andmebaasi, seotud varukoopiate ja tehingulogi failide veebisaidil ülejäänud järgige järgmisi juhiseid.

TDE krüptib kogu andmebaasi salvestusruumi sümmeetriline võti nimega andmebaasi krüptovõtme abil. Krüptovõtme andmebaas on kaitstud sisemine Serveri sert. Sisseehitatud Serveri sert on kordumatu iga Azure'i server. Microsoft pöörab need serdid automaatselt iga 90 päeva. TDE üldise kirjelduse leiate [Läbipaistev krüptimise (TDE)].

##<a name="enabling-encryption"></a>Krüptimise lubamine

Lubamiseks on Azure TDE andmebaasi, mis on andmete säilitamise migreeritud venitamine lubatud SQL Serveri andmebaas, tehke järgmised üksused:

1. Avage andmebaas, [Azure'i portaal](https://portal.azure.com)
2. Andmebaasi tera, klõpsake nuppu **sätted**
3. Valige suvand **läbipaistev andmete krüptimine**![][1]
4. Valige säte **sisse** ja seejärel valige **Salvesta**
![][2]


##<a name="disabling-encryption"></a>Krüptimise keelamine

TDE keelamiseks on Azure'i andmebaasi, mis on andmete säilitamise migreeritud venitamine lubatud SQL serveri andmebaasi, teha järgmisi toiminguid:

1. Avage andmebaas, [Azure'i portaal](https://portal.azure.com)
2. Andmebaasi tera, klõpsake nuppu **sätted**
3. Valige suvand **läbipaistev andmete krüptimine**
4. Valige säte **välja** ja seejärel valige **Salvesta**




<!--Anchors-->
[Läbipaistva andmete krüptimine (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
