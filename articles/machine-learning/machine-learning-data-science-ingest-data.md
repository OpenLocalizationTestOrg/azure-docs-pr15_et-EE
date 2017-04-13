<properties 
    pageTitle="Andmete laadimine salvestusruumi keskkonnas Analyticsi | Microsoft Azure'i" 
    description="Andmete teisaldamine ja sealt Azure'i bloobimälu" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" />

# <a name="load-data-into-storage-environments-for-analytics"></a>Salvestusruumi keskkonnas analytics andmete laadimine

Andmete meeskonna teadus protsessi nõuab, et andmed on märkimisväärselt või laaditud mitmesuguseid eri salvestusruumi keskkonnas töödeldud või analüüsitud kõige sobivam viis protsessi igas etapis. Andmete sihtkohtade tavaliselt kasutatakse töötlemise hulgas Azure'i bloobimälu SQL Azure'i andmebaasid, SQL Server Azure'i VM, Hdinsightiga (Hadoopi) ja Azure seadme õ. 

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

See **menüü** linke teemadele, kus kirjeldatakse, kuidas üheks nende target keskkonnas, kus andmed on salvestatud ja töödeldud andmete neelata.

Tehniliste ja ärivajadused kui ka algsest asukohast, vormindamine ja teie andmete maht määrab target keskkonnas, kuhu selle andmeid on vaja oma analüüsi eesmärkide saavutamiseks manustada. Ei nõua andmete teisaldamiseks mitu keskkondades saavutamiseks vajalikud koostada sõnastikupõhise mudel ülesanded mitmesuguste stsenaarium aeg-ajalt. Tööülesannete järjestusel võib sisaldada näiteks andmete uurimine, eelse töötlemine, puhastamine, alla-valimite ja mudeli koolitus.
