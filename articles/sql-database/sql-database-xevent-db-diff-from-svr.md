<properties
    pageTitle="Laiendatud sündmused SQL-andmebaasis | Microsoft Azure'i"
    description="Kirjeldatakse laiendatud Azure'i SQL-andmebaasi sündmuste (XEvents), ja kuidas sündmuse seansid veidi erineda sündmuse seanssi Microsoft SQL Server."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""
    tags=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="genemi"/>


# <a name="extended-events-in-sql-database"></a>Laiendatud sündmused SQL-andmebaasis

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

See teema selgitab, kuidas rakendamine laiendatud sündmused Azure SQL-andmebaasis on veidi teistsugused võrreldes laiendatud sündmuste Microsoft SQL Server.


- SQL-i andmebaasi V12 saadud funktsiooni laiendatud sündmuste teine pool kalendri 2015.
- SQL serveri oli laiendatud sündmusi, sest 2008.
- Funktsioon määrata laiendatud ürituste SQL-andmebaas on robustne alamhulk funktsioone SQL serveris.


*XEvents* on mitteametlikuks hüüdnimi, mida kasutatakse mõnikord "laiendatud sündmused" Ajaveebid ja mitteametlikuks mujal.


> [AZURE.NOTE] Oktoober 2015, laiendatud sündmus seansi funktsioon on aktiveeritud Azure SQL-andmebaasis tasemel eelvaade. General Availability (GA) kuupäev pole veel määratud.
>
> Azure'i [Teenusevärskendused](https://azure.microsoft.com/updates/?service=sql-database) lehe on postituste GA teadaannete meiliteatiste.


Lisateave laiendatud sündmusi, Azure'i SQL-andmebaasi ja Microsoft SQL Server, on saadaval veebisaidil:

