<properties
   pageTitle="Teenuses Microsoft Azure'i rakenduse kasutamisega DB2 | Microsoft Azure'i"
   description="Kuidas kasutada DB2 konnektor loogika rakenduse päästikute ja toimingud"
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

# <a name="db2-connector"></a>DB2 konnektor
>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon.

Microsofti konnektor DB2 on rakendus API rakenduste Azure'i rakendust Service kaudu ühenduse loomine IBM DB2 andmebaasiga talletatud ressursid. Konnektor sisaldab Microsoft Client ühenduse DB2 server kaugarvutite üle TCP/IP võrguühendust, sh Azure hübriid ühenduste DB2 kohapealsed serverid, kasutades Azure'i teenus siini Relay konnektor toetab andmebaasi järgmisi toiminguid:

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


## <a name="create-the-db2-connector"></a>Looge DB2 konnektor
Saate määratleda konnektori loogika rakenduses või Azure'i turuplatsilt, nagu järgmises näites:  

1. Azure'i startboard, valige **turuplatsilt**.
2. Tippige väljale **Otsi kõike** **db2** **Kõik** tera, ja klõpsake sisestusklahvi (enter).
3. Otsi kõike otsingutulemite paan, valige **DB2 konnektor**.
4. DB2 konnektor kirjeldus tera, valige **Loo**.
5. DB2 konnektor paketi tera, sisestage nimi (nt "Db2ConnectorNewOrders"), rakenduse teenuse kavandamine ja muid atribuute.
6. Valige **paketi sätted**ja sisestage paketi järgmised sätted:  

    Nimi | Nõutav |  Kirjeldus
--- | --- | ---
ConnectionString | Jah | Ühendusstringi DB2 kliendi (nt "võrgu aadress = serveri nimi; Võrgu Port = 50000; Kasutaja ID-d = kasutajanimi; Parool = parool; Lähtekataloogi = näidis; Saidikogumi paketti = NWIND; vaikimisi skeemi = NWIND ").
Tabelid | Jah | Komaga eraldatud loend tabeli, vaate ja alias nimed, mis on nõutav OData toimingute ja luua ärplema dokumentatsiooni näiteid (nt "*NEWORDERS*").
Juhised | Jah | Komaga eraldatud loend protseduur ja funktsioon nimed (nt "SPORDERID").
Valikul Asutusesisene | Ei | Juurutamine asutusesisese abil Azure'i teenus siini Relay.
ServiceBusConnectionString | Ei | Azure'i teenus siini Relay ühendusstring.
PollToCheckData | Ei | Kasutamiseks loogika rakenduse käivitamiseks valige COUNT lause (nt "SELECT COUNT (\*) kaudu NEWORDERS kus TARNEKUUPÄEV IS NULL").
PollToReadData | Ei | SELECT-lause kasutamine loogika rakenduse päästik (nt "valige \* NEWORDERS, kus TARNEKUUPÄEV on NULL FOR värskendamine:").
PollToAlterData | Ei | VÄRSKENDUSE või Kustuta lause loogika rakenduse käivitamiseks kasutada (nt "UPDATE NEWORDERS seadmine TARNEKUUPÄEV = praeguse kuupäeva kui praegune, &lt;KURSOR&gt;").

7. Klõpsake nuppu **OK**ja seejärel valige **Loo**.
8. Kui olete lõpetanud, paketi sätted välja nägema umbes järgmine:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Loogika DB2 konnektor toimingu andmete lisamiseks rakenduse ##
Loogika rakenduse toimingu andmete lisamiseks DB2 tabeli API Insert või postituse üksuse OData toimingu abil saate määratleda. Näiteks saate lisada uue kirje kliendi tellimuse töötlemine SQL-i lisamine lause määratletud identiteedi veeruga tabel vastu, identiteedi väärtust või loogika rakendus (valige ORDID lõplik TABELIST (lisamine RAKENDUSSE NWIND mõjutatud ridade. NEWORDERS (CUSTID, TARNENIMI KIRJED, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) VÄÄRTUSI (?,?,?,?,?,?))).

> [AZURE.TIP] DB2 ühendus "*Postita Olemikomplekt*" tagastab identiteedi veeru väärtust ja "*API lisa*" tagastab mõjutatud ridade

1. Azure'i startboard, valige **+** (plussmärk) **Web + Mobile**, ja seejärel **loogika rakendus**.
2. Sisestage nimi (nt "NewOrdersDb2"), rakenduse teenuse kavandamine, muud atribuudid ja seejärel nuppu **Loo**.
3. Azure'i startboard, valige loogika äsja loodud rakenduse, **sätted**, ja seejärel **päästikute ja toimingud**.
4. Päästikute ja toimingute tera, valige **loomine algusest peale** loogika rakendusest mallid.
5. Paanil API rakendused valige **Korduvus**, sageduse ja intervall, ja seejärel **märkige ruut**määramine.
6. Paanil API rakendused valige **DB2 konnektor**, laiendage **NEWORDER lisamiseks**valige toimingute loendist.
7. Laiendage parameetrite loendis sisestage järgmised väärtused:  

    Nimi | Väärtus
