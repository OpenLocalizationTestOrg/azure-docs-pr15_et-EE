## <a name="repeatability-during-copy"></a>Korratavuse ajal kopeerimine

Kui kopeerimine Azure SQL-i ja SQL serveri andmed muudest andmed talletatakse üks peab korratavuse pidage meeles, et vältida ootamatuid tulemusi. 

Andmete kopeerimisel Azure SQL-i ja SQL serveri andmebaasi Kopeeri tegevuse kuvatakse vaikimisi vaikimisi lisa andmehulga valamu tabelisse. Näiteks kui kopeerimine CSV (komaeraldusega väärtused) faili sisaldava andmeallika Azure SQL-i ja SQL serveri andmebaasi kahe kirje andmeid, see on tabel näeb:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Oletame, et olete leitud tõrgete lähtefail ja värskendatud toru allapoole arvu 2 lähtefaili 4. Kui käivitate andmete sektorit uuesti perioodi, leiate Azure'i SQL-i ja SQL serveri andmebaasi lisatud kaks uut kirjet. Kuvatakse allpool eeldab, et ükski tabelis veerud on esmase võtme piirangu.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Selle vältimiseks peate määrata UPSERT semantika tehtavate ühte selle all 2 menetlustele allpool toodud.

> [AZURE.NOTE] Mõnda sektorit saab seda uuesti käivitada automaatselt Azure'i andmed Factory määratud proovi uuesti poliitika kohta.

### <a name="mechanism-1"></a>Süsteem 1

Saate kasutada **sqlWriterCleanupScript** atribuudi esimese toimingu tegemiseks Kettapuhastus tükk käitamisel. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Skripti puhastus toimub esimese käigus antud sektorit, kuhu soovite andmed kustutada sektorit vastav SQL tabelist koopia. Seejärel lisab tegevuse andmed tabelisse SQL-i. 

Kui sektorit on nüüd uuesti käivitada, siis leiate kogus on värskendatud nimega soovitud.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Oletame, et lameseib kirje eemaldatakse algse csv. Käivitage uuesti sektorit saadav järgmine tulem: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Midagi uut tuli teha. Kopeeri tegevuse parandusfunktsiooni Kettapuhastus skripti kustutamiseks sektorit vastavaid andmeid. Seejärel seda lugeda sisend csv (mis siis sisaldas ainult 1 kirje) ja see lisatakse tabelisse. 

### <a name="mechanism-2"></a>Süsteem 2
> [AZURE.IMPORTANT] Sel ajal sliceIdentifierColumnName jaoks SQL Azure'i andmebaas pole toetatud. 

Mõne muu korratavuse saavutamiseks on spetsiaalne veeru (**sliceIdentifierColumnName**), millel target tabeli. Selles veerus sooviksite kasutada Azure'i andmed Factory tagamaks, et lähte-kui ka püsida sünkroonitud. Seda moodust töötab paindlikkust muutmise või määratlemine sihtkoha SQL tabeli skeemi korral. 

Selles veerus kasutataks Azure'i andmed Factory korratavuse huvides ja protsessi Azure'i andmed Factory ei tee skeemi muudatused tabelisse. Võimalus kasutada seda moodust:

1.  Määratleda sihtkoha SQL tabeli veeru tüüpi kahendarvu (32). Peaks olema selle veeru piirangud. Vaatame nimi See veerg nimega "ColumnForADFuseOnly" selle näite puhul.
2.  Kasutage seda Kopeeri tegevuse järgmiselt:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure'i andmed Factory asustada selle veeru ühe oma vajadust tagada, et lähte-kui ka püsida sünkroonitud. Selle veeru väärtusi ei tohi kasutada muus kontekstis kasutaja. 

Sarnane süsteem 1 Kopeeri tegevuse automaatselt esmalt puhastamiseks antud sektorit sihtkohta SQL tabeli andmed ja seejärel käivitage Kopeeri tegevuse tavaliselt lisamiseks andmete allikas sihtkoht sektorit. 
