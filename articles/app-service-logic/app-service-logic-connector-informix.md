<properties
   pageTitle="Teenuses Microsoft Azure'i rakenduse kasutamisega informixi | Microsoft Azure'i"
   description="Kuidas kasutada informixi konnektor loogika rakenduse päästikute ja toimingud"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="informix-connector"></a>Informixi konnektor
>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon.

Informixi Microsoft konnektor on rakendus API rakenduste Azure'i rakendust Service kaudu ühenduse ressursse, mis on salvestatud IBM informixi andmebaasis. Konnektor sisaldab Microsoft Client ühenduse informixi serveri kaugarvutite üle TCP/IP võrguühendust, sh Azure hübriid ühenduste informixi kohapealsed serverid, kasutades Azure'i teenus siini Relay. Konnektor toetab andmebaasi järgmisi toiminguid:

- Valige read ridade abil
- Küsitluse lugeda abil valige arv, millele järgneb valige read
- Ühe rea või kasutades Lisa ridu (hulgi) lisamine
- Muuta ühe rea või mitme (hulgi) rea UPDATE'I abil
- Ühe rea või kasutada käsku Kustuta ridu (hulgi) eemaldamine
- Vt ridu, valige KURSOR järgneb UPDATE kui praegune, KURSORI abil muuta
- Lugege abil valige KURSOR, millele järgneb UPDATE kui praegune, KURSOR rea eemaldamine
- Toimingu käivitada sisend- ja parameetriga, tagastusväärtus, resultset, kõne abil
- Kohandatud käsud ja kombineeritud toimingute abil valimine, lisamine, värskendamine, kustutamine

## <a name="triggers-and-actions"></a>Päästikute ja toimingud
Konnektor toetab järgmisi loogika rakenduse päästikute ja toimingud.

Päästikute | Toimingud
--- | ---
<ul><li>Küsitluse andmed</li></ul> | <ul><li>Hulgi lisada</li><li>Lisa</li><li>Hulgi värskendamine</li><li>Värskendamine</li><li>Helistamine</li><li>Hulgi kustutamine</li><li>Kustutamine</li><li>Valige</li><li>Tingimusvormingu värskendamine</li><li>Postita Olemikomplekt</li><li>Tingimusvormingu kustutamine</li><li>Ühe üksuse valimine</li><li>Kustutamine</li><li>Kui soovite Olemikomplekt upsert</li><li>Kohandatud käsud</li><li>Kombineeritud toimingud</li></ul>


## <a name="create-the-informix-connector"></a>Informixi konnektor loomine
Saate määratleda konnektori loogika rakenduses või Azure'i turuplatsilt, nagu järgmises näites:  

1. Azure'i startboard, valige **turuplatsilt**.
2. Tippige väljale **Otsi kõike** **informixi** **Kõik** tera, ja klõpsake sisestusklahvi (enter).
3. Otsi kõike otsingutulemite paan, valige **informixi konnektor**.
4. Informixi konnektori kirjeldus tera, valige **Loo**.
5. Informixi konnektori paketi tera, sisestage nimi (nt "InformixConnectorNewOrders"), rakenduse teenuse kavandamine ja muid atribuute.
6. Valige **paketi sätted**ja sisestage paketi järgmised sätted.

    Nimi | Nõutav |  Kirjeldus
--- | --- | ---
ConnectionString | Jah | Informixi kliendi ühendusstringi (nt "võrgu aadress = serveri nimi; Võrgu Port = 9089; Kasutaja ID-d = kasutajanimi; Parool = parool; Lähtekataloogi = nwind; vaikimisi skeemi informixi = ").
Tabelid | Jah | Komaga eraldatud loend tabeli, vaate ja alias nimed, mis on nõutav OData toimingute ja luua ärplema dokumentatsiooni näiteid (nt "NEWORDERS").
Juhised | Jah | Komaga eraldatud loend protseduur ja funktsioon nimed (nt "SPORDERID").
Valikul Asutusesisene | Ei | Juurutamine asutusesisese abil Azure'i teenus siini Relay.
ServiceBusConnectionString | Ei | Azure'i teenus siini Relay ühendusstring.
PollToCheckData | Ei | Kasutamiseks loogika rakenduse käivitamiseks valige COUNT lause (nt "SELECT COUNT (\*) kaudu NEWORDERS kus TARNEKUUPÄEV IS NULL").
PollToReadData | Ei | SELECT-lause kasutamine loogika rakenduse päästik (nt "valige \* NEWORDERS, kus TARNEKUUPÄEV on NULL FOR värskendamine:").
PollToAlterData | Ei | VÄRSKENDUSE või Kustuta lause loogika rakenduse käivitamiseks kasutada (nt "UPDATE NEWORDERS seadmine TARNEKUUPÄEV = praeguse kuupäeva kui praegune, &lt;KURSOR&gt;").