--- | --- 
CUSTID | 10042
TARNENIMI KIRJED | Rongile K Kountry pood 
SHIPADDR | 12 orchestra Terrass
SHIPCITY | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Valige **märge** salvestada selle toimingu sätted ja seejärel valige **Salvesta**.
9. Sätete peaks välja nägema järgmine:  
![][3]

10. **Kõik töötab** loendis **toimingute**all (kõige uuemad Käivita) esimese loendis üksuse valimine. 
11. Valige **toimingu** üksuse **db2connectorneworders**labale **loogika rakenduse käivitamine** .
12. Valige **loogika rakenduse** tiiviknoad **SISENDINA LINK**. DB2 Konnektori kasutab sisendeid parameetritega lisa lause.
13. Valige **loogika rakenduse** tiiviknoad **VÄLJUNDEID LINK**. Sisendeid peaks välja nägema järgmine:  
![][4]

#### <a name="what-you-need-to-know"></a>Mida on vaja teada

- Konnektor kärbitakse DB2 tabelinimede kui moodustavad loogika rakenduse toimingu nimed. Näiteks kärbitakse **Insert into NEWORDERS** toimingu lisamiseks **NEWORDER**.
- Pärast salvestamist loogika rakenduse **päästikute ja toimingute**, loogika rakenduse töötleb toiming. Võib kuluda mitme sekundi (nt 3 – 5 sekundites) enne loogika appi töötleb toiming. Teise võimalusena võite klõpsata **Rakenda kohe** tegevuse sooritamiseks.
- DB2 konnektor määratleb Olemikomplekt liikmete atribuutidega, sh kas liige vastab vaikimisi DB2 veerg või veerud (nt identiteet) loodud. Loogika rakendus kuvatakse punane tärn Olemikomplekt liikme ID nime kõrval olevat tähistamiseks DB2 veerge, mis nõuavad väärtused. Sisestada väärtuse ORDID liikme, mis vastab DB2 identiteedi veeru. Saate sisestada muude valikuline liikmete (üksused, ORDDATE, REQDATE, SHIPID, VEOKULU, SHIPCTRY), mis vastavad DB2 veergude vaikeväärtustega väärtused. 
- DB2 konnektor tagastab loogika rakendusse vastuse Olemikomplekt, mis sisaldab väärtusi identiteedi veerge, mis on saadud DRDA SQLDARD (SQL-i andmete ala vasta andmed) valmis SQL-i lisamine aruanne postitada. DB2 server ei tagasta vaikeväärtustega veergude lisatud väärtused.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Loogika DB2 konnektor toimingu andmete lisamiseks rakenduse ##
Loogika rakenduse toimingu andmete lisamiseks DB2 tabeli API hulgi lisada toimingu abil saate määratleda. Näiteks saate lisada kaks uut kliendi tellimuse kirjet, SQL-i lisamine lause, kasutades vastu määratletud identiteedi veeruga tabeli rea väärtused massiivi töötlemine esitus loogika rakendus (valige ORDID lõplik TABELIST (lisamine RAKENDUSSE NWIND mõjutatud ridade. NEWORDERS (CUSTID, TARNENIMI KIRJED, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) VÄÄRTUSI (?,?,?,?,?,?))).

1. Azure'i startboard, valige **+** (plussmärk) **Web + Mobile**, ja seejärel **loogika rakendus**.
2. Sisestage nimi (nt "NewOrdersBulkDb2"), rakenduse teenuse kavandamine, muud atribuudid ja seejärel nuppu **Loo**.
3. Azure'i startboard, valige loogika äsja loodud rakenduse, **sätted**, ja seejärel **päästikute ja toimingud**.
4. Päästikute ja toimingute tera, valige **loomine algusest peale** loogika rakendusest mallid.
5. Paanil API rakendused valige **Korduvus**, sageduse ja intervall, ja seejärel **märkige ruut**määramine.
6. Paanil API rakendused valige **DB2 konnektor**, laiendage toimingute loendist valimiseks **Hulgi lisada uus**.
7. Sisestage **ridade** massiivina. Näiteks, kopeerige ja kleepige järgmine:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
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

