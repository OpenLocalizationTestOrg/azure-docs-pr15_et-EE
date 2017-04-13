<properties
    pageTitle="Ühenduse loomine SQL-andmebaasiga, kasutades Python | Microsoft Azure'i"
    description="Esitab Python kood valimi abil saate Azure SQL-i andmebaasiga ühenduse loomiseks."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Ühenduse loomine SQL-andmebaasi Python abil


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Selles teemas kirjeldatakse, kuidas päringu abil Python Azure SQL-andmebaasi ühendada. See näidis saate käivitada Windows, Ubuntu Linux-või Mac-arvutisse.


## <a name="step-1-create-a-sql-database"></a>Samm 1: SQL-i andmebaasi loomine

Vaadake selle [lehe Alustamine](sql-database-get-started.md) saate teada, kuidas luua Näidisettevõtte andmebaas.  Oluline, et järgite juhend on **AdventureWorks veebiandmebaasi malli**loomiseks. Allpool näidised töötavad ainult **AdventureWorks skeemi**. Kui loote oma andmebaasi veenduge et saate juurdepääsu lubamine oma IP-aadressi, võimaldades tulemüüri reeglid, nagu on kirjeldatud [lehe Alustamine](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Samm 2: Konfigureerimine arenduskeskkond

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>Installige vajalikud moodulid
Avage oma terminal ja installimine

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Avage oma terminal ja liikuge kausta, kuhu kavatsete luua oma python skript. Sisestage installida **FreeTDS** ja **pymssql**järgmised käsud. pymssql kasutab FreeTDS ühenduse SQL-i andmebaasid.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Installige pymssql [**siin**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Veenduge, et valite õige whl fail. Näide: kui kasutate 64-bitises arvutis Python 2.7 valida: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Kui laadite faili .whl paigutamine C:/Python27 kausta.

Nüüd pymssql draiveri pip käsurea kaudu installida. C:/Python27 ja Käivita järgmine CD-le
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Juhiseid, mis võimaldavad kasutada pip leiate [siit](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Samm 3: Käivita proovi kood

Looge fail nimega **sql_sample.py** ja kleepige järgmine kood sees. Saate kasutada seda käsurea kaudu:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>SQL-andmebaasiga ühenduse loomine

Funktsiooni [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) kasutatakse SQL-andmebaasiga ühenduse loomiseks.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>SQL SELECT lause Execute

Funktsiooni [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) saab tuua result set päringu SQL-i andmebaasist. See funktsioon sisuliselt aktsepteerib iga päringu ja tagastab tulemi määrata, et saate näidata üle [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone)kasutamisega.


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Rea lisamine, edasi parameetrid ja tuua loodud primaarvõti

SQL-andmebaasis atribuudi [identiteedi](https://msdn.microsoft.com/library/ms186775.aspx) ja [JADA](https://msdn.microsoft.com/library/ff878058.aspx) objekti saab automaatselt luua [primaarvõtme](https://msdn.microsoft.com/library/ms179610.aspx) väärtused. 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Tehinguid


Selles näites kood näitab tehinguid, milles kasutamine saate:

* Alustage tehingu
* Rea andmete lisamine
* Tagasipööramine oma tehingu tagasivõtmiseks lisa 

Kleepige järgmine kood sql_sample.py sees.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Järgmised sammud

* Vaadake üle [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md)
* Lisateavet [Microsoft SQL serveri Python draiver](https://msdn.microsoft.com/library/mt652092.aspx)
* Külastage [Python Arenduskeskus](/develop/python/).

## <a name="additional-resources"></a>Lisaressursid 

* [Kujundus mustrid mitme rentniku SaaS rakenduste Azure SQL-andmebaas](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [SQL-andmebaasi võimaluste](https://azure.microsoft.com/services/sql-database/) uurimine
