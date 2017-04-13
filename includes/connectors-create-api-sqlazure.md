### <a name="prerequisites"></a>Eeltingimused
- Azure'i konto; [tasuta konto](https://azure.microsoft.com/free) loomisel
- Rakenduse [Azure'i SQL-andmebaasi](../articles/sql-database/sql-database-get-started.md) ühenduseteavet, sh serverinime, andmebaasi nime ja kasutajanime ja parooli abil. SQL-andmebaasi ühendusstringi kaasatakse see teave:
  
    Server = tcp:*yoursqlservername*. database.windows.net,1433;Initial kataloogi =*yourqldbname*; Turbeteabe püsida = False; Kasutaja ID-d = {your_username}; Parooli = {your_password}; MultipleActiveResultSets = False; Krüpti = True; TrustServerCertificate = False; Ühenduse ajalõpp = 30;

    Lugege lisateavet [Azure SQL-i andmebaasid](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Kui loote Azure'i SQL-andmebaasi, saate luua ka valimi kaasas SQL-i andmebaasid. 



Enne kasutamisel loogika rakenduse Azure'i SQL-andmebaasi ühenduse SQL-andmebaasi. Saate teha seda hõlpsalt Azure'i portaalis loogika rakendusest.  

Ühenduse loomine oma Azure'i SQL-andmebaasi tehes järgmist:  

1. Loogika rakenduse loomine. Designeris loogika rakenduste käivitamiseks lisada, ja seejärel lisage toimingu. Valige rippmenüüst **Kuva Microsoft hallatav API-d** , ja sisestage otsinguväljale "sql". Tehke toimingud:  

    ![SQL Azure'i ühenduse loomise etappi](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Kui te pole varem loonud kõik ühendused SQL-andmebaasiga, küsitakse teilt ühenduse üksikasju:  

    ![SQL Azure'i ühenduse loomise etappi](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Sisestage SQL-andmebaasi üksikasjad. Tärniga atribuudid on nõutav.

    | Atribuut | Üksikasjad |
|---|---|
| Ühenduse lüüs kaudu | Jätke ruut märkimata. Seda kasutatakse asutusesisese SQL serveriga ühenduse loomise korral. |
| Ühenduse nimi * | Sisestage mis tahes ühenduse nimi. | 
| SQL serveri nimi * | Sisestage serveri nimi; mis on umbes *servername.database.windows.net*. Serveri nimi on portaalis Azure SQL-andmebaasi atribuutide kuvamine ja ka kuvatakse ühendusstring. | 
| SQL-i andmebaasi nimi * | Sisestage andsite SQL-andmebaasi nimi. See on loetletud SQL-andmebaasi atribuutide ühendusstringi: Lähtekataloogi =*yoursqldbname*. | 
| Kasutajanimi * | Sisestage kasutajanimi, mis on loodud SQL-andmebaasi loomisel. See on loetletud portaalis Azure SQL-andmebaasi atribuudid. | 
| Parooli * | Sisestage parool, mis on loodud SQL-andmebaasi loomisel. | 

    Neid mandaate ühenduse loomiseks ja SQL-i teie andmetele juurde pääseda rakenduse loogika autoriseerida. Kui olete lõpetanud, oma ühenduse üksikasjad välja nägema järgmine:  

    ![SQL Azure'i ühenduse loomise etappi](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Valige **Loo**. 

5. Pange tähele, et ühendus on loodud. Nüüd jätkake oma loogika rakenduse juhiseid: 

    ![SQL Azure'i ühenduse loomise etappi](./media/connectors-create-api-sqlazure/table.png)