- Konnektor kärbitakse DB2 tabelinimede kui moodustavad loogika rakenduse toimingu nimed. Näiteks **Hulgi lisada NEWORDERS** toimingu kärbitakse **Hulgi lisada uus**.
- Jättes identiteedi veerud (nt ORDID), nullable veerud (nt TARNEKUUPÄEV) ja veergude vaikeväärtustega (nt ORDDATE, REQDATE, SHIPID, VEOKULU, SHIPCTRY), loob DB2 andmebaasiga väärtused.
- "Täna" ja "homme" määrates DB2 konnektor genereeritud "Praegune kuupäev" ja "praegune kuupäev + 1 päeva-funktsioone (nt REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Loogika rakenduse DB2 konnektor päästik lugeda, muuta või kustutada andmete ##
Loogika rakenduse päästik küsitlus ja andmeid lugeda DB2 tabelist API küsitluse andmete kombineeritud toimingu abil saate määratleda. Näiteks saate lugeda ühte või rohkem uut kliendi tellimuse kirjet, naasevad loogika rakenduse kirjed. DB2 ühenduse paketi/rakenduse sätted peaks välja nägema järgmine:

    Rakenduse säte | Väärtus
--- | --- | ---
PollToCheckData | Valige COUNT (\*) NEWORDERS, kus TARNEKUUPÄEV on NULL:
PollToReadData | Valige \* NEWORDERS, kus TARNEKUUPÄEV on NULL värskenduse kaudu
PollToAlterData | <no value specified>


Samuti saate määratleda loogika rakenduse päästik küsitlus, lugeda ja muuta andmete DB2 tabeli API küsitluse andmete kombineeritud toimingu abil. Näiteks saate lugeda ühte või rohkem uute kliendi tellimuse kirjete värskendamine rea väärtused, naasevad loogika rakendus (Värskenda) enne valitud kirjed. DB2 ühenduse paketi/rakenduse sätted peaks välja nägema järgmine:

    Rakenduse säte | Väärtus
--- | --- | ---
PollToCheckData | Valige COUNT (\*) NEWORDERS, kus TARNEKUUPÄEV on NULL:
PollToReadData | Valige \* NEWORDERS, kus TARNEKUUPÄEV on NULL värskenduse kaudu
PollToAlterData | VÄRSKENDUSE NEWORDERS määramine TARNEKUUPÄEV = praeguse kuupäeva kui praegune, &lt;KURSOR&gt;


Lisaks saate määratleda loogika rakenduse päästik küsitlus, lugeda ja API küsitluse andmete kombineeritud toimingu abil DB2 tabeli andmete eemaldamine. Näiteks saate lugeda üks või mitu uut kliendi tellimuse kirjet, Kustuta read, naasevad loogika rakendus (Kustuta) enne valitud kirjed. DB2 ühenduse paketi/rakenduse sätted peaks välja nägema järgmine:

    Rakenduse säte | Väärtus
--- | --- | ---
PollToCheckData | Valige COUNT (\*) NEWORDERS, kus TARNEKUUPÄEV on NULL:
PollToReadData | Valige \* NEWORDERS, kus TARNEKUUPÄEV on NULL värskenduse kaudu
PollToAlterData | Kustuta NEWORDERS kui praegune, &lt;KURSOR&gt;

Selles näites loogika rakendus on küsitlus, lugemine, värskendamine ja seejärel uuesti lugeda DB2 tabeli andmed.

