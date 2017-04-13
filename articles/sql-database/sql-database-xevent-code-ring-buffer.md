<properties 
    pageTitle="SQL-andmebaasi XEvent Ring puhvri koodi | Microsoft Azure'i" 
    description="Pakub Transact-SQL-i kood näide, mis on tehtud kiire ja lihtne kasutus Ring puhvri target, Azure SQL-andmebaasis." 
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


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Helisemine puhvri target kood SQL-andmebaasis laiendatud sündmuste jaoks.

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Soovite täielikku kood valimi lihtsaim kiireks jäädvustada ja aruande teabe laiendatud sündmuse katse ajal. Laiendatud sündmus andmete jaoks kõige lihtsam eesmärk on [Ring puhvri target](http://msdn.microsoft.com/library/ff878182.aspx).


Selles teemas esitab Transact-SQL-i kood valimi, mis:


1. Loob tabeli andmetega selleks, et näidata.

2. Loob seansi olemasoleva laiendatud sündmus, milleks **sqlserver.sql_statement_starting**.
    - Sündmuse on piiratud sisaldavad kindla värskenduse stringi SQL-lauseid: **lause nagu "UPDATE tabEmployee %"**.
    - Valib sihtrakenduse tüüpi Ring puhvri, milleks **package0.ring_buffer**sündmuse väljundi saatmiseks.

3. Käivitab sündmus seanss.

4. Probleemid lihtsa SQL-i värskendamine laused paar.

5. Probleemid SQL valige Helista puhvri sündmuse väljundi toomiseks.
    - kategoorias liitutud **sys.dm_xe_database_session_targets** ja muud dünaamilise haldusvaated ().

6. Tabelduskohtade sündmus seanss.

7. Langeb Ring puhvri target, vabastada selle ressursse.

8. Langeb sündmus seanss ja demo tabel.


## <a name="prerequisites"></a>Eeltingimused


- Azure'i konto ja tellimus. Saate registreeruda [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).


- Mis tahes andmebaasi, saate luua tabeli.
 - Soovi korral saate [ **AdventureWorksLT** tutvustamise andmebaasi loomine](sql-database-get-started.md) minutites.


- SQL Server Management Studio (ssms.exe) ideaalvariandis mitte selle kuu värskendus versioonile. Saate alla laadida uusima ssms.exe kaudu:
 - Teemast [SQL Server Management Studio alla laadida](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Otselink allalaadimine.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Proovi kood


Väga väike muutmist, mille käivitamist Ring puhvri järgmine kood näidis Azure'i SQL-andmebaasi või Microsoft SQL Server. Erinevus on sõlm '_database' nimi mõned dünaamiline haldusvaated (), kasutatakse FROM-klausli samm 5 olemasolu. Näiteks:

- sys.dm_xe**_database**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Helista puhvri sisu


Kasutasime ssms.exe käivitamiseks proovi kood.


Tulemuste vaatamiseks me klõpsatud lahtri jaotises selle veeru päist **target_data_XML**.

Seejärel tulemuste paanil me klõpsatud lahtri jaotises selle veeru päist **target_data_XML**. Klõpsake selle loodud menüüle fail ssms.exe mis kuvati tulemi lahtri sisu, XML-ina.


Väljundisse kuvatakse järgmised blokeerida. Tundub pikk, kuid see on vaid kahe **<event>** elemente.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Vabastage oma Ring puhvri ressursside


Kui olete lõpetanud oma Ring puhvri, saate selle eemaldada ja vabastage oma ressursse emissiooni ka **muuta** umbes selline:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Sündmuse seansi määratlus on värskendatud, kuid mitte. Hiljem saate lisada oma sündmuse seansi muus eksemplaris Ring puhvri:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Lisateave


Esmane laiendatud sündmuste Azure SQL andmebaasis on:


- [Laiendatud sündmus kaalutlused SQL-andmebaasis](sql-database-xevent-db-diff-from-svr.md), mida vastandub teatud aspektide laiendatud sündmused, mis erinevad võrreldes Microsoft SQL Server Azure'i SQL-andmebaasi.


Muud koodi valimi Teemad laiendatud sündmuste jaoks on saadaval järgmised lingid. Siiski peate kontrollima rutiinselt näha, kas valimi eesmärgid Microsoft SQL Server Azure'i SQL-andmebaasi või mis tahes valimi. Seejärel saate otsustada, kas valimi käivitamiseks on vaja väikesed muudatused.


- Azure'i SQL-andmebaasi proovi kood: [sündmuse faili target kood SQL-andmebaasis laiendatud sündmuste jaoks.](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