7. Klõpsake nuppu **OK**ja seejärel valige **Loo**.
8. Kui olete lõpetanud, paketi sätted välja nägema umbes järgmine:  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>Loogika informixi konnektori toimingu andmete lisamiseks rakenduse ##
Loogika rakenduse toimingu andmete lisamiseks informixi tabeli, mis API Insert või postituse üksuse OData toimingu abil saate määratleda. Näiteks saate lisada uue kirje kliendi tellimuse töötlemine SQL-i lisamine lause määratletud identiteedi veeruga tabeli suhtes, identiteedi väärtust või loogika rakendusse mõjutatud ridade (valige ORDID lõplik TABELIST (lisamine RAKENDUSSE NEWORDERS (CUSTID, TarneNimi kirjed, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) VÄÄRTUSI (?,?,?,?,?,?))).

> [AZURE.TIP] Informixi ühendus "*Postita Olemikomplekt*" tagastab identiteedi veeru väärtust ja "*API lisa*" tagastab mõjutatud ridade

1. Azure'i startboard, valige **+** (plussmärk) **Web + Mobile**, ja seejärel **loogika rakendus**.
2. Sisestage nimi (nt "NewOrdersInformix"), rakenduse teenuse kavandamine, muud atribuudid ja seejärel nuppu **Loo**.
3. Azure'i startboard, valige loogika äsja loodud rakenduse, **sätted**, ja seejärel **päästikute ja toimingud**.
4. Päästikute ja toimingute tera, valige **loomine algusest peale** loogika rakendusest mallid.
5. Paanil API rakendused valige **Korduvus**, sageduse ja intervall, ja seejärel **märkige ruut**määramine.
6. Paanil API rakendused valige **informixi konnektor**, laiendage **NEWORDER lisamiseks**valige toimingute loendist.
7. Laiendage parameetrite loendis sisestage järgmised väärtused:  

    Nimi | Väärtus
--- | --- 
CUSTID | 10042
SHIPID | 10000
TARNENIMI KIRJED | Rongile K Kountry pood 
SHIPADDR | 12 orchestra Terrass
SHIPCITY | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Valige **märge** salvestada selle toimingu sätted ja seejärel valige **Salvesta**.
9. Sätete peaks välja nägema järgmine:  
![][3]  
10. **Kõik töötab** loendis **toimingute**all (kõige uuemad Käivita) esimese loendis üksuse valimine. 
11. Valige **toiming** üksuse **informixconnectorneworders**labale **loogika rakenduse käivitamine** .
12. Valige **loogika rakenduse** tiiviknoad **SISENDINA LINK**. Informixi Konnektori kasutab sisendeid parameetritega lisa lause.
13. Valige **loogika rakenduse** tiiviknoad **VÄLJUNDEID LINK**. Sisendeid peaks välja nägema järgmine:  
![][4]

#### <a name="what-you-need-to-know"></a>Mida on vaja teada

