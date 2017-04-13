<properties
 pageTitle="Asjade jaoturi HA ja DR | Microsoft Azure'i"
 description="Kirjeldatakse funktsioone, mis aitavad luua väga kättesaadav asjade lahenduste toime funktsioonid."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>Asjade jaoturi kõrge kättesaadavus ja Avariijärgne taaste

Azure'i teenuse pakub asjade jaoturi abil koondamised tasemel Azure piirkond ilma lisatööd nõutud lahendus kõrge kättesaadavus (HA). Lisaks Azure pakub mitmeid funktsioone, mis aitavad luua lahendusi (DR) funktsioonid või rist – saadaval, kui nõutav. Peate kujundamine ja valmistatakse oma ära need DR funktsioone, kui soovite sisestada globaalne, rist-piirkond kõrge-saadavus seadmete või kasutajate jaoks. [Azure'i Business järjepidevuse tehnilised juhendid](../resiliency/resiliency-technical-guidance.md) artiklis kirjeldatakse sisseehitatud funktsioonid Azure järjepidevuse ja DR. [Katastroofiabi ja kõrge-saadavus Azure rakenduste][] paberi annab arhitektuur juhiseid Azure rakenduste HA ja DR strateegiad.

## <a name="azure-iot-hub-dr"></a>Azure'i asjade jaoturi DR
Lisaks siseseid piirkonna HA, rakendatakse asjade jaoturi Tõrkesiirde menetlustele avariitaastet, mis nõuavad kasutaja sekkumine. Asjade jaoturi DR omas käivitatakse ning taastamise aeg eesmärk (RTO), 2-26 tundi ja järgmised taastamine punkti eesmärgid (RPOs).

| Funktsioonid | RPO |
| ------------- | --- |
| Teenuse kättesaadavus registri ja suhtlus | CName võimalik kahju |
| Identiteedi andmete seadme identiteedi registri | 0-5 minutit andmekao |
| Seadme pilve sõnumid | Lähevad kaotsi kõik lugemata sõnumid |
| Toimingute sõnumite jälgimine | Lähevad kaotsi kõik lugemata sõnumid |
| Pilveteenuse-seadme sõnumid | 0-5 minutit andmekao |
| Pilveteenuse-seadme tagasiside järjekord | Lähevad kaotsi kõik lugemata sõnumid |

## <a name="regional-failover-with-iot-hub"></a>Piirkondliku Tõrkesiirde asjade jaoturi abil

Täieliku töötlemise juurutamise topoloogiatest asjade Solutions on käesoleva artikli, kuid kõrge-saadavus ja peame *piirkondliku Tõrkesiirde* juurutamise mudeli avariitaastet väljapoole.

Mudeli piirkondliku Tõrkesiirde lahenduse tagasi lõpuks töötab peamiselt ühe andmekeskuse asukoha, kuid tagasi end ja täiendavate asjade jaoturi juurutatud teise andmekeskuse asukoha Tõrkesiirde eesmärgil asjade jaoturi esmane andmekeskuses kannatab mõne katkestuste või võrguühendus seadme esmane andmekeskuse kuidagi katkeb. Seadmete kasutamine on sekundaarne teenuse lõpp-punkti iga kord, kui esmane lüüsi ei pääse. Rist-piirkond Tõrkesiirde võimalusega lahenduse-saadavus parandada lisaks ühe piirkonna kõrge kättesaadavus.

Kõrge, rakendada piirkondliku Tõrkesiirde mudeli asjade keskuses on vaja järgmist.

* **Teisene asjade jaoturi- ja seadme marsruutimine loogika**: teenuse häirete teie peamine piirkond, seadmed peate käivitama ühenduse oma teisene piirkond. Enamik teenuste oleku arvestada arvestades on tavaline lahenduse administraatorite vaheline piirkond Tõrkesiirde protsessi käivitamiseks. Parim viis suhtlemiseks uue lõpp-punkti ja seadmed, säilitades kontrolli protsessi, on neid regulaarselt kontrollida *portjeeteenust praeguse aktiivse lõpp-punkti* . Concierge-teenus võib olla lihtsa veebirakenduse, mis on kopeeritud ja hoida kättesaadav DNS-ümbersuunamise meetoditega (nt [Azure liikluse][]halduris).
* **Identiteedi registri dispersioonanalüüs** – selleks, et neid saab kasutada, teisene asjade, jaoturi peab sisaldama kõik seadme identiteedid, mida saab ühendada lahendus. Lahendus peaks varukoopiaid geo kopeeritud seadme identiteedid ja nende üles teisene asjade jaoturi enne vahetamist aktiivsete lõpp-punkti seadmete jaoks. Seadme identiteedi ekspordi funktsionaalsust asjade jaoturi on väga kasulik selles kontekstis. Lisateabe saamiseks vt [Asjade jaoturi arendaja juhend - identiteedi registri][].
* **Ühendamise loogika** - vabanemise esmane piirkond uuesti, kõik riigi- ja andmeid, mis on loodud saidi teise tuleb migreerida tagasi esmane alale. See peamiselt seotud seadme identiteedid ja rakenduse metaandmete, mis tuleb ühendada esmane asjade jaotur ja muude rakenduste kohased poed esmane piirkonna. Selles etapis tuleb lihtsustamiseks on soovitatav tavaliselt, et kasutate idempotent toimingud. See vähendab küljel efektid, mitte üksnes lõpliku ühtsete jaotuse sündmusi, vaid ka duplikaatide või-order kohaletoimetamise sündmused. Lisaks rakenduse loogika kavandada luba võimalikud vastuolusid või "veidi" välja kuupäeva-riik. See tõttu täiendavad aega kulub süsteemi "heal" põhineb taastamine punkti (RPO).

## <a name="next-steps"></a>Järgmised sammud

Järgige neid linke leiate Azure'i asjade jaoturi Lisateavet:

- [Alustamine asjade jaoturi (õpetuse)][lnk-get-started]
- [Mis on Azure asjade jaoturi?][]

[Katastroofiabi ja Azure rakenduste kõrge kättesaadavus]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure'i liikluse haldur]: https://azure.microsoft.com/documentation/services/traffic-manager/
[Asjade jaoturi arendaja juhend - identiteedi registri]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Mis on Azure asjade jaoturi?]: iot-hub-what-is-iot-hub.md
