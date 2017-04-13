<properties 
    pageTitle="Factory andmefunktsioonid ja süsteemi muutujad | Microsoft Azure'i" 
    description="Azure'i andmed Factory funktsioonid ja süsteemi muutujate loend" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure'i andmed Factory - funktsioonide ja süsteemi muutujad
Selles artiklis kirjeldatakse funktsioonide ja Azure andmete Factory ei toeta muutujat.
  
## <a name="data-factory-system-variables"></a>Andmete Factory süsteemi muutujad

Muutuja nimi | Kirjeldus | Objekti ulatus | JSON ulatuse ja kasutamise juhtudel
------------- | ----------- | ------------ | ------------------------
WindowStart | Praeguse töö aken ajaintervalli algus | tegevus | <ol><li>Määrake valiku andmepäringuid. Lugege konnektor artikleid viidatud [Andmete liikumine tegevuste](data-factory-data-movement-activities.md) artiklis.</li><li>Parameetrite edastamine taru script (proovi eespool näidatud).</li>
WindowEnd | Ajavahemik praeguse tegevuste käivitamine akna lõppu | tegevus | sama, mis eelmine
SliceStart | Koostatud andmete sektorit ajaintervalli algus | tegevus<br/>andmekomplekti | <ol><li>Määrake töötamisel [Azure'i bloobimälu](data-factory-azure-blob-connector.md) [failisüsteemis andmekomplektide](data-factory-onprem-file-system-connector.md)dünaamiline kausta tee ja faili nimi.</li><li>Määrake Sisestuskeel sõltuvused andmete factory tegevuse sisendina saidikogumi funktsioonid.</li></ol>
SliceEnd | Praeguse andmete sektorit koostatud ajaintervalli lõppu | tegevus<br/>andmekomplekti | sama mis eelmine. 

> [AZURE.NOTE] Praegu andmete factory nõuab, et märgitud täpselt Ajasta vastavad määratud väljundi andmekomplekti kättesaadavus ajakava. See tähendab, et WindowStart, WindowEnd ja SliceStart ja SliceEnd alati vastendada sama aja jooksul ja ühe väljundi sektorit.
 
## <a name="data-factory-functions"></a>Andmete Factory funktsioonid 

Saate kasutada funktsioone factory andmeid koos eespool nimetatud süsteemi muutujad järgmistel eesmärkidel:

1.  Valiku andmepäringuid määramine (vt konnektor artikleid viidatud [Andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel.

    Kasutada andmete factory funktsiooni süntaks: ** $$ ** valiku andmepäringuid ja muud atribuudid ja andmekomplektide.  
2. Mis määrab Sisestuskeel sõltuvused andmete factory funktsioonidega tegevuste sisendit saidikogumi (vt ülal valimi).

    $$ ei ole vaja määratlemiseks Sisestuskeel sõltuvus avaldised.   

Järgmises näites on **sqlReaderQuery** atribuudi JSON-failis on määratud **Text.Format** funktsiooni tagastatud väärtus. See näide kasutab ka süsteemi muutuja nimega **WindowStart**, mis tähistab aken tegevuse algusaeg.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Funktsioonid

Järgmises tabelis on loetletud Azure'i andmed Factory kasutusvõimalusi:

Kategooria | Funktsioon | Parameetrid | Kirjeldus
-------- | -------- | ---------- | ----------- 
Aeg | AddHours(X,Y) | X kuupäev ja kellaaeg <br/><br/>Y: int | Lisab Y tunni aja X. <br/><br/>Näide: 9/5/2013 12:00:00 PL + 2 tundi = 9/5/2013 2:00:00 PM
Aeg | AddMinutes(X,Y) | X kuupäev ja kellaaeg <br/><br/>Y: int | Lisab X Y minutit.<br/><br/>Näide: 9/15/2013 12:00:00 PL + 15 minutit = 9/15/2013 12:15:00 PL
Aeg | StartOfHour(X) | X kuupäev ja kellaaeg | Saab tund komponent x tähistab tunni algusaeg. <br/><br/>Näide: StartOfHour 9/15/2013 05:10:23 PM on 9/15/2013 05:00:00 PM
Kuupäev | AddDays(X,Y) | X kuupäev ja kellaaeg<br/><br/>Y: int | Lisab Y päeva X.<br/><br/>Näide: 9/15/2013 12:00:00 PL + 2 päeva = 9/17/2013 12:00:00 PL
Kuupäev | AddMonths(X,Y) | X kuupäev ja kellaaeg<br/><br/>Y: int | Lisab Y kuud X.<br/><br/>Näide: 9/15/2013 12:00:00 PL + 1 kuu = 10/15/2013 12:00:00 PL 
Kuupäev | AddQuarters(X,Y) | X kuupäev ja kellaaeg <br/><br/>Y: int | Lisab Y * x 3 kuud.<br/><br/>Näide: 9/15/2013 12:00:00 PL + 1 kvartali = 12/15/2013 12:00:00 PL
Kuupäev | AddWeeks(X,Y) | X kuupäev ja kellaaeg<br/><br/>Y: int | Lisab Y * x 7 päeva<br/><br/>Näide: 9/15/2013 12:00:00 PL + 1 nädal = 9/22/2013 12:00:00 PL
Kuupäev | AddYears(X,Y) | X kuupäev ja kellaaeg<br/><br/>Y: int | Lisab Y aastate X.<br/><br/>Näide: 9/15/2013 12:00:00 PL + 1 aasta = 9/15/2014 12:00:00 PL
Kuupäev | Day(X) | X kuupäev ja kellaaeg | Saab päeva komponent x.<br/><br/>Näide: Päeva 9/15/2013 12:00:00 PL on 9. 
Kuupäev | DayOfWeek(X) | X kuupäev ja kellaaeg | Saab nädala osa X päeva.<br/><br/>Näide: DayOfWeek 9/15/2013 12:00:00 PL on pühapäev.
Kuupäev | DayOfYear(X) | X kuupäev ja kellaaeg | Saab päeva aastas aasta osa X tähistab.<br/><br/>Näited:<br/>12/1/2015: päev 335 2015<br/>2015/12/31: 365 päeva 2015<br/>12/31/2016: päeva 366 2016 (aastal)
Kuupäev | DaysInMonth(X) | X kuupäev ja kellaaeg | Saab kuu komponent parameetri X tähistab kuu päeva.<br/><br/>Näide: 9/15/2013 DaysInMonth on 30, kuna mai kuu on 30 päeva.
Kuupäev | EndOfDay(X) | X kuupäev ja kellaaeg | Saab kuupäeva / kellaaja, mis tähistab day (päev komponent) x lõppu.<br/><br/>Näide: EndOfDay 9/15/2013 05:10:23 PM on 9/15/2013 11:59:59 PM.
Kuupäev | EndOfMonth(X) | X kuupäev ja kellaaeg | Saab kuu osa parameeter X tähistab kuu lõpu. <br/><br/>Näide: EndOfMonth 9/15/2013 05:10:23 PM on 9/30/2013 11:59:59 PM (kuupäev kellaaeg, mis tähistab mai kuu)
Kuupäev | StartOfDay(X) | X kuupäev ja kellaaeg | Saab päeva komponent parameetri X tähistab päeva algust.<br/><br/>Näide: StartOfDay 9/15/2013 05:10:23 PM on 9/15/2013 12:00:00 AM.
Kuupäev ja kellaaeg | From(X) | X string | Sõelub stringi X kuupäeva ja kellaaja.
Kuupäev ja kellaaeg | Ticks(X) | X kuupäev ja kellaaeg | Saab puugid atribuudi parameetri X. Üks rist võrdub 100 nanosekundit. Selle atribuudi väärtust tähistab arvu, mis on 12:00:00 keskööst möödunud, 1 Jaanuar 0001. 
Teksti | Format(X) | X stringi muutuja | Vormingud soovitud tekst.

#### <a name="textformat-example"></a>Text.Format näide

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Vt [kohandatud kuupäeva ja kellaaja vormingustringi](https://msdn.microsoft.com/library/8kb3ddd4.aspx) teema, mis kirjeldab erinevate vormindusvõimaluste abil saate (nt: t vs aaaa). 

> [AZURE.NOTE] Kui kasutate funktsiooni pesastamine teise funktsiooni, pole vaja kasutada **$$** eesliite sisemise funktsiooni. Näide: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' ja RowKey ge \\"{0:yyyy-MM-dd HH:mm:ss}\\", Time.AddHours (SliceStart -6)). Selles näites teate, et **$$** eesliite **Time.AddHours** funktsiooni ei kasutata. 
  

