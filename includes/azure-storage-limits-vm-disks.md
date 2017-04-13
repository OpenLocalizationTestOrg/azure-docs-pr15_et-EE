Azure'i virtuaalarvuti toetab manustamise arvu andmete ketast. Optimaalse jõudluse tagamiseks soovite tugevalt kasutatud ketast, mis on seotud võimalike pidurdamise vältimiseks virtuaalse masina arv piirata. Kui kõik ketast pole on väga kasutatakse korraga, saate salvestusruumi konto toetavad suurem arv ketast.

- **Kindlad kontode puhul:** Kindlad konto on kokku päringu määr 20 000 IOPS. Kokku IOPS kõigis oma virtuaalse masina ketta jaotise standard salvestusruumi konto ei tohi olla suurem kui selle piirmäära.

    Saate arvutada umbes üks standardne salvestusruumi konto põhjal taotluse määr limiit ei toeta tugevalt kasutatud ketast arv. Näiteks põhilised taseme VM tugevalt kasutatud ketast maksimumarv on 66 (20 000/300 IOPS kohta ketas) kohta ning Standard taseme VM, on umbes 40 (500/20 000 IOPS kohta ketas), nagu on näidatud järgmises tabelis. 
 
- **Premium salvestusruumi konto:** Salvestusruumi premium konto on maksimaalne kokku läbilaskevõime 50 Gbit. Kokku läbilaskevõime kõigis oma VM ketast ei tohi olla suurem kui selle piirmäära.