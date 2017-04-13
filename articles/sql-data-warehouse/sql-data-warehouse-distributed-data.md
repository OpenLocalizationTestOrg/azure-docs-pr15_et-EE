<properties
   pageTitle="Jaotatud andmetega ja jaotatud andmetabeli suvandid oluliselt paralleelne töötlemine (MPP) süsteemidele SQL-i andmebaas ja paralleelselt andmebaas | Microsoft Azure'i"
   description="Siit saate teada, kuidas andmeid levitatakse oluliselt paralleelne töötlemine (MPP) ja suvandite levitamise tabelid SQL Azure'i andmebaas ja paralleelselt andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Jaotatud andmed ja jaotatud tabelite jaoks oluliselt paralleelne töötlemine (MPP)

Siit saate teada, kuidas kasutaja andmeid levitatakse SQL Azure'i andmebaas ja paralleelselt andmebaas, mis on Microsofti oluliselt paralleelne töötlemine (MPP) süsteemidega. Kujundamise jaotatud andmete tõhusaks kasutamiseks teie andmebaas aitab teil eelised MPP arhitektuur päringutöötluse saavutamiseks. Mõned andmebaasi kujunduse Valikud saate päringu jõudluse parandamiseks oluliselt mõjutada.  

>[AZURE.NOTE] Azure SQL-i andmebaas ja paralleelselt andmebaas kasutada sama oluliselt paralleelne töötlemine (MPP) design, kuid neil on ka erinevusi aluseks platvorm tõttu. SQL-i andmebaas on teenuse (PaaS), mis töötab Azure'i platvormi. Paralleelselt andmebaas töötab sisse Analytics platvormi süsteemi (apps), mis on kohapealse seade, kus töötab Windows Server.

## <a name="what-is-distributed-data"></a>Mis on jaotatud andmeid?

SQL-i andmebaas ja paralleelselt andmebaas, viitab jaotatud andmetega kasutaja andmeid, mis on talletatud mitmesse kogu MPP süsteemis. Kõik need asukohad funktsioone iseseisev ja töötlemise üksus, mis töötab päringute selle osa andmeid. Päringute töötab samal ajal kõrge päringu jõudluse saavutamiseks on jaotatud andmetega.

Andmete levitamine, andmebaas määrab kasutaja tabeli iga rea jaotatud ühes kohas.  Tabelid on räsi-jaotuse või ringi-jaan meetodi levitamine Lause CREATE TABLE määratud jaotuse. 

## <a name="hash-distributed-tables"></a>Räsi jaotatud tabelid
  
Järgmine diagramm näitab, kuidas räsi jaotatud tabelina talletatakse täielik (mitte jaotatud tabel). Tarkadeks funktsioon määrab iga rea kuulub ühe jaotuse. Tabeli määratlus, üks veerud on määratud veeru jaotuse. Funktsiooni räsi kasutab väärtust veerus jaotuse määrata iga rea iseloomustab jaotuse.

Näiteks eristatavuse, andmete skew ja päringutüüpide töötamine süsteemi on jaotuse veeru valiku kohta.
  
![Jaotatud tabel] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Jaotatud tabel")  
  
-   Iga rea kuulub ühe jaotuse.  
  
-   Tarkadeks räsi algoritmi määrab ühe jaotuse iga rea kohta.  
  
-   Jaotuse kohta tabeli ridade arv muutub nagu on näidatud erineva suurusega tabelite abil.

## <a name="round-robin-distributed-tables"></a>Round-jaan jaotatud tabelid

Round-jaan jaotatud tabeli jaotab määrate iga rea iseloomustab jaotuse algses read. See on kiire round-jaan tabeli andmete laadimiseks, kuid päringu võib olla aeglasem.  Liitumine round-jaan tabeli tavaliselt nõuab ümberkorraldamisele lubamiseks andes õige tulemuse, mis võtab aega päringu ridu.

## <a name="distributed-storage-locations-are-called-distributions"></a>Jaotatud salvestusruumi asukohad nimetatakse üldiste müügijaotustega tutvumine

