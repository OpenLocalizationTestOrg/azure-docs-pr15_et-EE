<properties
   pageTitle="Azure'i masina õppimisega andmete analüüsiks | Microsoft Azure'i"
   description="Azure'i masina Õppekeskuse abil saate sõnastikupõhise masina Õppekeskuse mudel SQL Azure'i andmebaas talletatud andmete põhjal koostada."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Azure'i masina õppimisega andmete analüüsiks

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure'i masina õpetused](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Selle õpetuse kasutab Azure seadme Õppekeskuse sõnastikupõhise masina Õppekeskuse mudel SQL Azure'i andmebaas talletatud andmete põhjal koostada. Täpsemalt see koostab suunatud müügikampaania jaoks Adventure Worksi bike poes, prognoosida, kui tõenäoliselt kliendi osta bike või mitte.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Eeltingimused
Samm selle õpetuse kaudu, peate.

- SQL-i andmebaas eelnevalt laaditud AdventureWorksDW näidisandmeid. See säte, lugege teemat [loomine SQL-i andmebaas][] ja valige Näidisandmete laadimiseks. Kui teil juba on andmebaas, kuid ei saa näidisandmed, saate [laadida Näidisandmete käsitsi][].

## <a name="1-get-data"></a>1. andmete toomine
Andmed on AdventureWorksDW andmebaasis dbo.vTargetMail vaade. Lugeda andmed:

1. [Azure'i masina õ studio][] sisse logida ja klõpsake käsku Minu katsete.
2. Valige **+ Uus** ja valige **Tühi katse**.
3. Sisestage oma katse nimi: suunatud turundustegevuse.
4. Lohistage **lugeja** mooduli paanilt moodulid lõuend.
5. Määrake andmebaasi SQL-i andmebaas üksikasjade paanil atribuudid.
6. Määrake andmebaasi **päringu** huvipakkuvaid andmeid lugeda.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Käivitage katse, klõpsates nuppu **Käivita** alla katse lõuend.
![Käivitage katse][1]


Kui katse lõpule jõudnud edukalt töötab, klõpsake väljundi pordi lugeja mooduli allosas ja valige **Visualiseeri** imporditud andmete kuvamiseks.
![Imporditud andmete vaatamine][3]


## <a name="2-clean-the-data"></a>2 andmete puhastamiseks
Andmete puhastamiseks kukutage veerge, mis pole oluline mudeli. Soovitud toiming

1. Lohistage **Projekti veergude** mooduli lõuend.
2. Klõpsake paanil atribuudid, et määrata, milliseid veerge soovite esmasesse **käivitada veeru Vaateselektori** .
![Projekti veerud][4]

3. Kahe veergude välistamine: CustomerAlternateKey ja GeographyKey.
![Mittevajalike veergude eemaldamine][5]


## <a name="3-build-the-model"></a>3. mudeli koostamine
Jagame omavahel andmete 80 / 20: 80% mudeli ja 20% testimiseks mudeli masina koolitada. Teeme probleemi kahendarvu liigitamine "Kahe tunni" algoritmide kasutamiseks.

1. Lohistage **tükeldatud** mooduli lõuend.
2. Sisestage 0,8 murdosa ridade esimese väljundi andmekomplekti paanil atribuudid.
![Andmete tükeldamine koolitus ja test][6]
3. Lohistage **Kaks klassi võimendada otsustuspuu** mooduli lõuend.
4. **Rongi mudeli** mooduli lohistada lõuend ja määrake sisendeid. Klõpsake **veeru Vaateselektori käivitada** paanil atribuudid.
      - Esmalt sisestusteade: ML algoritmi.
      - Teine sisestusteade: andmete koolitada algoritmi kohta.
![Mooduli rongi mudeli ühenduse loomine][7]
5. Valige veerg **BikeBuyer** prognoosida veeruna.
![Valige veerg prognoosida][8]


## <a name="4-score-the-model"></a>4. Keskmine mudel
Nüüd me testida, kuidas mudeli sooritab testi andmete põhjal. Me võrdleb meie valik algoritmi koos algoritmi kuvamiseks, mis toimib paremini.

1. Lohistage **Keskmine mudeli** mooduli lõuend.
    Esmalt sisestusmeetodi: väljaõppe mudeli teine sisend: testige andmete ![Keskmine mudel][9]
2. Lohistage **Kaks klassi Bayes punkti masina** katse lõuend. Me võrdleb, kuidas seda algoritmi sooritab võrreldes kaks klassi võimendada Otsusepuu kuvamine.
3. Kopeerimine ja kleepimine moodulid rongi mudel ja Keskmine mudel lõuend.
4. Lohistage **Hinnata mudeli** mooduli võrdlemiseks kahe algoritmide lõuend.
5. **Käivitage** katse.
![Käivitage katse][10]
6. Väljundi pordi hinnata mudeli mooduli allservas ja seejärel klõpsake Visualiseeri.
![Hindamise tulemused visualiseerida][11]

Esitatud mõõdikud on ROC kõvera arvutustäpsuse-meelde skeemi ja tõstke kõver. Vaadates nende mõõdikute, näeme, et esimene mudel paremad kui teine. Vaadata, millised esimene mudel ennustatud, klõpsake väljundi pordi Keskmine mudeli ja klõpsake käsku Visualiseeri.
![Keskmine tulemused visualiseerida][12]

Kuvatakse kaks rohkem veerge, lisatakse teie testi andmekomplekti.

- Poolitusjoonega tõenäosus: tõenäosus, et kliendi on bike ostja.
- Poolitusjoonega Sildid: selle liigitus teha mudeli – bike ostja (1) või mitte (0). Selle tõenäosuse määra märgistamist seatakse 50% ja saab reguleerida.

Võrdlus veeru BikeBuyer (tegelik) viskas siltide (ennustamine), saate vaadata, kuidas täitnud mudel. Järgmised toimingud, kui saate selle mudeli teha prognoose uutele klientidele ja selle veebiteenuse mudeli avaldamine või kirjutage tulemused tagasi SQL-i andmebaas.

## <a name="next-steps"></a>Järgmised sammud

Sõnastikupõhise masina õppe mudelid koostamise kohta leiate Lisateavet vaadake [seadme Õppekeskuse Azure tutvustus][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure'i masina õ studio]:https://studio.azureml.net/
[Õppekeskuse Azure masina tutvustus]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Näidisandmete käsitsi laadimine]: sql-data-warehouse-load-sample-databases.md
[Looge SQL-i andmebaas]: sql-data-warehouse-get-started-provision.md
