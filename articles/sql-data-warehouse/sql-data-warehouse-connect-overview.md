<properties
   pageTitle="Ühenduse loomine SQL Azure'i andmebaas | Microsoft Azure'i"
   description="Kuidas leida serveri nimi ja ühenduse stringi teie soovite SQL Azure'i andmebaas"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Ühenduse loomine SQL Azure'i andmebaas

See artikkel aitab teil ühenduse loomine SQL-i andmebaas esimest korda.

## <a name="find-your-server-name"></a>Serveri nime otsimine

Esimene samm ühenduse SQL-i andmebaas on teada, kuidas leida oma serveri nimi.  Järgmises näites serveri nimi on näiteks sample.database.windows.net. Täielikult kvalifitseeritud serveri nime otsimine

1. Avage [Azure'i portaalis][].
2. Klõpsake nuppu **SQL** andmebaase 
3. Klõpsake andmebaasi, mida soovite ühendada.
4. Otsige üles täielik serveri nime.

    ![Täielik serveri nimi][1]

## <a name="supported-drivers-and-connection-strings"></a>Toetatud draiverid ja ühendusstringi

SQL Azure'i andmebaas toetab [ADO.net-i][], [ODBC][], [PHP][]ja [JDBC][]. Üks eelnev uusim versioon ja dokumentatsiooni otsimiseks klõpsake nuppu. Automaatselt genereerida ühendusstringi draiveri, mida kasutate Azure portaali, saate klõpsata funktsiooni **Kuva andmebaasi ühendusstringi** kaudu eelmises näites.  Järgnevalt on ka mõned näited ühendusstringi näeb iga draiverit.

> [AZURE.NOTE] Kaaluge säte ühenduse ajalõpp 300 sekundi lubamiseks ühendust saada lühemaks jääda.

### <a name="adonet-connection-string-example"></a>ADO.net-i ühenduse stringi näide

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>ODBC ühenduse stringi näide

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>PHP ühenduse stringi näide

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>JDBC ühenduse stringi näide

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Ühenduse sätted

SQL-i andmebaas ühtlustab teatud sätted ühenduse ja objekti loomise ajal. Nende sätete ei saa alistada ja kaasata.

| Andmebaasi säte       | Väärtus                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | SEES                           |
| [QUOTED_IDENTIFIERS][] | SEES                           |
| [DATEFORMAT][]         | MDY                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Järgmised sammud

Ühenduse loomine ja Visual Studio abil päringu, leiate teemast [päringu Visual Studio abil][]. Autentimise suvandite kohta leiate lisateavet teemast [autentimise SQL Azure'i andmebaas][].

<!--Articles-->
[Päring koos Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[SQL Azure'i andmebaas autentimine]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET-I]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure'i portaal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


