<properties
    pageTitle="Venitamine lubatud andmebaasi varundada | Microsoft Azure'i"
    description="Saate teada, kuidas varundada venitamine\-lubatud andmebaasid."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Varukoopia venitamine lubatud andmebaasid

Andmebaasi varundamine aitavad teil mitut tüüpi tõrkeid, tõrgete ja katastroofide taastada.  

-   Teil on oma venitamine varundamine\-lubatud SQL serveri andmebaasi.  

-   Microsoft Azure'i automaatselt varundage remote venitamine andmebaas on migreeritud Azure SQL serveri andmeid.  

>    [AZURE.NOTE] Varundus on ainult üks osa täieliku kättesaadavuse ja järjepidevuse ärilahenduse. Kõrge-saadavus kohta lisateabe saamiseks vt [Kõrge-saadavus lahendusi](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>SQL serveri andmete varundamine  

Varundage oma venitamine\-lubatud SQL serveri andmebaasi, saate jätkata kasutada SQL serveri varukoopia viise, mida te praegu kasutada. Lisateavet leiate teemast [varundamine ja taastamine, SQL serveri andmebaasi](https://msdn.microsoft.com/library/ms187048.aspx).

Varukoopiate venitamine lubatud SQL serveri andmebaasi sisaldavad ainult kohalikud andmed ja andmed, mille puhul punktis migreerimise ajal, kui käivitatakse varukoopia. \(Andmed, mis pole veel migreeritud, kuid migreeritakse Azure migreerimise sätted tabelite põhjal on õigus.\) See on teada **madal** varukoopiana ja ei sisalda andmeid juba viiakse üle Azure.  

## <a name="back-up-your-remote-azure-data"></a>Remote Azure andmete varundamine   

Microsoft Azure'i automaatselt varundage remote venitamine andmebaas on migreeritud Azure SQL serveri andmeid.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure'i vähendab andmekao Automaatne varundamine  
Azure SQL serveri venitamine andmebaasi teenuse kaitseb teie remote andmebaasid automaatse salvestusruumi hetktõmmiseid vähemalt iga. Säilitab iga hetktõmmise teile mitmesuguseid võimalike taastamine punktide 7 päevaks.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure'i vähendab andmekao koos geo\-koondamine  
Azure'i andmebaasi varundamine on talletatud geo\-liigsed Azure Storage (RA\-GRS) ja on seega geo\-üleliigsed vaikimisi. Geo\-liigsed salvestusruumi tiražeerib andmete teisese alale, mis on sadu miili eemale esmane piirkond. Nii põhi- ja piirkondades on andmete kopeeritud kolm korda iga eraldi viga domeenid ja täiendamine domeenide üle. See tagab, et teie andmed on püsival, isegi kui tegemist on lõpule viidud piirkondliku katkestuste või tõttu, mis muudab üks Azure piirkondades saadaval.

### <a name="stretchRPO"></a>Venitus andmebaasi vähendab andmekao Azure andmete jaoks migreeritud ridade ajutiselt säilitades
Pärast venitamine andmebaasi migreerib Azure SQL Serverist õigus read, säilitab need read vahekausta tabeli vähemalt 8 tundi. Kui Azure'i andmebaasi varukoopia taastamiseks kasutab venitamine andmebaasi salvestatud lavastus tabeli read SQL serveri ja Azure'i andmebaasid vastavusse viia.

Pärast andmete Azure varukoopia taastamiseks tuleb käivitada salvestatud protseduur [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) taaskäivitamiseks venitada\-lubatud kaugandmebaasiga Azure SQL serveri andmebaasi. Kui käivitate **sys.sp_rda_reauthorize_db**, venitamine andmebaasi automaatselt lepitab SQL serveri ja Azure'i andmebaasid.

Tundide migreeritud andmeid, mis venitamine andmebaasi säilitab ajutiselt vahekausta tabeli suurendamiseks käivitada salvestatud protseduur [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) ja määrake tunnid, mis on suuremad kui 8. Otsustada, kui palju andmeid säilitamise, arvestage järgmisi tegureid.
-   Sagedus Azure automaatvarunduse (vähemalt iga 8 tunni järel).
-   Vajalik aeg pärast probleem probleemi tuvastada ja taastada varukoopia.
-   Azure'i taastetoimingu kestus.

> [AZURE.NOTE] Suurendades venitamine andmebaasi säilitab ajutiselt vahekausta tabelis andmeid suurendab vaheline vaja SQL serveriga.

Tundide venitamine andmebaasi praegu säilitab ajutiselt vahekausta tabeli andmete käivitage salvestatud protseduur [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx).

## <a name="see-also"></a>Vt ka

[Haldamine ja venitamine andmebaasi tõrkeotsing](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Ja SQL serveri andmebaasi taastamine](https://msdn.microsoft.com/library/ms187048.aspx)
