<properties
   pageTitle="Power BI kasutamine SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid Power BI abil SQL Azure'i andmebaas lahendusi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-power-bi-with-sql-data-warehouse"></a>Power BI kasutamine SQL-andmebaas
Nimega Azure'i SQL-andmebaasi, kus SQL-i andmete ladu otse ühendust võimaldab kasutajal kasutada võimsaid loogilise pinustamine analytical Power BI võimaluste kõrval.  Otse ühendust luua, koos päringud saadetakse tagasi oma Azure SQL-i andmebaas reaalajas nagu andmete uurimiseks.  See koos skaala SQL-i andmebaas, võimaldab kasutajatel minutiga vastu TB andmete dünaamiline aruannete loomiseks.  Lisaks Power BI nupuga Ava kasutuselevõtt võimaldab kasutajatel ühendamiseks otse Power BI nende SQL-i andmebaas ilma mujalt Azure'i teabe kogumine.

Kui kasutate otse ühendust palun Märkus:

+ Täielikult kvalifitseeritud serveri nimi Määrake ühendamisel (vt allpool lähemalt)
+ Veenduge, et tulemüüri reeglid andmebaasi on konfigureeritud "Luba juurdepääs Azure'i teenuste".
+ Iga toimingu, nt veeru valimine või filtri lisamine otse päringu andmebaas
+ Paanide värskendatakse ligikaudu iga 15 minuti (Värskenda ei pea olema ajastatud)
+ K ja v ei ole saadaval andmekomplektide otsese ühenduse loomine
+ Muudatused on peale automaatselt
+ Kõik otsese ühenduse päringud kuvatakse ajalõpuni 2 minuti pärast

Need piirangud ja märkmete võivad muutuda jätkame parandada kogemusi. Ühenduse juhised on toodud allpool.  

## <a name="using-the-open-in-power-bi-button"></a>Nupu Ava Power BI abil
Lihtsaim viis oma SQL-i andmebaas ja Power BI vahel liikuda on Power BI nuppu Ava. Selle nupu abil saate alustada sujuvalt Power BI uue armatuurlaudade loomine.  

1.  Alustamiseks liikuge oma SQL-i andmebaas eksemplari Azure'i klassikaline portaalis.
2.  Klõpsake nuppu Ava Power BI.
3.  Kui me ei saa te otse sisse logida või kui teil pole Power BI kontot, peate sisse logima.  
4.  Suunatakse leht ühenduse SQL-i andmebaas oma SQL-i andmebaas eelasustatud teabega.
5.  Pärast sisestama oma mandaadi saate täielikult ühendatakse teie SQL-andmebaas.

## <a name="connecting-through-the-power-bi-portal"></a>Ühenduse loomine Power BI portaali kaudu
Lisaks rakendust Power BI nupuga Ava, saate kasutajad ühendada Power BI portaali kaudu oma SQL-i andmebaas.

1.  Klõpsake nuppu "Saada andmed" navigeerimispaani allosas.
2.  Valige "Andmebaasid".
3.  Üks kord valige andmebaaside lehel 'Azure SQL-i andmebaas' ja seejärel nuppu "Ühenda".
4.  Sisestage vajalikud ühenduse teave.  Oma serveri nimi ja andmebaasi nimi leiate Azure'i portaalis.
5.  Power BI avalehele naasmiseks liiklus ja pärast teie ühendus on loodud uue kirje jaotises "Andmekomplektide" kuvatakse teie eksemplari nimi.  
6.   Võite klõpsata uue andmekomplekti uurida kõik tabelid ja vaated oma andmebaasi. Veeru valimine saadab päringu tagasi allikas, dünaamiliselt luua oma visuaal. Nende visuaale saate uue aruande salvestatud ja kinnitatud tagasi armatuurlauale.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
