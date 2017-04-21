
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Ühenduse stringi turvalisus config näidisfail


See ei pea paika panna ühendusstringi nimega literaalide C# koodi. See on parem panna ühendusstringi config faili. Seal saate redigeerida stringi ilma on vaja Kompileeri.

Oletame, et teie kompileeritud C# programmi nimega **ConsoleApplication1.exe**ja asub see .exe on **bin\debug\* * kataloogi.

Selles näites on talletatud suurema osa oma ühendusstring config fail nimega täpselt **ConsoleApplication1.exe.config**. See config fail peab asuma ka **bin\debug\**.

Järgmise config faili XML-näete nimega **ConnectionString4NoUserIDNoPassword**ühendusstringi. C# koodi otsitakse stringile.

Redigeerige tegelikku nime jaoks kohatäiteid:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Järgmine joonis valisime argument kaks parameetrid:

- Kasutaja ID-d = {your_userName_here};
- Parooli = {your_password_here};


Saate need lisada, kuid mõnikord on parem on teie programmi toomine neid väärtusi kasutaja klaviatuur. See sõltub.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