1. Azure'i startboard, valige **+** (plussmärk) **Web + Mobile**, ja seejärel **loogika rakendus**.
2. Sisestage nimi (nt "ShipOrdersDb2"), rakenduse teenuse kavandamine, muud atribuudid ja seejärel nuppu **Loo**.
3. Azure'i startboard, valige loogika äsja loodud rakenduse, **sätted**, ja seejärel **päästikute ja toimingud**.
4. Päästikute ja toimingute tera, valige **loomine algusest peale** loogika rakendusest mallid.
5. Paanil API rakendused valige **DB2 konnektor**, sageduse ja intervall, ja seejärel **märkige ruut**määramine.
6. Paanil API rakendused valige **DB2 konnektor**, laiendage toimingute loendist valimiseks **Valige NEWORDERS**.
7. Valige **märge** salvestada selle toimingu sätted ja seejärel valige **Salvesta**. Sätete peaks välja nägema järgmine:  
![][10]  
8. **Päästikute ja toimingute** blade sulgemiseks klõpsake nuppu ja klõpsake **sätted** tera sulgemiseks.
9. Klõpsake loendis **kõik töötab** **toimingute**all esimese loendis üksust (kõige uuemad Käivita).
10. Klõpsake **toimingu** üksust labale **loogika rakenduse käivitamine** .
11. Klõpsake **loogika rakenduse** tiiviknoad **VÄLJUNDEID LINK**. Väljundid peaks välja nägema järgmine:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Loogika rakenduse DB2 konnektor toimingu andmete eemaldamine ##
Saate määratleda loogiline rakenduse toiming andmete eemaldamiseks DB2 tabeli kasutamine postituse või API Kustuta üksus OData toiming. Näiteks saate lisada uue kirje kliendi tellimuse töötlemine SQL-i lisamine lause määratletud identiteedi veeruga tabel vastu, identiteedi väärtust või loogika rakendus (valige ORDID lõplik TABELIST (lisamine RAKENDUSSE NWIND mõjutatud ridade. NEWORDERS (CUSTID, TARNENIMI KIRJED, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) VÄÄRTUSI (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Loogika rakenduse kasutamisega DB2 andmete eemaldamiseks loomine ##
Saate luua uue loogika rakenduse: Azure'i turuplatsi sees ja seejärel kasutage DB2 konnektor toimingu eemaldamiseks klientide tellimusi. Näiteks saate kasutada Kustutustoiming DB2 konnektor tingimusvormingu töödelda SQL-i kustutamine lause (kustutamine kaudu NEWORDERS kus ORDID > = 10000).

1. Azure'i **käivitamine** tahvli jaoturi menüüs nuppu **+** (plussmärk), klõpsake **Web + Mobile**, ja seejärel nuppu **loogika rakendus**. 
2. **Loogika loomine rakenduse** tera, tippige **nimi**, näiteks **RemoveOrdersDb2**.
3. Valige või Määratlege väärtused ka muud sätted (nt teenusleping, ressursirühm).
4. Sätete peaks välja nägema järgmine. Klõpsake nuppu **Loo**.  
![][12]  
5. Klõpsake **sätete** labale **päästikute ja toimingud**.
6. **Päästikute ja toimingute** labale **loogika rakenduste Mallid** loendis, klõpsake **loomine algusest peale**.
7. **Päästikute ja toimingute** labale paanil **API rakenduste** rühmas ressursi nuppu **Korduvus**.
8. Konstruktsioonipinnalt loogika rakenduse nuppu **Korduvus** üksus, **sageduse** ja **intervall**, näiteks **päeva** ja **1**, ja **märkige ruut** Korduvus üksuse sätete salvestamiseks klõpsake.
9. Klõpsake **päästikute ja toimingute** labale paanil **API rakenduste** rühmas ressursi **DB2 konnektor**.
10. Konstruktsioonipinnalt loogika rakenduse klõpsake üksust **DB2 konnektor** toimingu, klõpsake kolmikpunkti (…****) toimingute loendi laiendamiseks ja klõpsake **tingimuslik kustutada N**.
11. Tippige **avaldis, mis tuvastab kirjete alamhulga** **ORDID ge 10000** üksusel DB2 konnektor toimingu.
12. Klõpsake Toimingusätete salvestamiseks **märkige ruut** ja klõpsake siis nuppu **Salvesta**. Sätete peaks välja nägema järgmine:  
![][13]  
13. **Päästikute ja toimingute** blade sulgemiseks klõpsake nuppu ja klõpsake **sätted** tera sulgemiseks.
14. Klõpsake loendis **kõik töötab** **toimingute**all esimese loendis üksust (kõige uuemad Käivita).
15. Klõpsake **toimingu** üksust labale **loogika rakenduse käivitamine** .
16. Klõpsake **loogika rakenduse** tiiviknoad **VÄLJUNDEID LINK**. Väljundid peaks välja nägema järgmine:  
![][14]

**Märkus:** Loogika rakenduse designer kärbitakse tabelinimed. Näiteks kärbitakse **tingimuslik**kustutamiseks N **tingimuslik NEWORDERS kustutamine** toimingut.


> [AZURE.TIP] Järgmine SQL-lauseid abil saate luua näidistabeli ja salvestatud toimingute. 

Saate luua NEWORDERS näidistabeli abil DB2 SQL-i DDL järgmistest:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



Saate luua valimi SPOERID salvestatud protseduur abil DB2 DDL järgmine tekst:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Hübriidkonfiguratsioonis (valikuline)

> [AZURE.NOTE] See on vajalik ainult siis, kui kasutate tulemüüri taha DB2 konnektor kohapealse.

Rakenduse teenus kasutab hübriid Configuration Manager teie asutusesisese süsteemi turvaliselt ühendada. Kui konnektor kasutab kohapealse IBM DB2 Server for Windows, on vaja ühenduse hübriid ülemus.

Vaadake [hübriidjuurutuse ühendust halduriga](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Lisavõimalused oma konnektor
Nüüd, kui konnektor on loodud, saate selle lisada töövoos loogika rakenduse kasutamine. Vt [mis on loogika rakendused?](app-service-logic-what-are-logic-apps.md).

Looge API rakenduste abil REST API-d. Vt [konnektorid ja API rakenduste viide](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Saate vaadata ka jõudluse statistika ja juhtelemendi turvalisus konnektor. Vaadata, [hallata ja jälgida oma sisseehitatud API rakendused ja konnektorid](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png

