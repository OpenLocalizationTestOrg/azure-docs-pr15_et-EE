<properties 
    pageTitle="SQL-andmebaasi XEvent sündmuse faili koodi | Microsoft Azure'i" 
    description="Pakub PowerShelli ja Transact-SQL-i kahefaasiline koodi näide, mis näitab sündmuse faili target laiendatud juhul Azure SQL andmebaasis. Azure'i salvestusruumi on nõutav osa stsenaarium." 
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


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Sündmuse faili target kood SQL-andmebaasis laiendatud sündmuste jaoks.

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Soovite täielikku kood valimi robustne võimalus jäädvustada ja aruande laiendatud sündmuse teabe.


Microsoft SQL Server, kasutatakse [sündmuse faili target](http://msdn.microsoft.com/library/ff878115.aspx) sündmuse väljundeid kohalikule kõvakettale faili salvestada. Kuid selliseid faile pole saadaval Azure SQL-andmebaasiga. Selle asemel kasutada Azure salvestusteenus toetamiseks sündmuse faili sihtkoht.


Selles teemas esitab valimi kahefaasiline kood:


- PowerShelli loomiseks on Azure Storage container pilveteenuses.

- Transact-SQL:
 - Sündmuse faili eesmärgiks määrata Azure Storage ümbrises.
 - Luua ja sündmuse seansi käivitamine ja jms.


## <a name="prerequisites"></a>Eeltingimused


- Azure'i konto ja tellimus. Saate registreeruda [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).


- Mis tahes andmebaasi, saate luua tabeli.
 - Soovi korral saate [ **AdventureWorksLT** tutvustamise andmebaasi loomine](sql-database-get-started.md) minutites.


- SQL Server Management Studio (ssms.exe) ideaalvariandis mitte selle kuu värskendus versioonile. Saate alla laadida uusima ssms.exe kaudu:
 - Teemast [SQL Server Management Studio alla laadida](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Otselink allalaadimine.](http://go.microsoft.com/fwlink/?linkid=616025)


- Peab teil olema installitud [Azure PowerShelli moodulid](http://go.microsoft.com/?linkid=9811175) .
 - Moodulid pakuvad käsud nagu - **New-AzureStorageAccount**.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Etapp 1: PowerShelli koodi Azure Storage ümbrises.


See PowerShelli on proovi kood kahefaasiline etapp 1.

Skripti algab pärast eelmise võimalik käivitamine ja on rerunnable puhastamiseks käsud.



1. PowerShelli skripti kleepimine lihtsat tekstiredaktorit, nt Notepad.exe ja skripti abil laiend **.ps1**failina salvestada.

2. Käivitage PowerShelli ISE administraatorina.

3. Vastava viiba kuvamisel tippige<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>ja seejärel vajutage sisestusklahvi Enter.

4. PowerShell ISE, avage oma **.ps1** fail. Käivitage skript.

5. Skript käivitumisel uus aken, kus saate sisse logida Azure.
 - Kui te uuesti skripti ilma häireid seansi, on teil on mugav võimalus käsu **Lisa-AzureAccount** kommenteerimise.


![PowerShell ISE Azure moodul installitud, skripti käivitamiseks valmis.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Pange tähele mõne nimega väärtused, mis prinditakse PowerShelli skripti, kui see lõpeb. Saate redigeerida neid väärtusi Transact-SQL-i skripti, mis tuleneb etapp 2.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Etapp 2: Transact-SQL-i kood, mis kasutab Azure Storage container


- Etapp 1 selle proovi kood käivitasite PowerShelli skripti, mis on Azure Storage container loomiseks.
- Etapp 2, tuleb järgmine Transact-SQL-i skripti edasi kasutada ümbris.


Skripti algab pärast eelmise võimalik käivitamine ja on rerunnable puhastamiseks käsud.


Kui see on lõppenud, prinditakse PowerShelli skripti nimega väärtuste. Redigeerige Transact-SQL-i skripti kasutada neid väärtusi. Otsige **TODO** Transact-SQL-i skripti leidmiseks Redigeeri punkte.


1. Avage SQL Server Management Studio (ssms.exe).

2. Azure'i SQL-andmebaasi andmebaasiga ühenduse.

3. Klõpsake nuppu Uus päring paani avamiseks.

4. Päringu paanil järgmine Transact-SQL-i skripti kleepimine.

5. Otsige üles iga **TODO** skripti ja tehke sobiv muudatused.

6. Salvestage ja seejärel käivitage skript.


&nbsp;


> [AZURE.WARNING] Määrab eelnev PowerShelli skripti SAS võtmeväärtuse võib algavad on "?" (küsimärk). Järgmise T-SQL skripti SAS võti kasutamisel peate *juhtiva '?' eemaldada*. Muul juhul võib teie püüete blokeerinud turvalisus.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Kui sihtkohas ei manustamine käivitamisel, peate peatamine ja taaskäivitage seanss sündmuse:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Väljund


Kui skripti Transact-SQL-is on lõpule jõudnud, klõpsake mõnda lahtrit **event_data_XML** veeru päise all. Ühe **<event>** kuvatakse element, mis näitab UPDATE-lause.

Siin on üks **<event>** element, mis loodi testimise käigus:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Eelnev Transact-SQL-i skripti kasutada lugeda selle event_file süsteemi järgmist funktsiooni:

- [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Laiendatud sündmused andmete kuvamist täpsemate suvandite selgitus on saadaval veebisaidil:

- [Täpsemad Target andmete laiendatud sündmuste vaatamine](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Proovi kood SQL serveris teisendamine


Oletame, et soovite käivitada eelnev Transact-SQL-i valimi Microsoft SQL serveris.


- Lihtsa, tuleks soovite täielikult asendada kasutamine Azure Storage-ümbrisest lihtsa failiga, nt **C:\myeventdata.xel**. SQL serveri majutava arvuti kohalikule kõvakettale soovite faili kirjutada.


- Ei oleks vaja iga tüüpi Transact-SQL-lausete **loomine JUHTSLAIDI KEY** ja **MANDAADI loomine**.


- **Sündmuse loomine seansi** -lauses **Lisamine siht** -klauslis soovite asendada määratud Http väärtus tehtud **filename =** nagu **C:\myfile.xel**stringiga täielik tee.
 - Azure Storage konto pole vaja kaasata.


## <a name="more-information"></a>Lisateave


Kontode ja ümbriste teenuses Azure Storage kohta leiate teemast:

- [Kuidas kasutada bloobimälu .net-i kaudu](../storage/storage-dotnet-how-to-use-blobs.md)
- [Nime andmise ja ümbriste, plekid ja metaandmete viitamine](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Juur-ümbrisest töötamine](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [1 tund: Luua salvestatud Accessi poliitika ja ühiskasutusega juurdepääsu signatuuri on Azure container](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Tund 2: Looge SQL serveri identimisteavet, kasutades allkirja ühiskasutusega juurdepääsuks](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

