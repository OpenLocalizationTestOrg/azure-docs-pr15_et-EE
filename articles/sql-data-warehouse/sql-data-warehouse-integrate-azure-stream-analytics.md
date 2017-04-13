<properties
   pageTitle="Voo kasutamiseks Azure SQL-i andmebaas Analytics | Microsoft Azure'i"
   description="Näpunäiteid kasutades Azure voo Analytics SQL Azure'i andmebaas lahendusi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Azure'i voo Analytics kasutamine SQL-andmebaas

Azure'i voo Analytics on täielikult hallatav teenus, mis hõlmab latentsusajaga, väga kättesaadav, scalable keerukate sündmuse töötlemiseks üle streaming andmed pilveteenuses. Saate teada põhitõed, lugedes [Azure'i voo Analytics tutvustus][]. Seejärel saate teada, kuidas luua-lõpuni lahenduse voo Analytics järgides [Azure'i voo Analyticsi kasutamise alustamise][] õpetus.

Sellest artiklist saate teada, kuidas kasutada andmebaasi SQL Azure'i andmebaas on väljundi valamu oma Steam Analytics tööde haldamine.

## <a name="prerequisites"></a>Eeltingimused

Esmalt käivitage järgmiste toimingute abil õpetuses [Azure'i voo Analyticsi kasutamise alustamine][] .  

1. Sündmuse jaoturi sisend loomine
2. Sündmuse generaator rakenduse käivitamiseks ja konfigureerimiseks
3. Voo Analytics töö ettevalmistamine
4. Määrake töö sisestus- ja päring

Klõpsake SQL Azure'i andmebaas andmebaasi loomine

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Määrake töö väljund: SQL Azure'i andmebaas andmebaas

### <a name="step-1"></a>Samm 1

Oma töö voo Analytics **väljundi** lehe ülaosas nuppu ja klõpsake **Väljundi lisada**.

### <a name="step-2"></a>Samm 2

Valige SQL-andmebaas ja klõpsake nuppu edasi.

![][add-output]

### <a name="step-3"></a>Samm 3
Järgmisel lehel sisestage järgmised väärtused:

- *Väljundi alias (pseudonüüm)*: Sisestage sõbralik nimi selle töö väljund.
- *Tellimus*:
    - Kui andmebaasi SQL-i andmebaas on samas tellimuses voo Analytics töö, valige Kasuta SQL-andmebaasi praeguselt tellimuselt.
    - Kui teie andmebaas on mõni muu tellimus, valige Kasuta SQL-andmebaasi teise tellimusest.
- *Andmebaasi*: määrake sihtkoht andmebaasi nimi.
- *Serveri nimi*: serveri nime määratlemiseks ainult määratud andmebaasi. Saate selle Azure'i klassikaline portaal.

![][server-name]

- *Kasutajanimi*: kasutaja kontoga, millel on kirjutusõigusi andmebaasi nimi.
- *Parooli*: ette määratud kasutajakonto parooli.
- *Tabel*: määrake sihttabeli nimi andmebaasis.

![][add-database]

### <a name="step-4"></a>Samm 4

Klõpsake nuppu kontrolli selle töö väljundi lisamiseks ja kinnitamiseks, et voo Analytics saate ühendada andmebaasiga.

![][test-connection]

Kui andmebaasiga ühendus on loodud, kuvatakse teavitus portaali allosas. Võite klõpsata allosas andmebaasiga ühenduse testimiseks testi ühendust.

## <a name="next-steps"></a>Järgmised sammud

Integreerimine ülevaate leiate teemast [SQL-i andmebaas integreerimine ülevaade][].

Veel arengu näpunäiteid teemast [SQL-i andmebaas arengu ülevaade][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Azure'i voo Analytics tutvustus]: ../stream-analytics/stream-analytics-introduction.md
[Azure'i voo Analyticsi kasutamise alustamine]: ../stream-analytics/stream-analytics-get-started.md
[SQL-i andmebaas arengu ülevaade]:  ./sql-data-warehouse-overview-develop.md
[SQL-i andmebaas integreerimine ülevaade]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
