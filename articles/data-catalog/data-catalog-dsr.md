<properties
   pageTitle="Azure'i andmekataloogi Toetatud andmeallikate | Microsoft Azure'i"
   description="Praegu toetatud andmeallikate määratlus."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Azure'i andmekataloogi toetatud andmeallikad

Azure'i andmete kataloogi kasutajad saavad avaldada metaandmete avaliku API, ühe hiireklõpsu abil – üks kord registreerimise tööriist või teabe otse andmine Andmekataloogis käsitsi sisestamine web portaalis. Järgmised ruudustiku Kokkuvõte toetatud kataloogi täna ja avaldamise võimaluste iga kõik allikad.  Ka loetletud on meie portaali "Ava-in" kogemus iga andmeallika käivitavad välisandmete tööriistu. Selle artikli teises tabelis on tehnilisemat kirjeldust iga andmete allikad ühenduse atribuudid.


## <a name="list-of-supported-data-sources"></a>Toetatud andmeallikate loendi

<table>

    <tr>
       <td><b>Andmete lähteobjekt</b></td>
       <td><b>API-GA</b></td>
       <td><b>Käsitsi sisestamine</b></td>
       <td><b>Registreerimise tööriist</b></td>
       <td><b>Ava tööriistad</b></td>
       <td><b>Märkmete</b></td>
    </tr>

    <tr>
      <td>Azure'i andmesalve Lake kataloog</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure'i Lake andmesalve faili</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Salvestusruumi Azure'i bloobimälu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure'i salvestusruumi kataloog</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Storage Tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS kataloog</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HDFS fail</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Taru tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Taru kuvamine</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-i tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-i vaade</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle'i andmebaasi tabelisse</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle'i andmebaasi vaade</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Muu (üldine varade)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL-i ladu andmetabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL-i andmete ladu vaade</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services mõõde</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Serveri analüüsiteenuste KPI-d</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services mõõt</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Serveri analüüsiteenuste teenuste tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL serveri aruandlusteenuste aruanne</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Brauseris</font></td>
      <td><font size=2>Ainult omarežiimis serverites. SharePointi režiimis ei toetata.</font></td>
    </tr>

    <tr>
      <td>SQL serveri tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Serveri vaade</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata kuvamine</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Exceli</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP-i Hana kuvamine</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Arvutamise vaadete ja analüüsivaateid ainult; Atribuut vaated pole toetatud.</font></td>
    </tr>

    <tr>
      <td>Db2 tabel</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Db2 kuvamine</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Faili süsteemifail</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-kaust</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-fail</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Http-aruanne</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Http lõpp-punkt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Http-fail</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData üksuse määramine</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData funktsioon</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>PostgreSQL-i tabel</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>PostgreSQL-i vaade</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP-i Hana kuvamine</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Salesforce'i objekt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SharePointi loend </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Kui vajate täiendavaid allikate tugi, esitama funktsiooni kasutades [Azure andmekataloogi Foorum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Andmeallika viide spetsifikatsioon