- Konnektor kärbitakse informixi tabelinimede kui moodustavad loogika rakenduse toimingu nimed. Näiteks kärbitakse **Insert into NEWORDERS** toimingu lisamiseks **NEWORDER**.
- Pärast salvestamist loogika rakenduse **päästikute ja toimingute**, loogika rakenduse töötleb toiming. Võib kuluda mitme sekundi (nt 3 – 5 sekundites) enne loogika appi töötleb toiming. Teise võimalusena võite klõpsata **Rakenda kohe** tegevuse sooritamiseks.
- Informixi konnektori määratleb Olemikomplekt atribuutidega, sh kas vaikimisi informixi veergu, mis vastab liige või liikmed loodud veergude (nt identiteet). Loogika rakendus kuvatakse punane tärn Olemikomplekt liikme ID nime kõrval olevat tähistamiseks informixi veerud, mis nõuavad väärtused. Sisestada väärtuse ORDID liikme, mis vastab informixi identiteedi veeru. Saate sisestada muude valikuline liikmete (üksused, ORDDATE, REQDATE, SHIPID, VEOKULU, SHIPCTRY), mis vastavad informixi veergude vaikeväärtustega väärtused. 
- Informixi konnektori tagastab loogika rakendusse vastuse Olemikomplekt, mis sisaldab väärtusi identiteedi veerge, mis on saadud DRDA SQLDARD (SQL-i andmete ala vasta andmed) valmis SQL-i lisamine aruanne postitada. Informixi server ei tagasta vaikeväärtustega veergude lisatud väärtused.  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>Loogika informixi konnektori toimingu andmete lisamiseks rakenduse ##
Loogika rakenduse toimingu lisamiseks andmete informixi tabelisse API hulgi lisada toimingu abil saate määratleda. Näiteks saate lisada kaks uut kliendi tellimuse kirjet, mis töötlemine märge SQL-i lisamine abil vastu määratletud identiteedi veeruga tabeli rea väärtused massiivi esitus loogika rakendusse mõjutatud ridade (valige ORDID lõplik TABELIST (lisamine RAKENDUSSE NEWORDERS (CUSTID, TarneNimi kirjed, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) VÄÄRTUSI (?,?,?,?,?,?))).

