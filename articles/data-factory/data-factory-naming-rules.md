<properties 
    pageTitle="Andmete Factory - nime andmise reeglid | Microsoft Azure'i" 
    description="Kirjeldatakse andmete Factory üksuste nimede reeglid." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure'i andmed Factory - nime andmise reeglid 
Järgmises tabelis antakse nime reeglid andmete Factory artefakte.



Nimi | Nime unikaalsuse | Kontroll
:--- | :-------------- | :----------------
Andmete Factory | Microsoft Azure'i üle kordumatu. Nimed on väiketähed, st MyDF ja mydf viidata sama andmete factory. |<ul><li>Iga andmete factory on seotud täpselt ühe Azure'i tellimus.</li><li>Objekti nimi peab algama tähe või arvu ja võib sisaldada ainult tähti, numbreid ja sidekriipsu (-)-märk.</li><li>Iga sidekriipsu (-)-märk tuleb kohe ees ja järgneb tähe või arvu. Container nimedes järjestikust kriipsud pole lubatud.</li><li>Nimi võib olla 3 – 63 märki.</li></ul>
Lingitud teenuste/tabelid/torujuhtmete | Kordumatud abil andmete factory. Väiketähed on nimed. | <ul><li>Tabeli nime märkide arvu ülempiir: 260.</li><li>Objekti nimi peab algama tähe arv või allkriipsu (_).</li><li>Pärast märgid ei ole lubatud: ".", "+", "?", "/", "<", ">","*", "%", "ja", ":","\\"</li></ul>
Ressursirühm | Microsoft Azure'i kogu kordumatu. Nimed on väiketähed. | <ul><li>Märkide arvu ülempiir: 1000.</li><li>Nimi võib sisaldada tähti, numbreid ja järgmisi märke: "-", "_",","ja".".</li></ul>