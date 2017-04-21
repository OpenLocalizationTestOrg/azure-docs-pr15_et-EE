
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>Ühendusstringi võrdlus


Järgmises tabelis võrreldakse ühendusstringi, mis tuleb teie asutusesisese SQL serveriga SQL Azure'i andmebaasi pilves ja C# programmi. Erinevused on paksus kirjas.


| Ühendusstringi jaoks<br/>Azure'i SQL-andmebaas | Ühendusstringi jaoks<br/>Microsoft SQL Server |
| :-- | :-- |
| Server =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>Kasutaja ID-d = {your_loginName_here}**@{your_serverName_here}**;<br/>Parooli = {your_password_here};<br/>**Andmebaasi = {your_databaseName_here};**<br/>**Ühenduse ajalõpp = 30**;<br/>**Krüptimine = True**;<br/>**TrustServerCertificate = False**; | Server = {your_serverName_here};<br/>Kasutaja ID-d = {your_loginName_here};<br/>Parooli = {your_password_here}; |


Funktsiooni **andmebaasi =** on valikuline, SQL Server, kuid SQL-andmebaasi jaoks on vaja.


[.NET ADO SqlConnectionStringBuilder atribuudid](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) - käsitletakse üksikasjalikult kõik parameetrid.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
