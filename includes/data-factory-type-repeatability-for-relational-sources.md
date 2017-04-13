## <a name="repeatability-during-copy"></a>Korratavuse ajal kopeerimine

Saatja ja adressaat relatsiooniline poed andmete kopeerimisel peate korratavuse pidage meeles, et vältida ootamatuid tulemusi. 

Mõnda sektorit saate uuesti automaatselt sisse Azure'i andmed Factory määratud proovi uuesti poliitika kohta. Soovitame teil seada uuesti poliitika, mis kaitsevad siirdamiseks ebaõnnestumist. Seega korratavuse on tähtis hoida andmete liikumise ajal. 

**Allikana:**

> [AZURE.NOTE] Järgmised näidet on Azure SQL-i jaoks, kuid need kehtivad mis tahes andmesalve, mis toetab ristküliku andmekomplektide. Võib-olla peate reguleerida lähte- ja **päringu** atribuudi **Tüüp** (nt: päringu asemel sqlReaderQuery) andmete talletamiseks.   

Tavaliselt, kui lugemist relatsiooniliste salvestab, tuleks soovite lugeda ainult sektorit vastavaid andmeid. Azure'i andmed Factory WindowStart ja WindowEnd muutujate abil oleks võimalus teha. Lugege muutujate ja funktsioonide Azure'i andmed Factory siin teemas [plaanimine ja täitmise](../articles/data-factory/data-factory-scheduling-and-execution.md) kohta. Näide: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Selle päringu loeb andmete MyTable, mis langeb sektorit kestus vahemikus. Käivita uuesti selle sektori alati ka tagab seda käitumist. 

Muudel juhtudel, võiksite lugeda terve tabeli (Oletame, et jaoks ainult teisaldamine üks kord) ja määrata selle sqlReaderQuery järgmiselt:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
