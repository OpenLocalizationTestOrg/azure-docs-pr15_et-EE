<properties 
   pageTitle="SQL-i andmebaasi katastroofi taastamine harjutused | Microsoft Azure'i" 
   description="Lugege juhiseid ja katastroofi taastamine harjutused, mis aitavad teha Azure'i SQL-andmebaasi kasutamise head tavad hoida oma ülesanne kriitiliste tõrgete ja katkestuste olles ärirakenduste." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Läbimiseks Drill katastroofi taastamine

Soovitatav on rakenduse valmisoleku taastamine töövoo kinnitamine toimub perioodiliselt. Rakenduse käitumine ja mõju andmekao ja/või häireid selle Tõrkesiirde hõlmab engineering hea tava on. Samuti on osana business järjepidevuse sertifikaat, enamik tööstusstandarditele nõue.

Läbimiseks katastroofi taastamine drill koosneb järgmistest elementidest:

- Simuleerib andmete taseme katkestuste
- Taastamine 
- Kinnitage rakenduse terviklus postituse taastamine

Sõltuvalt sellest, kuidas [teie taotlus järjepidevuse loodud](sql-database-business-continuity.md)töövoo käivitada drill võivad erineda. Allpool kirjeldame toodud heade tavade läbi katastroofi taastamine drill Azure'i SQL-andmebaasi kontekstis. 

##<a name="geo-restore"></a>Geo-taastamine

Läbi katastroofi taastamine drill võimaliku andmekao vältimiseks soovitame, et kasutate tootmiskeskkonda eksemplari loomise ja selle abil, et rakenduse Tõrkesiirde töövoo drill läbimiseks.
 
####<a name="outage-simulation"></a>Katkestuste simulatsioon

Funktsiooni katkestuste jäljendamiseks saate kustutada või ümber nimetada allika andmebaasi. See võib põhjustada rakenduse ühenduvuse tõrge. 

####<a name="recovery"></a>Taastamine

- Tegema Geo taastamine andmebaasi eri server, mis on kirjeldatud [allpool](sql-database-disaster-recovery.md). 
- Rakenduse konfigureerimise taastatud andmebaasi ühenduse loomine ja [konfigureerimine andmebaasi taastamine pärast](sql-database-disaster-recovery.md) juhend taastamise lõpuleviimiseks järgige muuta.

####<a name="validation"></a>Valideerimine

- Täitke drill kinnitatava rakenduse terviklus postituse taaste (ühendusstringi, sisselogimise, põhifunktsioonide testimine või muu valideerimised osa standardsed rakenduse signoffs toimingute).

##<a name="geo-replication"></a>Geo-Dispersioonanalüüs

Andmebaasi, mis on kaitstud Geo-kopeerimise abil drill ülesanne hõlmab kavandatud Tõrkesiirde teise andmebaasi. Kavandatud Tõrkesiirde tagab, et esmast ja teisene andmebaaside sünkroonis eksportimisel jäävad rollid on sisse lülitatud. Erinevalt planeerimata Tõrkesiirde, seda toimingut pole tulemuseks andmete kaotsimineku nii drill saab teha tootmiskeskkonda. 

####<a name="outage-simulation"></a>Katkestuste simulatsioon

Simuleerida katkestuste, saate keelata veebirakenduse või virtuaalne seade on ühendatud andmebaasi. See põhjustada ühenduvuse tõrkeid web kliendid.

####<a name="recovery"></a>Taastamine

- Veenduge, et rakenduse konfigureerimise DR piirkonna osutab endise teisese, mis muutub täielikult juurdepääsetav uus primaarne. 
- [Plaanitud Tõrkesiirde](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) teise andmebaasi uus primaarne teha
- Juhend [konfigureerimine andmebaasi taastamine pärast](sql-database-disaster-recovery.md) taastamis lõpuleviimiseks järgige.

####<a name="validation"></a>Valideerimine

- Täitke drill kinnitatava rakenduse terviklus postituse taaste (ühendusstringi, sisselogimise, põhifunktsioonide testimine või muu valideerimised osa standardsed rakenduse signoffs toiminguid).


## <a name="next-steps"></a>Järgmised sammud

- Äriprotsessid järjepidevuse kohta leiate teemast [järjepidevuse stsenaariumid](sql-database-business-continuity.md)
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