1. Azure'i startboard, valige **+** (plussmärk) **Web + Mobile**, ja seejärel **loogika rakendus**.
2. Sisestage nimi (nt "NewOrdersBulkInformix"), rakenduse teenuse kavandamine, muud atribuudid ja seejärel nuppu **Loo**.
3. Azure'i startboard, valige loogika äsja loodud rakenduse, **sätted**, ja seejärel **päästikute ja toimingud**.
4. Päästikute ja toimingute tera, valige **loomine algusest peale** loogika rakendusest mallid.
5. Paanil API rakendused valige **Korduvus**, sageduse ja intervall, ja seejärel **märkige ruut**määramine.
6. Paanil API rakendused valige **informixi konnektor**, laiendage toimingute loendist valimiseks **Hulgi lisada uus**.
7. Sisestage **ridade** massiivina. Näiteks, kopeerige ja kleepige järgmine:  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
    ```
        
8. Valige **märge** salvestada selle toimingu sätted ja seejärel valige **Salvesta**. Sätete peaks välja nägema järgmine:  
![][6]

9. Klõpsake loendis **kõik töötab** **toimingute**all esimese loendis üksust (kõige uuemad Käivita).
10. Klõpsake **toimingu** üksust labale **loogika rakenduse käivitamine** .
11. Klõpsake **loogika rakenduse** tiiviknoad **SISENDINA LINK**. Väljundid peaks välja nägema järgmine:  
[][7]
12. Klõpsake **loogika rakenduse** tiiviknoad **VÄLJUNDEID LINK**. Väljundid peaks välja nägema järgmine:  
![][8]

#### <a name="what-you-need-to-know"></a>Mida on vaja teada

- Konnektor kärbitakse informixi tabelinimede kui moodustavad loogika rakenduse toimingu nimed. Näiteks **Hulgi lisada NEWORDERS** toimingu kärbitakse **Hulgi lisada uus**.
- Informixi andmebaasi võib olla tõstutundlik tabeli- ja veerunimesid. Näiteks hulgi lisada toiming massiivi veergude nimed võimalik, et peate olema määratud väiketähti ("custid") asemel läbivate suurtähtedega ("CUSTID").
- Jättes identiteedi veergude (nt ORDID), nullable veergude (nt TARNEKUUPÄEV) ja veergude vaikeväärtustega (nt ORDDATE, REQDATE, SHIPID, VEOKULU, SHIPCTRY), loob informixi andmebaasi väärtused.
- "Täna" ja "homme" määrates informixi konnektori genereeritud "Praegune kuupäev" ja "praegune kuupäev + 1 päeva-funktsioone (nt REQDATE). 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>Loogika rakenduse informixi konnektori päästik lugeda, muuta või kustutada andmete ##
Loogika rakenduse päästik küsitlus ja andmeid lugeda informixi tabelist API küsitluse andmete kombineeritud toimingu abil saate määratleda. Näiteks saate lugeda ühte või rohkem uut kliendi tellimuse kirjet, naasevad loogika rakenduse kirjed. Informixi ühendusesätteid paketi/rakenduse peaks välja nägema järgmine:

    Rakenduse säte | Väärtus
--- | --- | ---
PollToCheckData | Valige COUNT (\*) NEWORDERS, kus TARNEKUUPÄEV on NULL:
PollToReadData | Valige \* NEWORDERS, kus TARNEKUUPÄEV on NULL värskenduse kaudu
PollToAlterData | <no value specified>


Samuti saate määratleda loogika rakenduse päästik küsitlus, lugeda ja muuta API küsitluse andmete kombineeritud toimingu abil informixi tabelis andmete. Näiteks saate lugeda ühte või rohkem uute kliendi tellimuse kirjete värskendamine rea väärtused, naasevad loogika rakendus (Värskenda) enne valitud kirjed. Informixi ühenduse paketi/rakenduse sätted peaks välja nägema järgmine:

    Rakenduse säte | Väärtus
--- | --- | ---
PollToCheckData | Valige COUNT (\*) NEWORDERS, kus TARNEKUUPÄEV on NULL:
PollToReadData | Valige \* NEWORDERS, kus TARNEKUUPÄEV on NULL värskenduse kaudu
PollToAlterData | VÄRSKENDUSE NEWORDERS määramine TARNEKUUPÄEV = praeguse kuupäeva kui praegune, &lt;KURSOR&gt;


Lisaks saate määratleda loogika rakenduse päästik küsitlus, lugeda ja andmete kasutamise toimingu API küsitluse andmete kombineeritud informixi tabelist eemaldada. Näiteks saate lugeda üks või mitu uut kliendi tellimuse kirjet, Kustuta read, naasevad loogika rakendus (Kustuta) enne valitud kirjed. Informixi ühendusesätteid paketi/rakenduse peaks välja nägema järgmine:

    Rakenduse säte | Väärtus
--- | --- | ---
PollToCheckData | Valige COUNT (\*) NEWORDERS, kus TARNEKUUPÄEV on NULL:
PollToReadData | Valige \* NEWORDERS, kus TARNEKUUPÄEV on NULL värskenduse kaudu
PollToAlterData | Kustuta NEWORDERS kui praegune, &lt;KURSOR&gt;

Selles näites loogika rakendus on küsitlus, lugemine, värskendamine ja seejärel uuesti lugeda informixi tabeli andmed.

1. Azure'i startboard, valige **+** (plussmärk) **Web + Mobile**, ja seejärel **loogika rakendus**.
2. Sisestage nimi (nt "ShipOrdersInformix"), rakenduse teenuse kavandamine, muud atribuudid ja seejärel nuppu **Loo**.
3. Azure'i startboard, valige loogika äsja loodud rakenduse, **sätted**, ja seejärel **päästikute ja toimingud**.
4. Päästikute ja toimingute tera, valige **loomine algusest peale** loogika rakendusest mallid.
5. Paanil API rakendused valige **informixi konnektor**, sageduse ja intervall, ja seejärel **märkige ruut**määramine.
6. Paanil API rakendused valige **informixi konnektor**, laiendage toimingute loendist valimiseks **Valige NEWORDERS**.
7. Valige **märge** salvestada selle toimingu sätted ja seejärel valige **Salvesta**. Sätete peaks välja nägema järgmine:  
![][10]
8. **Päästikute ja toimingute** blade sulgemiseks klõpsake nuppu ja klõpsake **sätted** tera sulgemiseks.
9. Klõpsake loendis **kõik töötab** **toimingute**all esimese loendis üksust (kõige uuemad Käivita).
10. Klõpsake **toimingu** üksust labale **loogika rakenduse käivitamine** .
11. Klõpsake **loogika rakenduse** tiiviknoad **VÄLJUNDEID LINK**. Väljundid peaks välja nägema järgmine:  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>Loogika rakenduse informixi konnektori toimingu andmete eemaldamine ##
Saate määratleda loogika rakenduse toimingu andmete kasutamine postituse või API Kustuta üksus OData toimingu informixi tabelist eemaldada. Näiteks saate lisada uue kirje kliendi tellimuse töötlemine SQL-i lisamine lause määratletud identiteedi veeruga tabeli suhtes, identiteedi väärtust või loogika rakendusse mõjutatud ridade (valige ORDID lõplik TABELIST (lisamine RAKENDUSSE NEWORDERS (CUSTID, TarneNimi kirjed, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) VÄÄRTUSI (?,?,?,?,?,?))).

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>Informixi kasutamisega andmete eemaldamiseks loogika rakenduse loomine ##
Saate luua uue loogika rakenduse: Azure'i turuplatsi sees ja seejärel kasutage informixi konnektor toimingu eemaldamiseks klientide tellimusi. Näiteks saate kasutada Kustutustoiming informixi konnektori tingimusvormingu töödelda SQL-i kustutamine lause (kustutamine kaudu NEWORDERS kus ORDID > = 10000).

1. Azure'i **käivitamine** tahvli jaoturi menüüs nuppu **+** (plussmärk), klõpsake **Web + Mobile**, ja seejärel nuppu **loogika rakendus**. 
2. **Loogika loomine rakenduse** tera, tippige **nimi**, näiteks **RemoveOrdersInformix**.
3. Valige või Määratlege väärtused ka muud sätted (nt teenusleping, ressursirühm).
4. Sätete peaks välja nägema järgmine. Klõpsake nuppu **Loo**.  
![][12]
5. Klõpsake **sätete** labale **päästikute ja toimingud**.
6. **Päästikute ja toimingute** labale **loogika rakenduste Mallid** loendis, klõpsake **loomine algusest peale**.
7. **Päästikute ja toimingute** labale paanil **API rakenduste** rühmas ressursi nuppu **Korduvus**.
8. Konstruktsioonipinnalt loogika rakenduse nuppu **Korduvus** üksus, **sageduse** ja **intervall**, näiteks **päeva** ja **1**, ja **märkige ruut** Korduvus üksuse sätete salvestamiseks klõpsake.
9. Klõpsake paanil **API rakenduste** rühmas ressursi blade **päästikute ja toimingute** **informixi konnektor**.
10. Konstruktsioonipinnalt loogika rakenduse klõpsake üksust **informixi konnektori** toimingu, klõpsake kolmikpunkti (…****) toimingute loendi laiendamiseks ja klõpsake **tingimuslik kustutada N**.
11. Informixi konnektori toimingu üksusel on tippige **ordid ge 10000** **avaldis, mis tuvastab kirjete alamhulga**.
12. Klõpsake Toimingusätete salvestamiseks **märkige ruut** ja klõpsake siis nuppu **Salvesta**. Sätete peaks välja nägema järgmine:  
![][13]
13. **Päästikute ja toimingute** blade sulgemiseks klõpsake nuppu ja klõpsake **sätted** tera sulgemiseks.
14. Klõpsake loendis **kõik töötab** **toimingute**all esimese loendis üksust (kõige uuemad Käivita).
15. Klõpsake **toimingu** üksust labale **loogika rakenduse käivitamine** .
16. Klõpsake **loogika rakenduse** tiiviknoad **VÄLJUNDEID LINK**. Väljundid peaks välja nägema järgmine:  
![][14]

**Märkus:** Loogika rakenduse designer kärbitakse tabelinimed. Näiteks kärbitakse **tingimuslik**kustutamiseks N **tingimuslik NEWORDERS kustutamine** toimingut.


> [AZURE.TIP] Järgmine SQL-lauseid abil saate luua näidistabeli ja salvestatud toimingute. 

Saate luua NEWORDERS näidistabeli abil informixi SQL-i DDL järgmistest:
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


Saate luua valimi SPORDERID salvestatud protseduuri abil informixi DDL järgmine tekst:
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>Hübriidkonfiguratsioonis (valikuline)

> [AZURE.NOTE] See on vajalik ainult siis, kui kasutate tulemüüri taha DB2 konnektor kohapealse.

Rakenduse teenus kasutab hübriid Configuration Manager teie asutusesisese süsteemi turvaliselt ühendada. Kui konnektor kasutab kohapealse IBM DB2 Server for Windows, on vaja ühenduse hübriid ülemus.

Vaadake [hübriidjuurutuse ühendust halduriga](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Lisavõimalused oma konnektor
Nüüd, kui konnektor on loodud, saate selle lisada töövoos loogika rakenduse kasutamine. Vt [mis on loogika rakendused?](app-service-logic-what-are-logic-apps.md).

Looge API rakenduste abil REST API-d. Vt [konnektorid ja API rakenduste viide](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Saate vaadata ka jõudluse statistika ja juhtelemendi turvalisus konnektor. Vaadata, [hallata ja jälgida oma sisseehitatud API rakendused ja konnektorid](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png


