<properties
    pageTitle="Azure SQL-i andmebaasi JSON funktsioonid | Microsoft Azure'i"
    description="SQL Azure'i andmebaas võimaldab sõeluda, päringu ja andmete vormindamine JavaScript Object märke (JSON) esituses."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Azure'i SQL-andmebaasi JSON funktsioonide kasutamise alustamine

SQL Azure'i andmebaas võimaldab sõeluda ja päringu andmed, mida tähistab JavaScripti Objektiesituse [(JSON)](http://www.json.org/) vormingus relatsiooniliste andmete eksportimine JSON tekstina.

JSON on kasutatud andmevahetus tänapäevane web ja mobiilirakenduste populaarsed andmete vorming. JSON kasutatakse ka logifailid või NoSQL andmebaasidest [Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/)poolstruktuur andmete talletamiseks. Mitme ülejäänud veebiteenused tulemeid JSON tekstina vormindatud andmete või aktsepteerimiseks JSON vormindatud. Kõige Azure teenused, nt [Azure otsing](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/)ja [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) on ülejäänud lõpp-punktid, mis tagastavad või tarbimine JSON.

Azure'i SQL-andmebaasi abil saate hõlpsalt JSON andmetega töötamine ja kaasaegse andmebaasi integreerida.

## <a name="overview"></a>Ülevaade

SQL Azure'i andmebaas sisaldab järgmisi funktsioone JSON andmetega töötamiseks:

![JSON-funktsioonid](./media/sql-database-json-features/image_1.png)

Kui teil on JSON teksti, saate andmete eraldamiseks JSON või JSON on vormindatud sisseehitatud funktsioonid [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)ja [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx)abil kontrollida. Funktsiooni [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) saate värskendada väärtus JSON teksti sees. Lisateavet täiustatud päringu ja analüüsi, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) funktsioon saab muuta JSON objektide massiivi ridade kogumit. SQL-päringu saab teostada tagastatud tulemuste. Samuti on [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) -klauslis, mille abil saate vormindada tekstina JSON oma relatsiooniline tabelites talletatavad andmed.

## <a name="formatting-relational-data-in-json-format"></a>JSON-vormingus relatsiooniliste andmete vormindamine
Kui teil on veebiteenuse leiab andmebaasist andmete kiht ja pakub vastuse JSON vormindada, või kliendipoolne JavaScripti raamistik või teegid, mis aktsepteerida andmete vormindatud JSON, saate oma andmebaasi sisu vormindada JSON otse rakenduses SQL-päringu. Teil pole enam on vormingud tulemuste Azure SQL andmebaasis JSON rakenduse koodi kirjutamiseks või kaasata mõned JSON sariväljaanne teegi teisendada tabeli päringutulemite ja seejärel serialiseerida objektide JSON-vormingus. Selle asemel saate vormindada SQL-päringu tulemid JSON Azure SQL-andmebaasis ja kasutage seda otse oma rakenduse FOR JSON-klausel.

Järgmises näites Sales.Customer tabeli ridade vormindatud JSON jaoks JSON-klausel abil:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

JSON tee klausel vormindab selle päringu tulemused JSON tekstina. Veeru nimed kasutatakse klahvid, samal ajal, kui lahtri väärtused on loodud JSON-väärtusi nii:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Tulemite hulk vormindatakse JSON massiiv, kus iga rida on vormindatud eraldi JSON-objekt.

TEE näitab, et saate kohandada JSON tulemi vorming, kasutades dot märkega veeru pseudonüümid. Järgmine päring muudab nime "Kliendinimi" Sisestage JSON vorming ja paigutab telefoni-ja faksinumber "Kontakt" all objekti:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Päringu väljund näeb välja umbes järgmine:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

Selles näites me tagasi asemel massiivi JSON üksikobjektiks, määrates suvandi [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) . Kui teate, et tagastatava päringu tulemusel üksikobjektiks, saate selle suvandi.

FOR JSON-klausel peamine väärtus, et see võimaldab keerukaid hierarhiaandmeid tulu andmebaasi vormindatud pesastatud JSON-objekte ega massiivides. Järgmises näites kujutatakse hulka kuuluvad pesastatud massiivi tellimuste kliendi tellimused:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Selle asemel, et saata eraldi päringute kliendiandmete saada ja seejärel tõmmata seotud tellimuste loend, saate kõik vajalikud andmed koos ühe päringu, nagu on näidatud järgmises valimi väljund:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>JSON-andmetega töötamine

