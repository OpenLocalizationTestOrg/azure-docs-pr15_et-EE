   <properties
   pageTitle="Andmete laadimine SQL Azure'i andmebaas | Microsoft Azure'i"
   description="Siit saate teada, tavastsenaariumid andmete laadimine SQL-i andmebaas. Nendeks PolyBase, Azure'i bloobimälu, tasapinnalise failide ja ketta saatmine. Saate kasutada ka kolmanda osapoole tööriistu."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Andmete laadimine SQL Azure'i andmebaas

Kokkuvõte stsenaarium suvandid ja soovitused andmete laadimine SQL-i andmebaas.

Andmete laadimise raskem osa on andmete ettevalmistamine tavaliselt laadi. Azure'i lihtsustab laadimise, kasutades Azure'i bloobimälu levinud andmesalve paljude teenuste jaoks, ja kasutades Azure'i andmed Factory korraldab side- ja Azure teenuste vahel liikumine. Need protsessid on integreeritud PolyBase tehnoloogia, mis kasutab oluliselt paralleelne töötlemine (MPP) andmete paralleelselt Azure'i bloobimälu laadimiseks SQL-i andmebaas. 

Õpetused, mille laadimine valimi andmebaase, leiate teemast [koormus Näidisettevõtte andmebaasi][].

## <a name="load-from-azure-blob-storage"></a>Azure'i bloobimälu alla laadida
Kiireim viis andmete importimine SQL-i andmebaas on kasutada PolyBase Azure'i bloobimälu andmete laadimiseks. PolyBase kasutab SQL-i andmebaas oluliselt paralleelne töötlemine (MPP) design alla laadida andmeid paralleelselt Azure'i bloobimälu. PolyBase kasutamiseks saate kasutada T-SQL-i käskude või mõne Azure'i andmed Factory kohaletoimetamisel.

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase ja T-SQL-i kasutamine

Kokkuvõte laadimise protsess.

2. Oma andmete vormindamisel UTF-8, kuna PolyBase ei toeta praegu UTF-16.
2. Andmete teisaldamine Azure'i bloobimälu ja salvestada selle tekstifailidest.
3. SQL-i andmebaas määratleda asukoht ja andmete vorming väliste objektide konfigureerimine
4. Uue andmebaasi tabeli andmed paralleelselt laadimiseks T-SQL-käsu käivitava.

<!-- 5. Schedule and run a loading job. --> 

Juhendi, leiate [Azure'i bloobimälu SQL-i andmebaas (PolyBase) abil andmete laadimine][].

### <a name="2-use-azure-data-factory"></a>2. Kasutage Azure'i andmed Factory

Lihtsam viis PolyBase kasutada, saate luua Azure'i andmed Factory kohaletoimetamisel, mis kasutab PolyBase andmete Azure'i bloobimälu laadimiseks SQL-i andmebaas. See on küll kiire konfigureerida, kuna te ei ole vaja T-SQL-i objektide määratlemine. Kui teil on vaja see importimata välisandmete päringu, kasutage T-SQL-i. 

Kokkuvõte laadimise protsess.

2. Oma andmete vormindamisel UTF-8, kuna PolyBase ei toeta praegu UTF-16.
2. Andmete teisaldamine Azure'i bloobimälu ja salvestada selle tekstifailidest.
3. Saate luua mõne Azure'i andmed Factory müügivõimaluste neelata andmed. Kasutage suvandi PolyBase.
4. Plaanimine ja käivitage tulemas.

Juhendi, leiate [Azure'i bloobimälu SQL-i andmebaas (Azure'i andmed Factory) abil andmete laadimine][].


## <a name="load-from-sql-server"></a>Laadi SQL serverist
Saate kasutada andmete SQL Serverist SQL-i andmebaas laadimiseks Integration Services (SSIS), tasapinnalise faile või tarne ketast Microsoftile. Lugege edasi, et leiate ülevaate erinevate laadimise protsesside ja õppeteemade lingid.

Täielik andmete migratsiooni SQL serveri SQL-i andmebaas leping, vaadake [migreerimise ülevaade][]. 

### <a name="use-integration-services-ssis"></a>Kasutage Integration Services (SSIS)
Kui kasutate juba Integration Services (SSIS) pakettide laadimiseks SQL serveri, saate värskendada oma pakettide sihtkohana SQL serveri lähte-kui ka SQL-i andmebaas kasutamiseks. See on kiire ja lihtne teha, ja on hea valik, kui üritate pole teie laadimise protsessi kasutada andmete juba pilves migreerimiseks. Miinuseks on laadi aeglasem kui kasutada PolyBase, sest see SSIS täida laadi paralleelselt.

Kokkuvõte laadimise protsess.

