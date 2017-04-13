
Vaikimisi Mobile'i rakendus taustväärtus API-d saab kasutada anonüümselt. Järgmiseks peate juurdepääsu piiramiseks ainult autenditud kliendid.  

+ **Node.js kirjutamata (portaali) kaudu** :  
    
    Mobile'i rakenduses **sätted**, klõpsake **Lihtne tabelid** ja valige oma tabel. Klõpsake nuppu **Muuda õigusi**, valige **ainult autenditud juurdepääsu** kõigi õiguste ja seejärel klõpsake nuppu **Salvesta**. 

+ **.Net-i kirjutamata (C#)**:  

    Liikuge Projectis serveri **kontrollerid** > **TodoItemController.cs**. Lisage soovitud `[Authorize]` atribuut **TodoItemController** ainekursust, järgmiselt. Juurdepääsu piiramiseks ainult teatud meetodid, saate rakendada selle atribuudi vaid, et nende meetodite asemel ainekursust. Video uuesti avaldamiseks server project.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js kirjutamata (Node.js koodi) kaudu** :  
    
    Nõuab autentimist tabeli juurdepääsu, lisage järgmine rida Node.js serveri skripti:


        table.access = 'authenticated';

    Lisateabe saamiseks vaadake [kohta: autentimine nõua juurdepääsuks tabelite](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Kiirjuhend Koodiprojekti alla laadida saidi kohta leiate teavet teemast [kohta: allalaadimine Node.js kirjutamata Kiirjuhend Koodiprojekti abil Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

