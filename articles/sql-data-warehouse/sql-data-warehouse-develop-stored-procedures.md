<properties
   pageTitle="Salvestatud toimingute SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid salvestatud toimingute arendamise lahenduste rakendamisel SQL Azure'i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>Salvestatud toimingute SQL-andmebaas

SQL-i andmebaas toetab paljude SQL serveri Transact-SQL-i funktsioone. Olulisem on skaala välja funktsioonid, mis soovime kasutada jõudluse teie lahendus.

Säilitamiseks ulatuse ja jõudluse SQL-i andmebaas on siiski ka mõned ja funktsioonid, mis on käitumist erinevused ja teised, mis pole toetatud.

Selles artiklis selgitatakse, kuidas rakendada salvestatud toimingute sees SQL-i andmebaas.

## <a name="introducing-stored-procedures"></a>Salvestatud toimingute tutvustus
Salvestatud toimingute on suurepärane viis versioon SQL-koodi; salvestamine selle lähedale andmete andmebaas. Mida kapseldamist koodi mõistliku ühikutes salvestatud toimingute arendajad modularize nende lahendused; Siit leiate juhendajaga suurem korduvkasutatavuse koodi. Iga salvestatud protseduur nõus ka parameetrid veelgi paindlikuks muuta.

SQL-i andmebaas pakub liht- ja täiustatud salvestatud toimingu rakendamine. SQL serveri võrreldes suurim erinevus on salvestatud protseduur ei ole eelnevalt kompileeritud kood. Andmete laos oleme üldiselt vähem seotud koostamise ajal. Oluline veel salvestatud protseduur koodi õigesti optimeeritud töötamisel vastu suurte andmemahtudega. Eesmärk on salvestada tundide, minutite ja sekundite ei millisekundit. Seetõttu on kasulik mõelda salvestatud toimingute ümbriste SQL-i loogika.     

Kui SQL-i andmebaas aktiveeritakse teie salvestatud protseduur SQL-lauseid on sõeluda, tõlgitud ja optimeeritud käitusajal. Selle toimingu käigus teisendatakse iga lause jaotatud päringud. SQL-koodi, mis on tegelikult käivitada andmete erineb esitatud päringule.

## <a name="nesting-stored-procedures"></a>Pesastamine salvestatud toimingute
Kui salvestatud toimingute kõne muud salvestatud toimingute või käivitada dünaamiline SQL-i, siis öeldakse olla ka pesastatud sisemise salvestatud toimingu või koodi kutsumise.

SQL-i andmebaas toetab kuni 8 Pesastustasemete. See on veidi teistsugused SQL serveriga. SQL serveri pesa tase on 32.

Ülemise taseme salvestatud protseduur kõne võrdub pesastada tase 1

```sql
EXEC prc_nesting
```
Kui salvestatud protseduur võimaldab kõne teisele EXEC siis suureneb pesa tase 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Kui teist protseduuri seejärel aktiveeritakse mõned dünaamiline sql siis suureneb pesa tase 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Märkus SQL-i andmebaas ei toeta praegu @@NESTLEVEL. Peate hoida silma peal oma pesa tasemel ise. Tõenäoliselt klõpsamist 8 pesa taseme limiit, kuid kui te ei tee peate uuesti tööle oma koodi ja "Ühenda" nii, et see mahuks selle piirmäära.

## <a name="insertexecute"></a>LISA. KÄIVITADA
SQL-i andmebaas ei luba teil tulemite hulk salvestatud protseduuri abil lisada lause kasutamine. On siiski saate kasutada alternatiivse lähenemisviisi.

Lugege järgmises artiklis [Ajutised tabelid] näitena selle kohta, kuidas seda teha.

## <a name="limitations"></a>Piirangud

Seal on mõned elemendid Transact-SQL-is talletatud toimingutest, mida ei rakendata SQL-i andmebaas.

Need on:

- ajutiste salvestatud toimingute
- nummerdatud salvestatud toimingute
- laiendatud salvestatud toimingute
- CLR-i salvestatud toimingute
- suvand krüptimist
- suvand dispersioonanalüüs
- tabelis hinnatud parameetrid
- kirjutuskaitstud parameetrid
- Vaikimisi parameetrid
- täitmise kontekste
- lause tagasi

## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[Ajutised tabelid]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[arengu ülevaade]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
