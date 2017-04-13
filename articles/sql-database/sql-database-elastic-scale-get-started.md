<properties 
    pageTitle="Elastne Andmebaasiriistad kasutamise alustamine" 
    description="Ülevaade elastne andmebaasi tööriistad funktsiooni Azure'i SQL-andmebaasi, sh lihtne valimi rakenduse käivitamiseks." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Elastne Andmebaasiriistad kasutamise alustamine

Selle dokumendi tutvustab teile Arendaja kogemus valimi rakendus töötab. Valimi loob lihtne sharded rakendus ja uurib võtme võimaluste elastne Andmebaasiriistad. Valimi näitab funktsioonid [elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md)

Avage teek installimiseks [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Pange tähele, et teek on installitud rakendusega valimi allpool kirjeldatud.

## <a name="prerequisites"></a>Eeltingimused

1. Visual Studio 2012 või uuem versioon C# on nõutav. Laadige alla tasuta versiooni veebisaidil [Visual Studio allalaadimine](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nugeti 2.7 või uuem versioon. Uusima versiooni saamiseks vaadake [Nugeti installimisel](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Laadige alla ja käivitage rakendus näidis

Funktsiooni **elastne SQL Azure'i andmebaasi – alustamine** valimi rakenduse illustreerib Azure SQL-i elastne Andmebaasiriistad sharded rakenduste arendamise kogemus kõige olulisemaid aspekte. See keskendub tootenumbri kasutamine karpide [Kildu vastendada haldus](sql-database-elastic-scale-shard-map-management.md), [andmed sõltuvad marsruutimist](sql-database-elastic-scale-data-dependent-routing.md) ja [mitme Kildu päringud](sql-database-elastic-scale-multishard-querying.md). Laadige alla ja käivitage valimi, toimige järgmiselt. 

1. Avage Visual Studio ja valige **-Fail > uus -> projekti**.
2. Klõpsake dialoogiboksis **Online**.

    ![Uue projekti > Online][2]
3. Klõpsake **Visual C#** all **näidised**.

    ![Klõpsake Visual C#][3]
4. Tippige otsinguväljale **elastne db** valimi otsimiseks. Kuvatakse tiitel **Elastne DB tööriistad Azure SQL - alustamine** .

    ![Otsinguväli][1]
 
5. Valige proovi, valige nimi ja asukoht uue projekti jaoks ja vajutage **OK** , et luua projekt.
6. Lahendus valimi projekti **app.config** faili avamine ja järgige juhiseid faili lisamiseks oma Azure SQL-i andmebaasiserveri nimi ja sisselogimisteabe (kasutajanime ja parooli).
7. Koostamine ja käivitage rakendus. Kui teilt küsitakse, lubage Visual Studio taastamine lahendus NuGet-paketid. See on alla laadida uusima versiooni elastne andmebaasi kliendi teek Nugeti.
8. Esitada erinevaid võimalusi Lisateavet kliendi teegi võimaluste kohta. Pange tähele, et rakenduse võtab konsooli juhiseid väljund ja julgelt uurida kood taustal.

    ![edenemine][4]

Palju õnne – teil on edukalt ehitatud ja käivitage esimene sharded rakenduse kasutamise elastne Andmebaasiriistad Azure'i SQL-andmebaasi. Pilgu shards valimi loodud Visual Studio või SQL Server Management Studio Azure'i DB serveriga ühendust võtta. Te teate uue valimi Kildu andmebaasid ja Kildu kaardi halduri andmebaasi valimi on loodud.

> [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Võtme tekstilõigule proovi kood

1. **Haldamise Shards ja Kildu kaardid**: koodi illustreerib shards vahemikud, töötamise ja vastenduste sisse faili **ShardMapManagerSample.cs**. Leiate selle teema kohta lisateavet: [Kildu kaardi haldus](http://go.microsoft.com/?linkid=9862595).  
2. **Andmed sõltuvad marsruutimine**: tehingute õige Kildu marsruutimine on näidatud **DataDependentRoutingSample.cs**. Lisateavet leiate teemast [Andmed sõltuvad marsruutimist](http://go.microsoft.com/?linkid=9862596). 
3. **Querying üle mitme Shards**: Querying üle shards on kujutatud faili **MultiShardQuerySample.cs**. Lisateavet leiate teemast [Mitme Kildu päringute](http://go.microsoft.com/?linkid=9862597).
4. **Lisamine tühja shards**: uue tühja shards iteratiivne lisamise sooritab failis **AddNewShardsSample.cs**kood. Selles teemas üksikasjad asuvad siin: [Kildu kaardi haldus](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Muude toimingute elastne skaala

1. **Mõne olemasoleva Kildu tükeldamise**: tükeldamiseks shards võimalus on esitatud **tükeldatud ühendamine tööriista**abil. Leiate lisateavet selle tööriista abil: [tükeldatud ühendamine tööriista ülevaade](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Olemasoleva shards ühendamise**: Kildu ühendab ka läbi **tükeldatud ühendamine tööriista**abil. Lisateabe saamiseks vaadake: [tükeldatud ühendamine tööriista ülevaade](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Maksumus

Elastne Andmebaasiriistad on tasuta. Elastne Andmebaasiriistad ei nõua täiendavat tasu peal oma Azure kasutuse kulu. 

Näiteks rakendus näidis loob uue andmebaasi. Maksumus sõltub DB Azure SQL-i andmebaasi väljaande valite ja rakenduse Azure kasutamist.

Täpsemat teavet hinna [SQL-i andmebaasi hinnad üksikasjad](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Järgmised sammud
Elastne Andmebaasiriistad kohta leiate lisateavet teemast:

* [Elastne andmebaasi tööriistad dokumentatsiooni kaart](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Proovi kood: 
    -    [Elastne DB Azure SQL - töötamise alustamine](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Elastne DB Azure SQL - integreerimise üksuse raames](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Kildu elastsuse skripti Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Ajaveeb: [elastne skaala teadaanne](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Kanali 9: [Video elastne skaala ülevaade](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Foorum: [Azure'i SQL-andmebaasi Foorum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Jõudluse mõõtmiseks: [jõudluse hinnale Kildu kaardi Manager](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