Iga jaotatud asukohta nimetatakse iseloomustab jaotuse. Kui päring töötab samal ajal, sooritab iga jaotuse SQL-päringu selle osa andmeid. SQL-i andmebaas kasutab SQL-andmebaasi päringu käivitamiseks. Paralleelselt andmebaas kasutab SQL serveri päringu käivitamiseks. Selle ühiskasutusse andnud midagi arhitektuur kujundus on äärmiselt oluline skaala-out paralleelselt arvutuste.

### <a name="can-i-view-the-distributions"></a>Saate vaadata selle jaotuse?

Iga jaotus on jaotuse ID ja on nähtav süsteemi vaateid, mis on seotud SQL-i andmebaas ja paralleelselt andmebaas. Jaotuse ID abil saate päringu jõudluse ja muude probleemide tõrkeotsing. Süsteemi vaated loendi leiate teemast [MPP süsteemide vaade](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Leviloendi ja Arvuta sõlme erinevus

Leviloendi põhiühik on jaotatud andmete talletamiseks ja töötlemine paralleelselt päringuid. Jaotuse rühmitatakse Arvuta sõlmed. Iga MSDN jälitab ühe või mitme jaotuse.  

-   Kasutusanalüüsi platvormi süsteemi kasutab Arvuta sõlmed riistvara arhitektuur ja skaala-out võimaluste keskne osa. Alati kasutatakse kaheksa jaotuse kohta MSDN, nagu on näidatud räsi jaotatud tabelitega skeem. Arvuta sõlmed arvu, mistõttu jaotuse, arv määratakse Arvuta sõlmed ostate seadme arv. Näiteks kui ostate kaheksa Arvuta sõlmed, saate 64 üldiste müügijaotustega tutvumine (8 sõlmed x 8 jaotuse/MSDN). 

-   SQL-i andmebaas on kindla arvu 60 jaotuse ja Arvuta sõlmed arvuga. Arvuta sõlmed rakendatakse Azure ja ressursid. Arvuta sõlmed arvu muutmiseks vastavalt kirjutamata teenuse töökoormus ja määrate andmebaas arvuti võimsus (DWUs). Arvuta sõlmed arvu muutumise kohta MSDN jaotuse arv ka muutub. 

### <a name="can-i-view-the-compute-nodes"></a>Saate vaadata Arvuta sõlmed?

Iga MSDN on sõlm ID ja on nähtav süsteemi vaateid, mis on seotud SQL-i andmebaas ja paralleelselt andmebaas.  Saate vaadata Arvuta sõlme süsteemi vaated, mille nimi algab sys.pdw_nodes node_id veeru järgi. Süsteemi vaated loendi leiate teemast [MPP süsteemide vaade](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Kopeeritud tabelite jaoks Paralleelandmete ladu 
  
Kehtib: paralleelselt andmebaas

Lisaks jaotatud tabelite kasutamine paralleelselt andmebaas pakub võimalust ise tabelid. *Kopeeritud tabel* on tabel, mis on talletatud tervikuna iga MSDN. Tabeli imitatsiooniga eemaldab vaja edastamiseks selle tabeli ridade vahel Arvuta sõlmed enne Liitu või koondamine tabeli abil. Tiražeeritud tabelid, on üksnes väike, tabelid tõttu täiendav salvestusruum, mis on nõutav iga MSDN täielik tabeli salvestamiseks.  
  
Järgmisel joonisel on esitatud kopeeritud tabeli, mis on talletatud iga MSDN. Kopeeritud tabeli talletatakse üle kõik määratud Arvuta sõlme ketast. Ketas strateegia rakendatakse SQL serveri filegroups abil.  
  
![Paljundatud tabel] (media/sql-data-warehouse-distributed-data/replicated-table.png "Paljundatud tabel") 
  
## <a name="next-steps"></a>Järgmised sammud
  
Jaotatud tabelite tõhusaks kasutamiseks lugege teemat [jagamine tabelid SQL-andmebaas](sql-data-warehouse-tables-distribute.md)  
  



  