1. Parandage oma Integration Services pakett osutamiseks allika SQL serveri eksemplar ja SQL-i andmebaas andmebaasi sihtkoht.
2. Kui see pole juba migreerida SQL-i andmebaas, oma skeemi.
3. Muuda vastendust oma pakette kasutada ainult SQL-i andmebaas Toetatavad andmetüübid.
3. Plaanimine ja paketi käivitamine.

Õpetuse, leiate teemast [laadi andmete SQL Server Azure'i SQL-i andmete ladu (SSIS)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Kasutage AZCopy (soovitatav < 10 TB andmete jaoks)
Kui teie andmete maht on < 10 TB, te saate eksportida andmed SQL Serverist tasapinnalise faile, Azure'i bloobimälu failide kopeerimine ja andmete laadimine SQL-i andmebaas PolyBase abil

Kokkuvõte laadimise protsess.

1. SQL serveri andmete eksportimine tasapinnalise failide bcp käsurea kasuliku abil.
2. Andmete kopeerimisel tasapinnalise failidest Azure'i bloobimälu AZCopy käsurea kasuliku abil.
3. Kasutage PolyBase SQL-i andmebaas laadimiseks.

Juhendi, leiate [Azure'i bloobimälu SQL-i andmebaas (PolyBase) abil andmete laadimine][].

### <a name="use-bcp"></a>Kasutage bcp
Kui teil on small suurt hulka andmeid saate bcp SQL Azure'i andmebaas laadimiseks.

Kokkuvõte laadimise protsess.
1. SQL serveri andmete eksportimine tasapinnalise failide bcp käsurea kasuliku abil.
2. Kasutage bcp andmete laadimiseks tasapinnalise faile otse, et SQL-i andmebaas.

Õpetuse, leiate teemast [laadi andmete SQL serveri (bcp) SQL Azure'i andmebaas][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Kasutage impordi/ekspordi (soovitatav > 10 TB andmete jaoks)
Kui teie andmete maht on > 10 TB ja soovite liikuda Azure'i, soovitame kasutada meie ketta osutava [Impordi/ekspordi][]. 

Kokkuvõte laadimise protsess
2. SQL serveri andmete eksportimine tasapinnalise failide edasikantavast kettale bcp käsurea kasuliku abil.
3. Ketast Microsoftile saata.
4. Microsoft laadib andmed SQL-andmebaas

## <a name="load-from-hdinsight"></a>Hdinsightist laadimine
SQL-i andmebaas toetab laadimise andmete hdinsightist PolyBase kaudu. Protsess on sama, mis andmete laadimine: Azure'i bloobimälu – Hdinsightiga laadimiseks andmete ühendamine PolyBase abil. 

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase ja T-SQL-i kasutamine

Kokkuvõte laadimise protsess.

2. Oma andmete vormindamisel UTF-8, kuna PolyBase ei toeta praegu UTF-16.
2. Andmete teisaldamine Hdinsightiga ja salvestada selle tekstifailid, ORC või parkett-vormingus.
3. Konfigureerida väliste objektide SQL-i andmebaas määratleda asukoht ja andmete vorming.
4. Uue andmebaasi tabeli andmed paralleelselt laadimiseks T-SQL-käsu käivitava.

Juhendi, leiate [Azure'i bloobimälu SQL-i andmebaas (PolyBase) abil andmete laadimine][].

## <a name="recommendations"></a>Soovitused

Paljud partnerite on laadimise lahendusi. Rohkem teada saada, vaadake meie [lahenduse partnerite][]loendi. 

Kui teie andmed on pärit-relatsiooniliste andmeallikate ja soovite SQL-i andmebaas laadimiseks peate muuta ridade ja veergude enne selle laadimist. Ümber andmed ei pea olema talletatud andmebaasi, seda saab talletada tekstifailidest.

Luua statistika äsja laaditud andmed. SQL Azure'i andmebaas ei veel tugiteenuste automaatne loomine või statistika värskendamise automaatselt.  Päringute parimaid tulemusi saada, on oluline, et luua statistika kõigi veergude kõigi tabelite pärast esimest laadi või olulisi muudatuste andmeid.  Lisateavet leiate teemast [statistika][].


## <a name="next-steps"></a>Järgmised sammud
Lisateavet arengu leiate näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[Laadi andmetega Azure'i bloobimälu SQL-i andmebaas (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Laadi andmetega Azure'i bloobimälu SQL-i andmebaas (Azure'i andmed Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Andmete laadimine SQL Serverist Azure SQL-i andmete ladu (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Andmete laadimine SQL serverist, et (bcp) SQL Azure'i andmebaas]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Näidisettevõtte andmebaasi laadimine]: ./sql-data-warehouse-load-sample-databases.md
[Migreerimise ülevaade]: ./sql-data-warehouse-overview-migrate.md
[lahendus partnerid]: ./sql-data-warehouse-partner-business-intelligence.md
[arengu ülevaade]: ./sql-data-warehouse-overview-develop.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/eksport]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