> [AZURE.NOTE] "DSL struktuur" veerus järgmine tabel ainult loendid ühenduse atribuutide "aadress" Atribuudikonteineri kasutavad Azure'i andmekataloogi (st "aadress" Atribuudikonteineri võib sisaldada muude ühenduse atribuudid, mis Azure'i andmekataloogi ei lahene, kuid ei kasuta andmeallika.)
<table>
    <tr>
       <td><b>Andmeallika tüüp</b></td>
       <td><b>Varade tüüp</b></td>
       <td><b>Objekti tüüp tüübid</b></td>
       <td><b>DSL struktuur<b></td>
    </tr>
    <tr>
      <td>Azure'i andmesalve Lake</td>
      <td>Container</td>
      <td>Andmete Lake</td>
      <td>
        <font size=2>protokoll: webhdfs <br>autentimise: {basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Azure'i andmesalve Lake</td>
      <td>Tabel</td>
      <td>Kataloogi fail</td>
      <td>
        <font size=2>protokoll: webhdfs <br>autentimise: {basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Azure'i salvestusruum</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>protokoll: azure plekid <br>autentimine: {Azure'i juurdepääsu võti} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeeni <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Container</font>
      </td>
    </tr>
    <tr>
      <td>Azure'i salvestusruum</td>
      <td>Tabel</td>
      <td>Bloobimälu kataloog</td>
      <td>
        <font size=2>protokoll: azure plekid <br>autentimine: {Azure'i juurdepääsu võti} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeeni <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Container <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nimi</font>
      </td>
    </tr>
    <tr>
      <td>Azure'i salvestusruum</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>protokoll: Azure'i-tabelite <br>autentimine: {Azure'i juurdepääsu võti} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeeni <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto</font>
      </td>
    </tr>
    <tr>
      <td>Azure'i salvestusruum</td>
      <td>Tabel</td>
      <td>Tabel</td>
      <td>
        <font size=2>protokoll: Azure'i-tabelite <br>autentimine: {Azure'i juurdepääsu võti} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeeni <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nimi</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Container</td>
      <td>Virtuaalne kobar</td>
      <td>
        <font size=2>protokoll: cosmos <br>autentimine: {basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tabel</td>
      <td>Voo voo kogum, vaade</td>
      <td>
        <font size=2>protokoll: cosmos <br>autentimine: {basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Container</td>
      <td>Saidi</td>
      <td>
        <font size=2>protokoll: http <br>autentimine: {pole, windows, basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Aruanne</td>
      <td>Aruande armatuurlaud</td>
      <td>
        <font size=2>protokoll: http <br>autentimine: {pole, windows, basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Container</td>
      <td>Andmebaasi</td>
      <td>
        <font size=2>protokoll: db2 <br>autentimine: {basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Tabel</td>
      <td>Tabeli, vaate</td>
      <td>
        <font size=2>protokoll: db2 <br>autentimine: {basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi</font>
      </td>
    </tr>
    <tr>
      <td>Failisüsteemis</td>
      <td>Tabel</td>
      <td>Faili</td>
      <td>
        <font size=2>protokoll: fail <br>autentimine: {pole, basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tee</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabel</td>
      <td>Kataloogi fail</td>
      <td>
        <font size=2>protokoll: ftp <br>autentimine: {pole, basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Hadoopi Hajusfailisüsteem</td>
      <td>Container</td>
      <td>Kobar</td>
      <td>
        <font size=2>protokoll: webhdfs <br>autentimise: {basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Hadoopi Hajusfailisüsteem</td>
      <td>Tabel</td>
      <td>Kataloogi fail</td>
      <td>
        <font size=2>protokoll: webhdfs <br>autentimise: {basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Taru</td>
      <td>Container</td>
      <td>Andmebaasi</td>
      <td>
        <font size=2>protokoll: taru <br>autentimine: {hdinsightiga, basic, kasutajanimi, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Taru</td>
      <td>Tabel</td>
      <td>Tabeli, vaate</td>
      <td>
        <font size=2>protokoll: taru <br>autentimine: {hdinsightiga, basic, kasutajanimi, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Http kaudu</td>
      <td>Container</td>
      <td>Saidi</td>
      <td>
        <font size=2>protokoll: http <br>autentimine: {pole, windows, basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Http kaudu</td>
      <td>Aruanne</td>
      <td>Aruande armatuurlaud</td>
      <td>
        <font size=2>protokoll: http <br>autentimine: {pole, windows, basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Http kaudu</td>
      <td>Tabel</td>
      <td>Lõpp-punkt, fail</td>
      <td>
        <font size=2>protokoll: http <br>autentimine: {pole, windows, basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>MySQL-i</td>
      <td>Container</td>
      <td>Andmebaasi</td>
      <td>
        <font size=2>protokoll: MySQL-i <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi</font>
      </td>
    </tr>
    <tr>
      <td>MySQL-i</td>
      <td>Tabel</td>
      <td>Tabeli, vaate</td>
      <td>
        <font size=2>protokoll: MySQL-i <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Üksuse Container</td>
      <td>
        <font size=2>protokoll: odata <br>autentimine: {pole, basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabel</td>
      <td>Üksuse määramine, funktsioon</td>
      <td>
        <font size=2>protokoll: odata <br>autentimine: {pole, basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ressurss</font>
      </td>
    </tr>
    <tr>
      <td>Oracle'i andmebaasiga</td>
      <td>Container</td>
      <td>Andmebaasi</td>
      <td>
        <font size=2>protokoll: Oracle'i <br>autentimine: {protokoll, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi</font>
      </td>
    </tr>
    <tr>
      <td>Oracle'i andmebaasiga</td>
      <td>Tabel</td>
      <td>Tabeli, vaate</td>
      <td>
        <font size=2>protokoll: Oracle'i <br>autentimine: {protokoll, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL-i</td>
      <td>Container</td>
      <td>Andmebaasi</td>
      <td>
        <font size=2>protokoll: PostgreSQL-i <br>autentimine: {basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL-i</td>
      <td>Tabel</td>
      <td>Tabeli, vaate</td>
      <td>
        <font size=2>protokoll: PostgreSQL-i <br>autentimine: {basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Container</td>
      <td>Saidi</td>
      <td>
        <font size=2>protokoll: http <br>autentimine: {pole, windows, basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Aruanne</td>
      <td>Aruande armatuurlaud</td>
      <td>
        <font size=2>protokoll: http <br>autentimine: {pole, windows, basic, OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tabel</td>
      <td>Andmete seguri</td>
      <td>
        <font size=2>protokoll: power-päring <br>autentimine: {OAuthi} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>Salesforce'i</td>
      <td>Tabel</td>
      <td>Objekti</td>
      <td>
        <font size=2>protokoll: salesforce'i-com <br>autentimine: {basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;klassi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;üksuse nimi</font>
      </td>
    </tr>
    <tr>
      <td>SAP-i Hana</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>protokoll: sap-hana-SQL-is <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </td>
    </tr>
    <tr>
      <td>SAP-i Hana</td>
      <td>Tabel</td>
      <td>Vaade</td>
      <td>
        <font size=2>protokoll: sap-hana-SQL-is <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>SharePointi</td>
      <td>Tabel</td>
      <td>Loend</td>
      <td>
        <font size=2>protokoll: SharePointi loendisse <br>autentimine: {basic, windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i</font>
      </td>
    </tr>
    <tr>
      <td>SQL-i andmebaas</td>
      <td>Käsk</td>
      <td>Salvestatud protseduur</td>
      <td>
        <font size=2>protokoll: tds <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>SQL-i andmebaas</td>
      <td>TableValuedFunction</td>
      <td>Tabelväärtusega funktsiooni</td>
      <td>
        <font size=2>protokoll: tds <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>SQL-i andmebaas</td>
      <td>Container</td>
      <td>Andmebaasi</td>
      <td>
        <font size=2>protokoll: tds <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi</font>
      </td>
    </tr>
    <tr>
      <td>SQL-i andmebaas</td>
      <td>Tabel</td>
      <td>Tabeli, vaate</td>
      <td>
        <font size=2>protokoll: tds <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>SQL serveri</td>
      <td>Käsk</td>
      <td>Salvestatud protseduur</td>
      <td>
        <font size=2>protokoll: tds <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>SQL serveri</td>
      <td>TableValuedFunction</td>
      <td>Tabelväärtusega funktsiooni</td>
      <td>
        <font size=2>protokoll: tds <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>SQL serveri</td>
      <td>Container</td>
      <td>Andmebaasi</td>
      <td>
        <font size=2>protokoll: tds <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi</font>
      </td>
    </tr>
    <tr>
      <td>SQL serveri</td>
      <td>Tabel</td>
      <td>Tabeli, vaate</td>
      <td>
        <font size=2>protokoll: tds <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;skeemi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>SQL Serveri analüüsiteenuste mitmedimensiooniliste</td>
      <td>Container</td>
      <td>Mudel</td>
      <td>
        <font size=2>protokoll: analüüsiteenuste <br>autentimise: {windows, basic, Anonüümne, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel</font>
      </td>
    </tr>
    <tr>
      <td>SQL Serveri analüüsiteenuste mitmedimensiooniliste</td>
      <td>KPI-D</td>
      <td>KPI-D</td>
      <td>
        <font size=2>protokoll: analüüsiteenuste <br>autentimine: {windows, basic, Anonüümne, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektitüüp: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Serveri analüüsiteenuste mitmedimensiooniliste</td>
      <td>Mõõt</td>
      <td>Mõõt</td>
      <td>
        <font size=2>protokoll: analüüsiteenuste <br>autentimine: {windows, basic, Anonüümne, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektitüüp: {mõõt}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Serveri analüüsiteenuste mitmedimensiooniliste</td>
      <td>Tabel</td>
      <td>Mõõde</td>
      <td>
        <font size=2>protokoll: analüüsiteenuste <br>autentimine: {windows, basic, Anonüümne, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektitüüp: {mõõtme}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Serveri analüüsiteenuste tabelina</td>
      <td>Container</td>
      <td>Mudel</td>
      <td>
        <font size=2>protokoll: analüüsiteenuste <br>autentimise: {windows, basic, Anonüümne, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel</font>
      </td>
    </tr>
    <tr>
      <td>SQL Serveri analüüsiteenuste tabelina</td>
      <td>KPI-D</td>
      <td>KPI-D</td>
      <td>
        <font size=2>protokoll: analüüsiteenuste <br>autentimine: {windows, basic, Anonüümne, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektitüüp: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Serveri analüüsiteenuste tabelina</td>
      <td>Mõõt</td>
      <td>Mõõt</td>
      <td>
        <font size=2>protokoll: analüüsiteenuste <br>autentimine: {windows, basic, Anonüümne, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektitüüp: {mõõt}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Serveri analüüsiteenuste tabelina</td>
      <td>Tabel</td>
      <td>Tabel</td>
      <td>
        <font size=2>protokoll: analüüsiteenuste <br>autentimine: {windows, basic, Anonüümne, pole} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektitüüp: {tabel}</font>
      </td>
    </tr>
    <tr>
      <td>SQL serveri aruandlusteenused</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>protokoll: aruandlusteenuste <br>autentimine: {windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versioon: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL serveri aruandlusteenused</td>
      <td>Aruanne</td>
      <td>Aruanne</td>
      <td>
        <font size=2>protokoll: aruandlusteenuste <br>autentimine: {windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tee <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versioon: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Andmebaasi</td>
      <td>
        <font size=2>protokoll: teradata <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tabel</td>
      <td>Tabeli, vaate</td>
      <td>
        <font size=2>protokoll: teradata <br>autentimine: {Protocol (protokoll), windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;andmebaasi <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekti</font>
      </td>
    </tr>
    <tr>
      <td>SQL serveri juhtslaidi Data Services</td>
      <td>Container</td>
      <td>Mudel</td>
      <td>
        <font size="2">protokoll: mssql-MDS-i <br>autentimine: {windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versioon</font>
      </td>
    </tr>
    <tr>
      <td>SQL serveri juhtslaidi Data Services</td>
      <td>Tabel</td>
      <td>Üksuse</td>
      <td>
        <font size="2">protokoll: mssql-MDS-i <br>autentimine: {windows} <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-i <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Mudel <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versioon <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;üksuse</font>
      </td>
    </tr>
    <tr>
      <td>Teine (mitte eespool)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>protokoll: Üldine-vara <br>aadress: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assetId</font>
      </td>
    </tr>
</table>
