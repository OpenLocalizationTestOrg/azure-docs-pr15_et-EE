<properties
    pageTitle="Õpetus: Web app koos mitme rentniku andmebaasi üksuse raames ja rea taseme turvalisus abil"
    description="Saate teada, kuidas töötada mitme rentniku SQL-andmebaasi backent, kasutades üksuse raames ja rea taseme turvalisus ASP.net-i MVC 5 web app."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Õpetus: Web app koos mitme rentniku andmebaasi üksuse raames ja rea taseme turvalisus abil

Selle õpetuse näitab, kuidas koostada mitme rentniku veebirakenduse "[ühiskasutatava andmebaasiga, ühiskasutusega skeemi](https://msdn.microsoft.com/library/aa479086.aspx)" rendiõigus mudeli, üksuse raames ja [Rea taseme turvalisus](https://msdn.microsoft.com/library/dn765131.aspx)abil. Selle mudeli üks andmebaas sisaldab andmeid, paljude rentnike jaoks ja iga tabeli iga rea on seostatud "Rentniku ID" Rea taseme turvalisus (RLS), on uus funktsioon Azure'i SQL-andmebaasi, kasutatakse takistada rentnikud juurdepääsu üksteise andmeid. Selleks on vaja vaid ühe, väike muudatuse taotlus. Koondamise rentniku Accessi loogika andmebaas ise sees, RLS lihtsustab rakenduse koodi ja vähendab juhusliku andmeleket rentnikud vahel.

Alustame lihtsa Ärikontaktide halduri rakenduse [luua auth ja SQL-i DB ASP.net-i MVP rakendust ja võtta kasutusele Azure'i rakendust Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Paremale nüüd rakendus võimaldab kõik kasutajad vaadata kõik kontaktid (rentnike):

![Ärikontaktide halduri rakenduse enne RLS lubamine](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Vaid paari väike muudatustega lisame tugi mitme rendiõigus, nii, et kasutajad saavad vaadata ainult kontaktid, mis kuuluvad neile.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Samm 1: Lisa hõlmav tund Interceptor rakenduse määramiseks soovitud SESSION_CONTEXT

On ühe rakenduse muutmine läheb vaja teha. Kuna kõik rakenduse kasutajad andmebaasiga ühenduse loomiseks kasutab sama ühendusstringi (st sama SQL-i Logi sisse), praegu kuidagi RLS poliitika teada, milline see peaks filtreerivad kasutaja. See lähenemine on väga levinud veebirakendusi, kuna see lubab tõhusa ühendusepark, kuid see tähendab, et läheb vaja teine viis praeguse andmebaasi rakenduse kasutaja tuvastamiseks. Lahendus on on olemas paari võti-väärtus seadmine praeguse kasutajanimi on [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) kohe pärast avamist ühenduse, enne selle aktiveeritakse küsimusi. SESSION_CONTEXT on seansi ulatusega võti väärtus pood ja meie RLS poliitika on kasutajanimi, salvestatakse see abil saate tuvastada praeguse kasutaja.

Lisame [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (eriti on [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), uus funktsioon üksus Framework (EF) 6, automaatselt määramiseks praeguse kasutajanimi on SESSION_CONTEXT täites T-SQL-lauses iga kord, kui EF avatakse ühenduse.

1.  Avage projekt ContactManager Visual Studios.
2.  Paremklõpsake Solution Exploreris mudelite kausta ja valige Lisa > klassi.
3.  Selle uue ainekursuse "SessionContextInterceptor.cs" nimi ja klõpsake nuppu Lisa.
4.  Asendage SessionContextInterceptor.cs sisu järgmine kood.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

See on nõutav ainult rakenduse muutmine. Minna ja koostamine ja avaldamine rakenduse.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Samm 2: Andmebaasiskeem kasutajanimi veeru lisamine

Järgmiseks tuleb kasutajanimi veeru lisamiseks tabeli iga rea seostada kasutaja (rentnik) kontaktid. Me otse andmebaasi skeemi muuta nii, et meil ei ole lisada väli meie EF andmemudelis.

Andmebaasiga ühenduse loomiseks otse, SQL Server Management Studio või Visual Studio ja seejärel käivitada T-SQL.

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Kasutajanimi veeru lisatakse tabeli kontaktid. Kasutame UserIds, mis on talletatud AspNetUsers tabelis vastavaks nvarchar(128) andmetüüp ja loome vaikimisi piirangu, mis loob automaatselt selle äsja lisatud ridade kasutajanimi kasutajanimi, mis on talletatud praegu SESSION_CONTEXT olevat.

Nüüd tabel näeb välja umbes järgmine:

![Tabeli SSMS kontaktid](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Uute kontaktide loomisel neid kuvatakse automaatselt määrata õige kasutajanimi. Demo eesmärgil siiski vaatame määrata need olemasolevad kontaktid mõne olemasoleva kasutaja.

Kui olete loonud mõne kasutaja rakenduse juba (nt kohaliku abil, Google või Facebook kontod), kuvatakse need AspNetUsers tabelis. Pildil on ainult üks kasutaja siiani.

![SSMS AspNetUsers tabel](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopeerige Id user1@contoso.com, ja kleepige see allpool T-SQL-lause. Kolm kontaktid seostada selle kasutajanimi selle lause Execute.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Samm 3: Loo rea taseme turbepoliitika andmebaasis

Viimane toiming on luua turbepoliitika, mis kasutab funktsiooni kasutajanimi SESSION_CONTEXT automaatselt päringu tagastatud tulemite filtreerimiseks.

Veel ühendatud andmebaasi, käivitada T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Järgmine kood ei kolm üksust. Esmalt loob uue skeemi hea tava koondamise ja piirata juurdepääsu RLS objektide jaoks. Järgmisena loob põhiliste funktsioon, mis tagastab '1' kui vastab rea kasutajanimi kasutajanimi SESSION_CONTEXT sisse. Lõpetuseks, loob see turbepoliitika, mis lisab see funktsioon on filter ja Blokeeri predikaat kontaktide tabeli. Filtri predikaat põhjustab päringud, et tagastataks ainult praeguse kasutaja ridade ja Blokeeri predikaat toimib rakenduse kunagi kogemata vale kasutaja rea lisamistoimingu vältimiseks kaitsta.

Nüüd käivitage rakendus ja logige sisse user1@contoso.com. Selle kasutaja näeb nüüd ainult kontaktid, saame määratud selle kasutajanimi varasemas versioonis:

![Ärikontaktide halduri rakenduse enne RLS lubamine](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Kinnitage see edasi, proovige registreerimisel uus kasutaja. Nad näevad pole kontaktid, kuna neile on määratud mitte keegi. Kui need uue kontakti loomiseks seda määratakse neile ja ainult neid seda vaadata.

## <a name="next-steps"></a>Järgmised sammud

See on õige! Lihtne Contact Manager web appi teisendatud mitme rentniku, üks, kus igal kasutajal on oma kontaktiloendisse. Rea-turvalisuse abil olete me vältida keerukus jõustamine rentniku Accessi loogika meie rakenduse koodi. Selle läbipaistvus rakendusel keskenduda tõeline äri probleem käepärast ja seda ka vähendab kogemata lekib andmeid nagu rakenduse kasutaja codebase kasvab.

Selles õpetuses on ainult kriimustatud pind, mis on võimalik RLS. Näiteks on keerulisemad saab või Varundustöö Accessi loogika, ja see on võimalik talletada rohkem kui lihtsalt praeguse kasutajanimi on SESSION_CONTEXT. See on võimalik [integreerida RLS teekidega elastne andmebaasi tööriistad kliendi](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toetamiseks mitme rentniku shards skaala välja andmete astme.

Lisaks nendest võimalustest ka tegeleme RLS veelgi paremini teha. Kui teil on küsimusi, ideid, või asjad, mida soovite vaadata, palun andke meile teada kommentaarid. Teie tagasiside!