Kui teil pole tingimata Liigendatud andmete, kui teil on keerukas sub objektid, massiivid või hierarhiliste andmete või kui teie andmestruktuurid muutuvad aja jooksul, JSON-vormingus saate mis tahes keerukate andmestruktuur tähistada.

JSON on teksti vormingut, mida saab kasutada nagu muude stringi tüüpi Azure SQL-andmebaasis. Saate saata või nimega standard NVARCHAR JSON andmete talletamiseks.

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

Selles näites kasutatakse JSON andmed on esitatud NVARCHAR(MAX) tüüp. JSON saate lisada selle tabeli või esitatud argumendina salvestatud protseduur standard Transact-SQL-i süntaksi kasutamine, nagu on näidatud järgmises näites:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

JSON-andmetega töötab ka mis tahes kliendipoolne keel või teeki, mis töötab koos stringi andmed Azure SQL-andmebaasis. JSON saab salvestada mis tahes tabeli, mis toetab NVARCHAR tüüp, nt mälu optimeeritud tabeliks või tabeli süsteemi versiooniga. JSON ei võta kasutusele mis tahes piirang kliendipoolne kood või andmebaasi kiht.

## <a name="querying-json-data"></a>JSON andmete päringud

Kui teil on vormindatud JSON Azure SQL-i tabelites talletatavad andmed, JSON funktsioonide abil saate kasutada neid andmeid SQL-päringu.

JSON funktsioonid, mis on saadaval Azure SQL-i andmebaasi võimaldavad teil kohelge andmete vormindatud JSON nagu mis tahes muud SQL-i andmetüüp. Saate hõlpsasti väärtused JSON teksti ekstraktida ja JSON andmeid kasutada mis tahes päringu.

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

Funktsiooni JSON_VALUE väärtus ekstraktib JSON teksti, mis on talletatud veerus andmed. See funktsioon kasutab JavaScripti nagu tee viidata JSON teksti eraldamiseks väärtuse. Ekstraktitud väärtus saab kasutada mis tahes osa SQL-päringu.

Funktsiooni JSON_QUERY on sarnane JSON_VALUE. Erinevalt JSON_VALUE, ekstraktib selle funktsiooni complex sub objekti näiteks massiivid või objekte, mis paigutatakse JSON teksti.

Funktsiooni JSON_MODIFY saate määrata väärtus tee JSON tekst, mida tuleks värskendada, kui ka uus väärtus, mis kirjutab vana. Nii saate hõlpsasti värskendada JSON teksti ilma reparsing struktuuri.

Kuna JSON talletatakse standardteksti, pole garantiid tekstiveergude talletatud väärtuste õigesti vormindatud. Teksti, mis on talletatud JSON veerg on vormindatud standard Azure'i SQL-andmebaasi sisse piiranguid ja ISJSON funktsiooni abil saate kontrollida.

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Teksti sisestamine on õigesti vormindatud JSON, tagastab funktsioon ISJSON väärtuse 1. See piirang kontrollib iga lisa või Värskenda JSON veeru, ei ole uue tekstiväärtuse moonutatud JSON.

## <a name="transforming-json-into-tabular-format"></a>Muutes JSON tabelivorming

Azure'i SQL-andmebaasi saate ka muuta JSON saidikogumid tabelina vormindamine ja laadi või päringu JSON andmeteks.

OPENJSON on tabeli väärtus funktsiooni, mis sõelub JSON teksti, otsib massiivi JSON objektide, seni, kuni massiivi elemendid ja tagastab ühe rea väljundi tulemi massiivi iga elemendi jaoks.

![JSON tabelina](./media/sql-database-json-features/image_2.png)

Ülaltoodud näites me ei saa määrata määrata asukoha JSON massiiv, mis tuleks avada ($. Tellimuste tee), milliseid veerge tuleks tagastada tulemi ja kust neid leida JSON väärtused, mis tagastatakse lahtritena.

Me saate muuta JSON massiivi soovitud @orders read komplekt analüüsida selle tulemite hulk, mis ridade lisamine tabelisse standard:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Saidikogumi tellimuste vormindatud JSON massiivi ja viisil salvestatud protseduur parameetrit saab sõeluda ja lisatakse tabelisse Orders.

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas rakenduse JSON integreerida, saate neid allikatest.

- [TechNeti ajaveebi](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [MSDN-i dokumendid](https://msdn.microsoft.com/library/dn921897.aspx)
- [Kanali 9 video](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Erinevate stsenaariumide JSON integreerimine rakenduse kohta lisateabe saamiseks vt selle [kanali 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) demos või leida stsenaarium, mis vastab teie kasutamine juhtumiga [JSON postitused](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).
