<properties
    pageTitle="Saate luua ja hallata mastaabitud välja SQL Azure'i andmebaasid, kasutades elastne tööd | Micosoft Azure"
    description="Tutvustavad loomist ja haldamist ka elastne andmebaasi töö."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Saate luua ja hallata mastaabitud välja SQL Azure'i andmebaasid, kasutades elastne tööd (eelvaade)

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShelli](sql-database-elastic-jobs-powershell.md)


**Elastne andmebaasi töö** lihtsustamiseks haldustoimingud, nt skeemi muudatusi, identimisteabe haldamine, viide andmete värskenduste, jõudluse andmete kogumine või rentniku (klient) telemeetria saidikogumi täites rühmade andmebaaside haldamine. Elastne andmebaasi töö on praegu saadaval PowerShelli cmdlet-käskude ja Azure portaali kaudu. Azure'i portaali pinna vähendada, et täitmise üle kõik andmebaasid on [elastne andmebaasi rakenduskausta (eelvaade)](sql-database-elastic-pool.md)piiratud funktsionaalsus. Juurdepääs lisafunktsioone ja täitmise skriptide rühma andmebaaside, sh kohandatud määratletud saidikogumi või failikogumi Kildu (loodud [elastne andmebaasi kliendi teek](sql-database-elastic-scale-introduction.md)), vaadake [loomine ja haldamine PowerShelli abil](sql-database-elastic-jobs-powershell.md). Projektide kohta leiate lisateavet teemast [elastne andmebaasi töö ülevaade](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Eeltingimused

* Azure'i tellimuse. Tasuta prooviversiooni leiate [ühe kuu tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
* Kohapeal on elastne andmebaasi. Vt [kohta elastne andmebaasi kaustu](sql-database-elastic-pool.md)
* Install elastne andmebaasi töö teenuse komponendid. Vaadake [installimist elastne andmebaasi töö teenus](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Töökohtade loomine

1. [Azure'i portaali](https://portal.azure.com)kasutamisel olemasoleva elastne andmebaasi töö pool, nuppu **Loo töö**.
2. Tippige kasutajanimi ja parool (loodud töökohtade installimisel) andmebaasi administraatori töö juhtelemendi andmebaasi (metaandmete salvestusruum tööde haldamine).

    ![Töö nime, tippige või kleepige kood ja klõpsake nuppu Käivita][1]
2. **Töö loomine** tera, tippige töö nimi.
3. Tippige kasutajanimi ja parool ühenduse target andmebaaside skripti täitmise õnnestub piisavalt õigusi.
4. Kleepige või tippige T-SQL-skript.
5. Klõpsake nuppu **Salvesta** ja seejärel klõpsake nuppu **Käivita**.

    ![Töö loomine ja käivitamine][5]

## <a name="run-idempotent-jobs"></a>Käivitage idempotent tööde haldamine

Skripti käivitamisel vastu andmebaase, peate olema kindel, et skript on idempotent. Seega skripti peate saama käivitada mitu korda, isegi siis, kui see on nurjunud enne lõpetamata olekus. Näiteks kui skripti nurjub, töö automaatselt proovitakse seni, kuni see ei õnnestu (piirides nimega uuesti loogika lõpuks lõpetaks selle uuesti proovimine). Viis selleks on kasutada funktsiooni klausel "Kui on olemas" ja mis tahes leitud eksemplari kustutada enne uue objekti loomine. Siin on esitatud näide:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Teise võimalusena kasutage klausel "IF pole olemas" enne luua uue eksemplari.

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

See skript värskendab siis varem loodud tabel.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Töö oleku kontrollimine

Pärast töö algust, saate selle edenemist.

1. Klõpsake elastne andmebaasi rakenduskausta lehel **Halda tööde haldamine**.

    ![Klõpsake "Hallata"][2]

2. Klõpsake töö nime (a). **Oleku** saab "Valmis" või "Nurjus." Projekti üksikasjad kuvatakse (b) ja selle kuupäev ja kellaaeg loomine ja töötab. (C) selle all kuvatakse loendis iga andmebaasi skripti edenemist sisse oma kuupäeva ja kellaaja üksikasju.

    ![Valmis töö kontrollimine][3]


## <a name="checking-failed-jobs"></a>Kontrollimine nurjus tööde haldamine

Kui töö nurjub, et täitmise Logi võib leida. Klõpsake üksikasjade kuvamiseks nurjunud töö nime.

![Nurjunud töö kontrollimine][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