- [Lühijuhend: SQL serveri laiendatud sündmused](http://msdn.microsoft.com/library/mt733217.aspx)
- [Laiendatud sündmused](http://msdn.microsoft.com/library/bb630282.aspx)


## <a name="prerequisites"></a>Eeltingimused


Selles teemas eeldab, et teil on juba mõningaid teadmisi:


- [Azure'i SQL-andmebaasi teenus](https://azure.microsoft.com/services/sql-database/).


- [Laiendatud sündmused](http://msdn.microsoft.com/library/bb630282.aspx) Microsoft SQL Server.
 - Suurema osa oma dokumentatsiooni laiendatud sündmuste kohta kehtib nii SQL Server ja SQL-andmebaasi.


Eelnev järgmiste üksuste oleks hea [target](#AzureXEventsTargets)sündmuse faili valimisel.


- [Azure'i salvestusteenus](https://azure.microsoft.com/services/storage/)


- PowerShelli
 - [Azure'i PowerShelli kasutamine Azure Storage](../storage/storage-powershell-guide-full.md) – pakub täielik teavet PowerShelli ja Azure salvestusteenus.


## <a name="code-samples"></a>Koodinäiteid


Kahe koodinäiteid seotud teemadest:


- [Helisemine puhvri target kood SQL-andmebaasis laiendatud sündmuste jaoks.](sql-database-xevent-code-ring-buffer.md)
 - Lühike lihtsa Transact-SQL-i skripti.
 - Me rõhutada kood valimi teema, et kui olete lõpetanud helisemise puhvri target, tuleks Vabastage oma ressursse on alter-drop täites `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` lause. Hiljem saate lisada teise eksemplari Ring puhvri, `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Sündmuse faili target kood SQL-andmebaasis laiendatud sündmuste jaoks.](sql-database-xevent-code-event-file.md)
 - Etapp 1 on PowerShelli loomiseks on Azure Storage ümbrises.
 - Etapp 2 on Transact-SQL Azure'i salvestusruumi-ümbrisest kasutava.


## <a name="transact-sql-differences"></a>Transact-SQL-i erinevused


- Kui [Sündmus seansi loomine](http://msdn.microsoft.com/library/bb677289.aspx) käsu SQL serveris saate kasutada **Serveri** klausel. Kuid SQL-andmebaasi kasutate **Sees andmebaasi** klausel selle asemel.


- **Sees andmebaasi** klausel kehtib ka [Muuta sündmuse SEANSS](http://msdn.microsoft.com/library/bb630368.aspx) ja [KUKUTAGE sündmuse seansi](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-i käskude.


- Hea tava on kaasata sündmuse seansi võimalus **STARTUP_STATE = ON** oma **Sündmuse loomine SEANSIL** või **muuta sündmuse seansi** aruannetes.
 - **= ON** väärtus toetab automaatset taaskäivitamist pärast ümberkonfigureerimine loogilise andmebaasi Tõrkesiirde tõttu.


## <a name="new-catalog-views"></a>Uut kataloogi vaadete


Laiendatud sündmused funktsiooni toetab mitut [kataloogi vaadete](http://msdn.microsoft.com/library/ms174365.aspx). Kataloogi vaadete öelda *metaandmete või määratlused* kasutaja loodud sündmuse seansside praeguses andmebaasis. Vaadete ei tagasta eksemplari aktiivsed sündmuse seansid teavet.


| Nimi<br/>kataloogi vaates | Kirjeldus |
| :-- | :-- |
| **sys.database_event_session_actions** | Tagastab üht rida iga toimingu iga sündmuse mõni sündmus seansi. |
| **sys.database_event_session_events** | Tagastab üht rida iga sündmuse mõni sündmus seanss. |
| **sys.database_event_session_fields** | Tagastab üht rida iga kohandamine võimalik veerg, mis loodi konkreetselt sündmused ja eesmärkide. |
| **sys.database_event_session_targets** | Tagastab üht rida iga sündmuse Target (sihtkoht) soovitud sündmus seansi jaoks. |
| **sys.database_event_sessions** | Tagastab järjest iga sündmuse seansi SQL-andmebaasi andmebaasi. |


Microsoft SQL Server, on sarnane kataloogi vaadete nimed, mis sisaldavad *.server\_ * asemel *.database\_*. Mustri nimi on nagu **sys.server_event_%**.


## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Uue dünaamilise juhtimise vaatamist [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)


SQL Azure'i andmebaas on laiendatud sündmused toetavad [dünaamiliste haldusvaated ()](http://msdn.microsoft.com/library/bb677293.aspx) . DMVs räägi *aktiivse* sündmuse seansid.


| DMV nimi | Kirjeldus |
| :-- | :-- |
| **sys.dm_xe_database_session_event_actions** | Annab vastuseks teabe sündmus seansi toimingute kohta. |
| **sys.dm_xe_database_session_events** | Annab vastuseks teabe seansi sündmuste kohta. |
| **sys.dm_xe_database_session_object_columns** | Näitab konfiguratsiooni väärtusi objekte, mis on seotud seansi jaoks. |
| **sys.dm_xe_database_session_targets** | Annab vastuseks teabe seansi sihtkohtade kohta. |
| **sys.dm_xe_database_sessions** | Tagastab järjest iga sündmuse seansi, millele on rakendatud praegune andmebaas. |


Microsoft SQL Server, on sarnane kataloogi vaadete nimega ilma selle * \_andmebaasi* osa nimi, näiteks:


- **sys.dm_xe_sessions**, nime asemel<br/>**sys.dm_xe_database_sessions**.


### <a name="dmvs-common-to-both"></a>Mõlemal DMVs


Laiendatud sündmuste jaoks on täiendavad DMVs, mis on nii Azure'i SQL-andmebaasi ja Microsoft SQL Server.


- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**



 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Otsige on saadaval laiendatud sündmusi, toiminguid ja sihtkohad


Saate mõne lihtsa SQL **Valige** loendi saadaval sündmused, toiminguid ja target käivitada.


```
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```



<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a>

&nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Eesmärgid oma SQL-andmebaasi sündmuse seansid


Siin on eesmärgid, mis võivad teie sündmuse seansside SQL-andmebaasi tulemusi.


- Lühidalt hoiab [ring puhvri siht](http://msdn.microsoft.com/library/ff878182.aspx) - mälu sündmuse andmed.
- [Sündmuse Counter siht](http://msdn.microsoft.com/library/ff878025.aspx) - loendab kõik sündmused on laiendatud sündmuste seanss.
- [Sündmuse faili siht](http://msdn.microsoft.com/library/ff878115.aspx) - kirjutab täieliku puhvrid, mis on Azure Storage ümbrises.


[Sündmuse jälgimine for Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API ei ole saadaval SQL andmebaasis laiendatud sündmuste jaoks.


## <a name="restrictions"></a>Piirangud


On paar sobiv SQL-andmebaasi pilvepõhise keskkonna turvalisusega seotud erinevused.


- Laiendatud sündmused põhineb ühe – rentniku eraldamise mudel. Mõni sündmus seansi ühe andmebaasi ei pääse mõnda muusse andmebaasi andmete või sündmused.

- Te ei saa välja **Seansi sündmuse loomine** aruanne konteksti **põhi** andmebaasi.


## <a name="permission-model"></a>Õiguste mudel


**Sündmuse loomine seansi** aruande puhul andmebaasi peab teil olema **kasutusõigus** . Andmebaasi omanik (dbo) on **kasutusõigus** .


### <a name="storage-container-authorizations"></a>Salvestusruumi container lube


Saate luua oma Azure Storage container SAS luba peate määrama **rwl** jaoks õigusi. **Rwl** väärtus sisaldab järgmisi õigusi:


- Lugemine
- Kirjutamine
- Loend


## <a name="performance-considerations"></a>Jõudluse huvides


Stsenaariumid, kus saate laiendatud sündmuste intensiivselt koguda aktiivne rohkem mälu, kui on üldist süsteemi terve on. Seetõttu Azure'i SQL-andmebaasi süsteemi dünaamiliselt määrab ja reguleerib aktiivne mälu, mis võib mõni sündmus seansi koguda piirangud. Dünaamiline arvutus minna palju tegureid.


Kui kuvatakse tõrketeade, mis ütleb, suurim lubatud mälu jõustatud, on mõned korrektiivläätsed toiminguid, mida saate teha.


- Käivitage vähem samaaegseid sündmuse seansid.


- Vähendada kaudu oma **loomine** ja **muuta** laused sündmuse seansid, saate määrata mälu funktsiooni **MAX\_mälu** klausel.


### <a name="network-latency"></a>Võrgu latentsus


**Sündmuse faili** target võivad tekkida võrgu latentsus või tõrkeid ajal Azure Storage plekid püsib andmed. Kui nad oodata võrguühenduse lõpuleviimiseks võib viivitada muid sündmusi, SQL-andmebaasis. See viivitus aeglustada oma töökoormus.

- Vältige selle jõudluse riski vähendamiseks säte suvandit **EVENT_RETENTION_MODE** **NO_EVENT_LOSS** oma juhul seansi määratlusi.


## <a name="related-links"></a>Seotud lingid


- [Azure Storage Azure PowerShelli kaudu](../storage/storage-powershell-guide-full.md).
- [Azure'i salvestusruumi cmdlet-käsud](http://msdn.microsoft.com/library/dn806401.aspx)


- [Azure'i PowerShelli kasutamine Azure Storage](../storage/storage-powershell-guide-full.md) – pakub täielik teavet PowerShelli ja Azure salvestusteenus.
- [Kuidas kasutada bloobimälu .net-i kaudu](../storage/storage-dotnet-how-to-use-blobs.md)


- [Looge MANDAADI (Transact-SQL)](http://msdn.microsoft.com/library/ms189522.aspx)
- [Looge sündmus SEANSS (Transact-SQL)](http://msdn.microsoft.com/library/bb677289.aspx)


- [Jonathan Kehayias postitused Microsoft SQL serveri laiendatud sündmuste kohta](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


Muud koodi valimi Teemad laiendatud sündmuste jaoks on saadaval järgmised lingid. Siiski peate kontrollima rutiinselt näha, kas valimi eesmärgid Microsoft SQL Server Azure'i SQL-andmebaasi või mis tahes valimi. Seejärel saate otsustada, kas valimi käivitamiseks on vaja väikesed muudatused.


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
