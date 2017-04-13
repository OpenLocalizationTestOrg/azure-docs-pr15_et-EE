<properties
   pageTitle="Azure'i masina Õppekeskuse kasutamine SQL-i andmebaas | Microsoft Azure'i"
   description="Õpetus Azure seadme õ abil SQL Azure'i andmebaas lahendusi."
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

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Azure'i masina Õppekeskuse kasutamine SQL-andmebaas

Azure'i masina õ on täielikult hallatud ennustav teenus, mis abil saate luua oma andmetega prognoosmudelite SQL-i andmebaas ja seejärel valmis tarbimine veebiteenuste avaldada. Saate teada, ennustav ja seadme Õppekeskuse lugedes [masina Õppekeskuse Azure tutvustus][]põhialused.  Seejärel saate teada, kuidas luua, koolitada, Keskmine ja testida arvuti õ mudeli [katsetamiseks õpetuse loomine][]abil.

Sellest artiklist saate teada, kuidas teha järgmist [Azure seadme õ Studio][]abil:

- Andmeid lugeda, luua, koolitada ning Keskmine prognoositava mudeli oma andmebaasist
- Andmebaasi andmete kirjutamine


## <a name="read-data-from-sql-data-warehouse"></a>Andmeid lugeda SQL-andmebaas

Me ei loe andmete andmebaasis AdventureWorksDW toote tabelist.

### <a name="step-1"></a>Samm 1

Uue katse käivitamiseks klõpsake nuppu + Uus arvuti õ Studio akna allosas valige katse, ja seejärel valige tühjaks katsetamiseks. Valige lõuend ülaosas katse vaikenime ja pange sellele midagi olulist, näiteks jalgratta hind ennustamine.

### <a name="step-2"></a>Samm 2

Otsige lugeja moodul värvipaleti andmekogumite ja moodulid vasakus servas katse lõuend. Lohistage katse lõuend.
![][drag_reader]

### <a name="step-3"></a>Samm 3

Valige lugeja mooduli ja täitke paanil atribuudid.

1. Valige andmeallikana Azure'i SQL-andmebaas.
2. Andmebaasiserveri nimi: sisestage serveri nimi. Saate selle [Azure'i portaalis][] .

![][server_name]

3. Andmebaasi nimi: Tippige andmebaasi nimi ainult määratud serveris.
4. Serveri konto kasutajanimi: tippige kasutaja kontoga, millel on juurdepääsuõigused andmebaasi nimi.
5. Kasutajakonto parooli server: ette määratud kasutajakonto parooli.
6. Mis tahes serveri serdi aktsepteerida: Kasutage seda suvandit (vähem turvaline), kui soovite vahele läbivaatamise saidi serdi enne oma andmeid lugeda.
7. Andmebaasi omapäringute: sisestage SQL-lause, mis kirjeldab andmeid, mida soovite lugeda. Selles näites me ei loe andmete tabel Product abil järgmine päring.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Samm 4

1. Käivitage katse, klõpsates nuppu Käivita alla katse lõuend.
2. Pärast katse lõppemist on lugeja mooduli roheline märge näitamaks, et see on lõpule viidud. Pange tähele ka lõpetatud, töötavad olek paremas ülanurgas.

![][run]

3. Imporditud andmete kuvamiseks klõpsake allosas auto andmehulga pordi väljundi ja valige Visualiseeri.


## <a name="create-train-and-score-a-model"></a>Luua, koolitada ja mudeli Keskmine

Nüüd saate selle andmekomplekti, et:

- Andmemudeli loomine: andmete töötlemiseks ja funktsioonide määratlemine
- Kui seate mudeli koolitada: valimine ja rakendamine õ algoritmi
- Keskmine ja testida mudeli: prognoosida uus jalgratta hind


![][model]

Lisateavet selle kohta, kuidas luua koolitada, Keskmine ja testimine Õppekeskuse mudeli kasutamine masina [loomine katsetamiseks õpetuse][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Kirjutage andmed SQL Azure'i andmebaas

Me kirjutada tulemuste komplekti ProductPriceForecast AdventureWorksDW andmebaasi tabelisse.

### <a name="step-1"></a>Samm 1

Otsige kirjutaja moodul värvipaleti andmekogumite ja moodulid vasakus servas katse lõuend. Lohistage katse lõuend.

![][drag_writer]

### <a name="step-2"></a>Samm 2

Valige kirjutaja mooduli ja täitke paanil atribuudid.

1. Valige SQL Azure'i andmebaasi andmete sihtkoht.
2. Andmebaasiserveri nimi: sisestage serveri nimi. Saate selle [Azure'i portaalis][] .
3. Andmebaasi nimi: Tippige andmebaasi nimi ainult määratud serveris.
4. Serveri konto kasutajanimi: tippige kasutaja kontoga, millel on kirjutusõigusi andmebaasi nimi.
5. Kasutajakonto parooli server: ette määratud kasutajakonto parooli.
6. Aktsepteeri kõik Serveri sert (ebaturvalise): valige see suvand, kui te ei soovi serdi kuvamine.
7. Komaga eraldatud loend veergude salvestada: loetleda andmekomplekti või tulem veerud, mida soovite väljund.
8. Andmete tabeli nimi: määrake andmete tabeli nimi.
9. Saidihaldur komaga eraldatud loend: Määrake kasutamine uue tabeli veergude nimed. Veergude nimed võib erineda allika andmekomplekti omadega, kuid teil peab olema sama veergude arv siin väljund tabeli määratleva.
10. SQL Azure'i toimingus kirjutada ridade arv: saate konfigureerida SQL-andmebaasiga ühe toiminguga kirjutada ridade arv.

![][writer_properties]

### <a name="step-3"></a>Samm 3

1. Käivitage katse, klõpsates nuppu Käivita alla katse lõuend.
2. Kui lõpetab katse, on kõik moodulid roheline märge näitamaks, et need on lõpule viidud.

## <a name="next-steps"></a>Järgmised sammud

Veel arengu näpunäiteid teemast [SQL-i andmebaas arengu ülevaade][].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL-i andmebaas arengu ülevaade]: ./sql-data-warehouse-overview-develop.md
[Looge katse õpetus]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Õppekeskuse Azure masina tutvustus]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure'i masina õ Studio]: https://studio.azureml.net/Home
[Azure'i portaal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
