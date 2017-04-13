Järgmises tabelis on loetletud piirangud, mis on seotud muu teenuse astme (S1, S2, S3, F1). Täpsemat teavet iga taseme iga *Ühiku* maksumus [Asjade jaoturi hinnad](https://azure.microsoft.com/pricing/details/iot-hub/).

| Ressurss | S1 Standard | S2 Standard | S3 Standard | Tasuta F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Sõnumite/päev | 400 000 | 6 000 000   | 300000000 | 8 000   |
| Maksimaalsed ühikud | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Kui eeldate, et üle 200 tk S1 või S2 või S3 taseme jaoturi abil, võtke ühendust Microsofti toega.

Järgmises tabelis on loetletud piirangud, mis rakenduvad asjade jaoturi ressursse:

| Ressurss | Limiit |
| -------- | ----- |
| Maksimum makstud asjade jaoturi Azure tellimuse kohta | 10 |
| Suurim lubatud tasuta asjade jaoturi Azure tellimuse kohta | 1 |
| Seadme identiteedid arvu ülempiir<br/>  tagastatakse üks kõne | 1000 |
| Asjade jaoturi sõnumi maksimaalse säilituspoliitika seadme pilve sõnumite jaoks | 7 päeva |
| Seadme pilve sõnumi maksimaalne maht | 256 KB |
| Seadme pilve partii maksimaalne maht | 256 KB |
| Seadme pilve partii maksimaalne sõnumite | 500 |
| Pilveteenuse-seadme sõnumi maksimaalne maht | 64 KB |
| Suurim lubatud TTL cloud-seadme sõnumite jaoks | 2 päeva |
| Pilveteenuse-seadme maksimaalne arv <br/> sõnumite | 100 |
| Tagasiside sõnumite maksimaalne arv <br/> pilveteenuse-seadme sõnumi vastuseks | 100 |
| Suurim lubatud TTL tagasiside sõnumite jaoks <br/> vastuse sõnumile pilveteenuste-seade | 2 päeva |

> [AZURE.NOTE] Kui teil on vaja rohkem kui 10 makstud asjade jaoturi Azure tellimuse, võtke ühendust Microsofti toega.

Asjade jaoturi teenuse ahendab taotlusi, kui järgmised piirmäärasid on ületatud:

| Ahendamise | Väärtuse keskuse kohta. |
| -------- | ------------- |
| Identiteedi registri tehtavad toimingud <br/> (loomine, tuua, loendi, värskendada, kustutada) <br/> üksikute või hulgikopeerimistoimingute import/eksport | ühiku/5000/min (S3) jaoks <br/> ühiku/100/min (S1 ja S2) jaoks. |
| Seadme ühendused | ühiku/6000/sec (S3) jaoks 120/sec/ühik (S2), ühiku/12/sec (S1) jaoks. <br/>Minimaalselt 100 sekundis. |
| Seadme pilve saadab | ühiku/6000/sec (S3) jaoks 120/sec/ühik (S2), ühiku/12/sec (S1) jaoks. <br/>Minimaalselt 100 sekundis. |
| Pilveteenuse-seade saadab | ühiku/5000/min (jaoks S3) 100 min/ühiku (S1 ja S2) jaoks. |
| Pilveteenuse-seade saab | ühiku/50000/min (S3) jaoks 1000 min/ühiku (S1 ja S2) jaoks. |
| Faili üleslaadimine toiminguid | 5000 faili üles laadida ja ühiku/teatised/min (jaoks S3), 100 faili üleslaadimine teatised min/ühiku (S1 ja S2). <br/> 10000 SAS URI-d võib olla korraga Azure Storage konto jaoks.<br/> 10 SAS URI-d seade võib olla välja korraga. |
