<properties
   pageTitle="SQL Azure'i andmebaasi lahendus kiire käivitamise | Microsoft Azure'i"
   description="Lisateavet SQL Azure'i andmebaasi lahendused"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>SQL Azure'i andmebaasi lahendus kiire käivitamise uurimine

Käesolevast artiklist leiate ülevaate on Azure SQL-i andmebaasi lahenduse kiirülevaate käivitab. Need kiiresti käivitab GitHub SQL serveri näidised hoidlas, mis asuvad ja SQL-andmebaasi kasutamist põhjal tegelike stsenaariumid täielik lahendus. Lihtne õppetükid, mis illustreerivad kindla andmebaasi SQL-i funktsiooni kasutamist, leiate teemast [tutvumine Azure'i SQL-andmebaasi õpetused](sql-database-explore-tutorials.md).

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Proovige WingTipTickets demo ja praktiliste lab

[Azure SQL-i andmebaasi WingTipTickets](https://github.com/microsoft/wingtiptickets) demo ja praktiliste lab näidata leiate Azure'i SQL-andmebaasi ning Azure'i otsingupõhiste valimi rakenduse kasutatud müüa kontserdipiletid.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Koguda ja jälgida ressursside kasutusandmete üle mitme kaustu

[Lahenduse Kiirjuhend: elastne rakenduskausta telemeetria PowerShelli kaudu](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) pakub lahenduse koguda ja jälgida SQL-andmebaasi ressursikasutus üle mitme kaustu tellimuse sisse. Kui teil on suur hulk andmebaaside tellimus, on tülikas kontrollida iga elastne kaust eraldi.

Selle probleemi lahendamiseks saate koondada SQL-i andmebaasi PowerShelli cmdlet-käskude ja ressursi kasutamine andmete kogumine mitme kaustu ja nende andmebaaside päringute T-SQL-i. See aitab teil jälgida ja analüüsida ressursikasutus tõhusamaks.

Selles lühijuhendis pakub PowerShelli skriptide ja T-SQL-päringud koos dokumentatsiooni lahendus mida ja kuidas seda rakendada.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Alustamine elastne andmebaasi SaaS stsenaariumi puhul

 [Lahenduse Kiirjuhend: elastne rakenduskausta kohandatud armatuurlaud SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) pakub lahendus tarkvara-kui-a-lahendus (SaaS) stsenaarium, mis mõjutab funktsiooni elastne andmebaasi SQL-i andmebaasi rakenduse SaaS tulusate ja scalable andmebaasi taustväärtus ette.

See lahendus, mida juhendab web appi rakendamine. See web app saab visualiseerida laadi, mis on loodud elastne andmebaasis laadi generaator, mis kasutab kohandatud armatuurlaua, mida täiendab Azure portaali.

Selles lühijuhendis pakub laadi generaator ja jälgimise web app dokumentatsiooni kohta, mis rakenduse kas ja kuidas seda kasutada koos.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Koodi esimene arendamise ja üksuse raames Azure SQL-andmebaasi loomine

Video- ja proovi [Kood esimese uue andmebaasi](https://msdn.microsoft.com/data/jj193542.aspx) pakub koodi esimene arengu, et eesmärgid uue andmebaasi tutvustus. Selle stsenaariumi suunatud andmebaas, mida pole olemas, kuid mis on loodud koodi esimene. Teise võimalusena seda stsenaariumi loob mõnda tühja andmebaasi, millele koodi esimene lisab uue tabeli.

Koodi esmalt võimaldab määratleda mudelisse C# või Visual Basic .net-i abil. Valikuline konfigureerimise saate teha atribuudid tunnid ja atribuudid või Fluenti API.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Elastne Andmebaasiriistad integreerida rakenduse üksuse raames

[Elastne andmebaasi kliendi teek koos üksuse raames](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) näidis kuvatakse muudatused, mida peate tegema üksuse raames rakenduse integreerida [elastne Andmebaasiriistad](sql-database-elastic-scale-get-started.md). Keskendutakse koostamise [Kildu kaardil haldus](sql-database-elastic-scale-shard-map-management.md) ja [andmed sõltuvad marsruutimine](sql-database-elastic-scale-data-dependent-routing.md) üksus Framework koodi esimene lähenemine.

[Koodi esimene uue andmebaasi jaoks EF valimi](http://msdn.microsoft.com/data/jj193542.aspx) toimib kogu selle valimi töötava näites. Proovi kood, mis kaasneb selles dokumendis on osa elastne andmebaasi tööriistad Visual Studio koodinäiteid proovide määramine.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Rea-turvalisuse elastne Andmebaasiriistad integreerimine

[Rentnikuga rakenduste elastne Andmebaasiriistad ja rea taseme](sql-database-elastic-tools-multi-tenant-row-level-security.md) kuvatakse muudatused, et peate tegema üksuse raames rakenduse [elastne Andmebaasiriistad](sql-database-elastic-scale-get-started.md) integreerida [rea taseme turvalisus](https://msdn.microsoft.com/library/dn765131). See näide illustreerib nende tehnoloogiate koostamiseks rakenduse väga paindlik andmete kiht, mis toetab rentnikuga shards koos koos kasutamine.

Tehke seda, kasutades ADO.net-i SqlClient või üksuse raames. See näide laiendab [elastne andmebaasi kliendi teek koos üksuse raames](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) rentnikuga Kildu andmebaaside tuge.
See loob lihtsa konsooli rakendus Ajaveebid ja postitusi, neli rentnikud ja kaks rentnikuga Kildu andmebaasi loomiseks.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Online küsitlused Tailspin küsitluste rakenduse loomine

See [Tailspin küsitluste proovi taotluse](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) on rentnikuga veebirakenduse, nimetatakse küsitlused, mis võimaldab kasutajatel luua online küsitlused. Valimi aadressid võtme muret selle kohta, kuidas hallata kasutajate identiteete rentnikuga rakenduse, sh registreerumise, autentimine, autoriseerimine ja rakenduse rollid.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Siit saate teada, SQL-andmebaasi Contoso kliinikus Demo rakenduse uusim turbefunktsioonid

See [Contoso kliinikus Demo rakenduse](https://github.com/Microsoft/azure-sql-security-sample) tutvustatakse SQL-andmebaasi turvalisuse uusimaid funktsioone.

## <a name="next-steps"></a>Järgmised sammud

[Azure'i SQL-andmebaasi õpetused uurimine](sql-database-explore-tutorials.md)
