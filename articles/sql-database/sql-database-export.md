<properties
    pageTitle="Azure'i SQL-andmebaasiga BACPAC faili Azure'i portaalis arhiivimine"
    description="Azure'i SQL-andmebaasiga BACPAC faili Azure'i portaalis arhiivimine"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Azure'i SQL-andmebaasiga BACPAC faili Azure'i portaalis arhiivimine

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-export.md)
- [PowerShelli](sql-database-export-powershell.md)

Selles artiklis antakse juhiseid SQL Azure'i andmebaasi BACPAC faili (salvestatud Azure'i bloobimälu) arhiivimiseks [Azure portaali](https://portal.azure.com).

Kui teil on vaja luua Arhiiv Azure'i SQL-andmebaasiga, saate eksportida andmebaasi skeemi ja andmete BACPAC faili. BACPAC faili on lihtsalt ZIP faili laiendiga BACPAC. BACPAC faili saab salvestada hiljem Azure'i bloobimälu või kohaliku salvestusruumi kohapealse asukoht ja hiljem imporditud tagasi dokumenti SQL Server Azure'i SQL-andmebaasi või kohapealse installi. 

***Kaalutlused***

- Ülekandega ühtlustamist arhiivi jaoks peab tagate, kas see ei kirjuta esineb tegevuse käigus ekspordi või ekspordite SQL Azure'i andmebaasi [ülekandega ühtsete koopia](sql-database-copy.md) .
- Azure'i bloobimälu arhiivitakse BACPAC faili maksimummaht on 200 GB. Kasutage suuremat BACPAC faili kohaliku salvestusruumi arhiivimiseks [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) käsuviiba kasuliku. Selle kasuliku teenusega nii Visual Studio ja SQL serveri. Samuti saate [alla laadida](https://msdn.microsoft.com/library/mt204009.aspx) SQL Server Data Tools saada see kasuliku uusim versioon.
- Arhiivimine Azure premium mäluruumi BACPAC faili abil ei toetata.
- Kui eksporditoimingu ületab 20 tundi, võib see tühistada. Ekspordi ajal jõudluse suurendamiseks saate teha järgmist.
 - Oma teenuse taseme ajutine suurendamine
 - Kõik lugemine ja kirjutamine tegevuse käigus ekspordi lõpetada.
 - Kõik suurte tabelite mittenullväärtusi on [rühmitatud indeksi](https://msdn.microsoft.com/library/ms190457.aspx) abil. Rühmitatud registrid, ilma ekspordi võib nurjuda, kui kulub üle 6 / 12 tunni. See on ekspordi teenuse peab lõpule viima tabeli skannimine ja proovige eksportida kogu tabel. Käivitage **DBCC SHOW_STATISTICS** ja veenduge, et *peale RANGE_HI_KEY suurima* ei ole tühi ja selle väärtus on hea jaotuse on hea viis kindlaks teha, kui tabelitel on optimeeritud ekspordi. Lisateavet leiate teemast [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).


> [AZURE.NOTE] BACPACs ei ole mõeldud kasutada varundamise ja taastamise toimingud. Azure'i SQL-andmebaasi loob automaatselt iga kasutaja andmebaasi varundada. Lisateavet leiate teemast [Äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md).

Selles artiklis lõpuleviimiseks vajate järgmist:

- Azure'i tellimuse.
- Azure'i SQL-andmebaas. 
- [Azure'i Standard salvestusruumi konto](../storage/storage-create-storage-account.md) bloobimälu container on BACPAC talletamiseks kindlad.

## <a name="export-your-database"></a>Andmebaasi eksportimine

Avage andmebaas, mida soovite eksportida SQL-andmebaasi höövlitera.

> [AZURE.IMPORTANT] Selleks, et tagada ülekandega ühtsete BACPAC faili tuleks [luua andmebaasi koopia](sql-database-copy.md) esimese ja seejärel eksportige andmebaasi koopia. 

1.  Avage [Azure'i portaalis](https://portal.azure.com).
2.  Klõpsake nuppu **SQL-i andmebaasid**.
3.  Klõpsake andmebaasi arhiivida.
4.  Klõpsake SQL-andmebaasi labale **eksportimine** tera **eksportimine andmebaasi** avamiseks.

    ![nupp ekspordi][1]

5.  **Salvestusruum** ja valige oma soovitud BACPAC talletuskohaks salvestusruumi konto ja bloobimälu ümbrises.

    ![Andmebaasi eksportimine][2]

6. Valige oma autentimistüüp. 
7.  Sisestage mandaat sobiv autentimise sisaldava ekspordite andmebaasi Azure SQL-i serveri.
8.  Klõpsake nuppu **OK** , et arhiivimine andmebaas. Nupu **OK klõpsamist** loob taotluse ekspordi andmebaas ja teenuse esitab. Aeg, mis viivad ekspordi sõltub suurus ja keerukus andmebaasi ja oma teenuste tase. Saate teate.

    ![ekspordi teatis][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Eksporditoimingu edenemise jälgimine

1.  Klõpsake nuppu **SQL-i serverid**.
2.  Klõpsake sisaldav algse (allikas) andmebaasi saate arhiivida ainult server.
3.  Liikuge kerides jaotiseni toimingud.
4.  Klõpsake SQL serveri tera **impordi/ekspordi ajalugu**.

    ![impordi ekspordi ajalugu][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Veenduge, et selle BACPAC on teie salvestusruumi ümbrises

1.  Klõpsake **salvestusruumi kontod**.
2.  Klõpsake kontot salvestusruumi talletuskohaks BACPAC arhiivi.
3.  Klõpsake **ümbriste** ja valige ümbris saate eksportida andmebaasi üheks üksikasjad (saate alla laadida ja salvestada BACPAC siin).

    ![.bacpac faili üksikasjad][5]  

## <a name="next-steps"></a>Järgmised sammud

- Azure'i SQL-andmebaasi lisamine BACPAC importimise kohta leiate artiklist [importimine on BACPCAC Azure SQL-andmebaasi](sql-database-import.md)
- SQL Serveri andmebaasiga on BACPAC importimise kohta leiate artiklist [importimine on BACPCAC SQL Serveri andmebaasiga](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

