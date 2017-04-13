<properties
   pageTitle="SQL-i kasutamisega loogika rakendustes | Microsoft Azure'i rakendust Service"
   description="Kuidas luua ja SQL-i konnektori või API rakenduse konfigureerimiseks ja kasutamine loogika rakenduse teenuses Azure rakendus"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Alustamine Microsoft SQL-i konnektor ja lisada selle oma loogika rakenduse
>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon. Azure SQL-i 2015-08-01-eelvaade skeemi versiooni, klõpsake nuppu [SQL Azure'i API -ga](../connectors/connectors-create-api-sqlazure.md).

Ühenduse loomine mõne asutusesisese SQL serveri või Azure'i SQL-andmebaasi luua ja muuta oma andmed või andmed. Loogika rakendustes saab kasutada konnektorid toomiseks, protsessi või lükake andmete "töövoo" osana. Oma töövoo konnektor SQL-i kasutamisel võite saavutada erinevaid stsenaariumeid lähemalt. Näiteks saate teha järgmist.

- Jätke elavate SQL-andmebaasi veebis või mobiilirakenduse abil andmete osa.
- Andmete lisamine Storage SQL-i andmebaasi tabelisse. Näiteks saate sisestada töötaja kirjed, värskendage müügitellimuste ja jms.
- Andmete toomine SQL-i ja seda kasutada äriprotsessi. Näiteks saate hankida klientide kirjeid ja Salesforce'i nende klientide kirjeid luua.

Saate lisada töövoog ja protsessi äriteabe osana töövoo loogika rakenduses SQL-i konnektor. 

## <a name="triggers-and-actions"></a>Päästikute ja toimingud
*Päästikute* on sündmused, mis tehakse. Näiteks kui värskendatakse tellimuse või kui uus klient on lisatud. *Toiming* on põhjustanud käivitada. Näiteks kui tellimuse värskendatakse, müügiesindaja teatise saata. Või kui lisatakse uus klient, saate saata uus klient tervituskirja.

SQL-i konnektor saab kasutada käivitamiseks või toimingu loogika rakendus ja toetab andmete JSON ja XML-vormingus. Iga tabeli sisalduvate sätted (rohkem, et selles teemas allpool), on JSON toimingute kogum ja XML-i toimingute kogum.

SQL-i konnektor on järgmine päästikute ja saadaolevad toimingud.

Päästikute | Toimingud
--- | ---
Küsitluse andmed | <ul><li>Tabeli lisamine</li><li>Värskenda tabel</li><li>Valige tabelist</li><li>Tabeli kustutamine</li><li>Kõne salvestatud protseduur</li></ul>

## <a name="create-the-sql-connector"></a>Konnektor SQL-i loomine

Konnektor saate luua loogika rakenduses või Azure'i turuplatsilt. Turuplatsilt konnektori loomiseks tehke järgmist.  

1. Azure'i startboard, valige **turuplatsilt**.
2. Otsige "SQL-i konnektor", valige see ja valige **Loo**.
3. Sisestage nimi, rakenduse teenuse kavandamine ja muid atribuute.
4. Sisestage paketi järgmised sätted:

    Nimi | Nõutav |  Kirjeldus
--- | --- | ---
Serveri nimi | Jah | Sisestage SQL serveri nimi. Sisestage näiteks *SQLserver/SQLExpressis* või *SQLserver.mydomain.com*.
Port | Ei | Vaikimisi on 1433.
Kasutajanimi | Jah | Sisestage kasutaja nimi, mida saate sisse logida, SQL Server. Kui asutusesisese SQL serveriga ühendatud, sisestage SQL-i autentimise kasutajanimi ja parool.
Parooli | Jah | Sisestage kasutajanime ja parooli.
Andmebaasi nimi | Jah | Sisestage loote andmebaasi. Näiteks saate sisestada *klientide* või *dbo/tellimused*.
Kohapealse | Jah | Vaikimisi on False. Sisestage False, kui Azure'i SQL-andmebaasiga ühenduse. Sisestage väärtuse True, kui asutusesisese SQL serveriga ühendatud.
Teenuse siini ühendusstring. | Ei | Kui loote ühenduse kohapealse, sisestage ühendusstring teenuse siini relay.<br/><br/>[Ühenduse hübriid halduri abil](app-service-logic-hybrid-connection-manager.md)<br/>[Teenuse siini hinnad](https://azure.microsoft.com/pricing/details/service-bus/)
Partneri serveri nimi | Ei | Kui esmane server pole saadaval, saate sisestada partner server nimega serveriks alternatiivse või varukoopia.
Tabelid | Ei | Konnektor värskendatavate andmebaasi tabelite loendi. Sisestage näiteks *OrdersTable* või *EmployeeTable*. Kui sisestatakse tabeleid, saab kõik tabelid. Konnektor selle toimingu kasutamiseks on vaja kehtivad tabelite ja/või salvestatud toimingute.
Salvestatud toimingute | Ei | Sisestage olemasoleva salvestatud protseduur, mida saab kutsuda konnektor. Sisestage näiteks *sp_IsEmployeeEligible* või *sp_CalculateOrderDiscount*. Konnektor selle toimingu kasutamiseks on vaja kehtivad tabelite ja/või salvestatud toimingute.
Andmete saadaval päring | Päästik tugiteenuste | SQL-lause kas kõik andmed on saadaval küsitlused SQL serveri andmebaasi tabelisse. See peaks tagastama arvulise väärtuse tähistav andmete ridade arv. Näide: Valige COUNT(*) tabeli_nimi.
Küsitluse andmete päring | Päästik tugiteenuste | SQL-lause küsitlus SQL serveri andmebaasi tabelisse. Saate sisestada suvalist arvu SQL-lauseid, eraldades need semikooloniga. See lause on täidetud ülekandega ja ainult kinnitatud, kui andmed on salvestatud turvaline loogika rakenduse. Näide: Valige *tabeli_nimi; Kustuta: tabeli_nimi. <br/>**Note**<br/>Peate esitama küsitluse teade, et vältida silmuses kustutamine, teisaldades või veenduge, et samad andmed pole uuesti küsitletutest valitud andmete värskendamine.

5. Kui olete lõpetanud, paketi sätted välja nägema umbes järgmine:  
![][1]  

6. Valige **Loo**. 


## <a name="use-the-connector-as-a-trigger"></a>Kasutage käivitamiseks konnektor
Vaatame lihtsa loogika rakendust SQL tabeli andmeid küsitlused, lisab teise tabeli andmetega ja värskendab andmeid.

SQL-i konnektor käivitamiseks kasutamiseks sisestage **Saadaval andmepäringu** ja **Küsitluse andmete päring** väärtused. **Andmete saadaval päring** käivitatakse ajakavas sisestate ja määrab, kas kõik andmed on saadaval. Kuna see päring tagastab ainult skalaarse numbri, saate selle häälestatud ja sagedased täitmise jaoks optimeeritud.

**Küsitluse andmete päring** käivitatakse ainult siis, kui andmete saadaval päring näitab, et andmed on saadaval. See lause sees tehingu aktiveeritakse ja on ainult siis, kui ekstraktitud andmed salvestatakse püsivalt oma töövoo kinnitatud. On oluline, et vältida lõputult uuesti ekstraktimiseks samad andmed. Selgituseks laadi täitmist saab kustutada või seda pole kogumise järgmine kord, kui andmed on esitama päringu andmeid värskendada.

> [AZURE.NOTE] See lause tagastatud skeemiga tuvastab teie konnektor saadaolevad atribuudid. Kõigi veergude peab nimega.

#### <a name="data-available-query-example"></a>Andmete saadaval päringu näide

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Küsitluse andmete päringu näide

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Päästiku lisamine
1. Loomisel või redigeerimisel loogika rakendus, valige loodud SQL-i konnektor käivitada. See on loetletud saadaval päästikute: **Küsitluse andmed (JSON)** ja **Küsitluse andmed (XML)**:  
![][5]

2. Valige **Küsitluse andmed (JSON)** päästik, sisestage sagedus ja valige soovitud ✓:  
![][6]

3. Käivitab kuvatakse nüüd rakenduse loogika konfigureeritud. Output(s), käivitab kuvatakse ja seda saab kasutada sisendina klõpsake mis tahes järgnevate toimingute:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Kasutage toimingut konnektor
Kasutades meie lihtsa loogika rakenduse stsenaarium, mis küsitlused andmed SQL tabelist teise tabeli andmetega lisab ja värskendab andmeid.

SQL-i konnektor toimingu kasutamiseks sisestage nimi tabelite ja/või salvestatud toimingute sisestatud loomisel SQL-i konnektor:

1. Pärast oma päästik (või valige "see loogika käsitsi käivitamine"), lisage loodud galeriist SQL-i konnektor. Valige üks lisa toimingud, nt *Lisamine rakendusse TempEmployeeDetails (JSON)*.  
![][8]

2. Sisestage sisendväärtuste lisada, ja klõpsake soovitud ✓ kirje:  
![][9]

3. Valige galeriis loodud sama SQL-i konnektor. Valige teatud toimingu, kui värskenduse toimingu sama tabeli, nt *Värskenduse EmployeeDetails*.  
![][11]

4. Sisestage sisendväärtuste värskenduse toimingu, ja klõpsake soovitud ✓:  
![][12]

Loogika rakenduse testimiseks tabeli, mis on küsitletutest uue kirje lisamine.

## <a name="what-you-can-and-cannot-do"></a>Mida saab teha ja mida ei saa

SQL-päringu | Toetatud | Pole toetatud
--- | --- | ---
Kus klausel | <ul><li>Tehtemärkide: Ja, või, =, <>, <, < =, >, > = ja meeldib</li><li>Mitme sub tingimuse saab kombineerida "(" ja")"</li><li>Tekstistringi literaalide, kuupäeva ja kellaaja (suletud ühe jutumärkideta), arvud (peaks sisaldama ainult numbreid)</li><li>Peaks tingimata olema kahendarvu avaldis kujul, nagu ((operand operator operand) ja/või (operandi tehtemärk operand)) *</li></ul> | <ul><li>Tehtemärgid: Vahel, tolli</li><li>Kõik sisseehitatud funktsioonid nagu ADD(), MAX() NOW(), POWER() ja jne</li><li>Matemaatilisi tehtemärke, näiteks *, -, +, jne</li><li>Stringi concatenations abil +.</li><li>Kõik ühendused</li><li>ON tühi ja pole tühi</li><li>Mis tahes arvude mittearvulise märki, nt kuueteistkümnendarvu arvud</li></ul>
Välju (valikupäringu) | <ul><li>Lubatud veergude nimed komadega. Pole tabeli nimi eesliiteid lubatud (konnektor töötab ühe tabeli korraga).</li><li>Nimesid saate tuleb seda vältida koos ' ["ja"] "</li></ul> | <ul><li>Märksõnade nagu ÜLEVAL, DISTINCT, jne</li><li>Aliast, nt tänav, linn + Zip AS aadress</li><li>Kõik sisseehitatud funktsioonid, näiteks ADD(), MAX() NOW(), POWER() ja jne</li><li>Matemaatilisi tehtemärke, nt *, -, +, jne</li><li>Stringi concatenations abil +</li></ul>

#### <a name="tips"></a>Näpunäited

- Täpsemaid päringuid, soovitame salvestatud protseduur ja käivitamine abil execute salvestatud protseduur API loomine.
- Sisemine päringute kasutamisel tuleb neid kasutada salvestatud toimingute sees.
- Ühendab mitu tingimust, saate kasutada "Ja" ja "OR" tehtemärke.

## <a name="hybrid-configuration-optional"></a>Hübriidkonfiguratsioonis (valikuline)

> [AZURE.NOTE] See on vajalik ainult siis, kui kasutate tulemüüri taha asutusesisese SQL serveri.

Rakenduse teenus kasutab hübriid Configuration Manager teie asutusesisese süsteemi turvaliselt ühendada. Kui olete konnektor kasutava ettevõtte asutusesisese SQL serveris, on vaja ühenduse hübriid ülemus.

> [AZURE.NOTE] Kui soovite alustada Azure'i loogika rakendused enne Azure'i konto kasutajaks, minge [Proovige loogika rakenduse](https://tryappservice.azure.com/?appservice=logic), kus saate kohe luua lühiajaline starter loogika rakendus App teenuses. Nõutav; krediitkaardid kohustusi.

Vaadake [hübriidjuurutuse ühendust halduriga](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Lisavõimalused oma konnektor
Nüüd, kui konnektor on loodud, saate selle lisada töövoos loogika rakenduse kasutamine. Vt [mis on loogika rakendused?](app-service-logic-what-are-logic-apps.md).

Vaadata ärplema REST API viide [konnektorid ja API rakenduste viide](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Saate vaadata ka jõudluse statistika ja juhtelemendi turvalisus konnektor. Vaadata, [hallata ja jälgida oma sisseehitatud API rakendused ja konnektorid](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
