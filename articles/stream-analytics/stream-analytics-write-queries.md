<properties 
    pageTitle="Kuidas kirjutada päringute Kasutusanalüüsi voo | Microsoft Azure'i" 
    description="Kirjutage päringute voo analüütika ja päringu andmete | Õppekeskuse tee lõigu."
    keywords="Kuidas kirjutada päringute andmepäringuid, kirjutage päringu päringute kirjutamine"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="how-to-write-queries-in-stream-analytics"></a>Kuidas kirjutada päringute Kasutusanalüüsi voo

"Alalise päringuna", mis on määratletud enne töö algab ja täide, kui see jõuab töö andmete kirjutamiseks päringuid voo loogika Azure'i voo Kasutusanalüüsi töötlemise rakendatakse. Andmete teisendus väljendub SQL-like päringukeele, mis on suuresti alamhulk T-SQL-i mõned lisatud keele laiendiga nagu [akendesüsteemide](https://msdn.microsoft.com/library/azure/dn835019.aspx) harjunud kiire ajaline semantika.

## <a name="writing-queries"></a>Päringute tehke järgmist. ##

1. Klõpsake oma voo Analytics töö Azure'i haldusportaal **päringu**.

    ![Päringu valimine](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Azure'i portaalis, klõpsake nuppu **päring**.

    ![Valige päringu eelvaade](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Uute töökohtade on Päringumall, mis aitavad teil alustada. Päringumall teostab "i läbiv" päring, mis projektide kõigi väljade Sisestuskeel sündmustest väljundisse sisse.  

    - Kui olete määratlenud vähemalt üks sisestus- ja väljundi oma töö, saate asendada kohatäite "[YourOutputAlias]" ja "[YourInputAlias]" väljad pseudonüümid sisendi ja väljundi, mida soovite kasutada esmalt. Lisaks saate endiselt autor ja Azure klassikaline portaalis päringu testimine ilma sisendi ja väljundi töö.
    - Kui soovite teha rohkem kui lihtne läbiv töötlemine, saate päringu definitsiooni redigeerida. Alustada päringu koostamine Heitke pilk mõned levinud päring on jäädvustatud mustrite [siin](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Päringu andmete aken](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Päringu andmete valideerimiseks ei tööta: ##

Saate kontrollida, et päringu toimib ootuspäraselt töötab see brauseris üle ühe või mitme kohaliku JSON failide testi andmeid sisaldav. See pole alustada tööd või arvelduse mõjutavad.

> [AZURE.NOTE] Praegu brauseris päringu testimine ei toetata Azure'i portaalis.  

1.  Veenduge, et pole vigu päringu (vastasel Test nupp keelatakse) ja seejärel klõpsake nuppu testi.  

    ![Päringu andmete Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  Teil palutakse iga päringus viidatud sisendeid failide määramiseks. Selles näites on jäänud malli päringu-on, et dialoogiboksi on nimega "yourinputalias" sisendi küsimine.  

    ![Andmete testpäring](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Liikuge sirvides faili test. Mitme failid on saadaval [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) ja ka võib Näidisandmete allalaadimiseks oma andmete voo sisendina Näidisandmete funktsioon sisendina menüü kaudu.  

    ![Päringu sisendi](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Pärast sulgemist dialoogiboksi, päring käivitatakse üle testi andmed ja te näete tulemusi päringulehe allservas.  

    ![Päringu Kokkuvõte](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
