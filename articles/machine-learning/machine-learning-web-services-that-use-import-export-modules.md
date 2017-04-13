<properties
    pageTitle="Azure'i ML veebiteenuste kasutamine andmete importimine ja eksportimine andmete moodulid juurutamine | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada andmete importimine ja eksportimine andmete moodulid saata ja vastu võtta andmed veebiteenusest."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Kasutage impordi- ja andmete eksportimine moodulid Azure'i ML veebiteenuste juurutamine 

Kui loote sõnastikupõhise katse, lisamist tavaliselt web teenuse sisendi ja väljundi. Katse juurutamisel tarbijad saate saata ja vastu võtta andmete veebiteenuse sisendi ja väljundi kaudu. Mõnede rakenduste tarbija andmete võib Andmekanal saadaval või juba elavad nagu Azure'i bloobimälu välise andmeallikaga. Sellisel juhul ei pea web teenuse sisendi ja väljundi abil andmete lugemine ja kirjutamine. Need saate selle asemel paketi täitmise teenuse (BES) abil saate andmeid lugeda mõne andmete importimine mooduli kasutamise andmeallikast ja kirjutage hinded tulemusi on andmete eksportimine mooduli kasutamise kohta erinevat tüüpi andmete.

Andmete importimine ja eksportimine andmete moodulid, saate lugeda ja kirjutada andmed asukohta, nt Web URL-i kaudu HTTP, taru päringu, Azure'i SQL-andmebaasiga, Azure'i tabelimälu, Azure'i bloobimälu Andmekanalist pakuvad või kohapealne SQL-andmebaasi arvu.

Selles teemas kasutab funktsiooni "näide 5: rongis, Test, hinnata kahendarvu liigitamiseks: täiskasvanud andmekomplekti" Kuulake ja eeldab andmekomplekti on juba laaditud nimega censusdata Azure SQL-i tabelisse.

## <a name="create-the-training-experiment"></a>Koolitus katse loomine 
 
Avamisel on "näide 5: rongis, Test, hinnata kahendarvu liigitamiseks: täiskasvanud andmekomplekti" näide kasutab valimi täiskasvanud rahvaloendus tulu kahendarvu liigitamine andmekomplekti. Ja katse lõuendile sarnaneb järgmisel pildil.

![Esialgne konfiguratsioon katse.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

SQL Azure'i tabelist andmeid lugeda:

1.  Andmekomplekti mooduli kustutada.
2.  Tippige otsinguväljale komponendid impordi.
3.  Tulemite loendis soovitud *Andmete importimine* mooduli lisada katse lõuend.
4.  *Andmete importimine* mooduli *Clean puuduvad andmed* mooduli sisendi väljundi ühendada.
5.  Valige paanil atribuudid **Azure'i SQL-andmebaasi** **Andmeallika** ripploend.
6.  **Andmebaasiserveri nimi**, sisestage **andmebaasi nimi**, **kasutajanimi**ja **parool** väljadele asjakohane teave andmebaasi.
7.  Sisestage väljale andmebaasi päringu järgmine päring.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  Katse allosas lõuend, klõpsake nuppu **Käivita**.

## <a name="create-the-predictive-experiment"></a>Sõnastikupõhise katse loomine

Häälestate sõnastikupõhise katse, millest juurutate oma veebiteenuse edasi.

1.  Katse lõuend allosas käsku **Määra veebiteenuse** ja valige **Sõnastikupõhise veebiteenuse [soovitatav]**.
2.  Sõnastikupõhise katse *Web teenuse sisestus* - ja *Web teenuse väljundi moodulid* eemaldada. 
3.  Tippige otsinguväljale komponendid ekspordi.
4.  Tulemite loendis soovitud *Andmete eksportimine* mooduli lisada katse lõuend.
5.  Ühendage väljundi *Keskmine mudeli* mooduli sisestatud *Andmete eksportimine* mooduli. 
6.  Valige paanil atribuudid **Azure'i SQL-andmebaasi** andmete sihtkoht ripploend.
7.  **Andmebaasiserveri nimi** **andmebaasi nimi**ja **Serveri konto kasutajanimi** **serveri kasutajakonto parooli** väljad, sisestage soovitud teave andmebaasi.
8.  Tippige väljale **komaga eraldatud loend salvestada** viskas sildid.
9.  Tippige **andmed tabeli nimi väljal**dbo. ScoredLabels. Kui tabel pole olemas, luuakse katse käitatakse või veebiteenus nimetatakse.
10. Tippige väljale **komaga eraldatud loend Saidihaldur** ScoredLabels.

Kui kirjutate lõplik veebiteenuse rakendus, võite määrata eri Sisestuskeel päringu või sihttabel käitusajal. Nende sisendi ja väljundi konfigureerimiseks saate Web teenuse parameetrite funktsiooni atribuudi *Andmete importimine* mooduli *andmeallika* ja *Andmete eksportimine* režiimi andmete sihtkoht atribuudi seadmiseks.  Web teenuse parameetrite kohta leiate lisateavet teemast [AzureML Web teenuse parameetrite kirje](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) Cortana ärianalüüsi ja seadme õ ajaveebi.

Konfigureerida Web teenuse parameetrid importimine päring ja sihttabel:

1.  *Andmete importimine* mooduli paanil atribuudid, klõpsake ikooni ülaosas paremal **andmebaasi päringu** väli ja valige **Määra veebiteenuse parameeter**.
2.  *Andmete eksportimine* mooduli paanil atribuudid, klõpsake ikooni ülaosas välja **andmete tabeli nimi** ja valige **Määra veebiteenuse parameeter**.
3.  *Andmete eksportimine* mooduli paanil atribuudid jaotises **Web teenuse parameetrid** allservas nuppu andmebaasi päringut ja pange sellele päringu.
4.  Klõpsake **andmete tabeli nimi** selle **tabeli**ümber nimetada.

Kui olete lõpetanud oma katse peaks sarnanema järgmisel pildil.
 
![Lõplik ilme katse.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Nüüd saate juurutada katse nimega veebiteenusest.

## <a name="deploy-the-web-service"></a>Veebiteenuse juurutamine 
Saate juurutada klassikaline või uue veebiteenuse.

### <a name="deploy-a-classic-web-service"></a>Klassikaline veebiteenuse juurutamine

Kui klassikaline veebiteenuse juurutada ja luua rakenduse kasutamine selle:

1.  Katse lõuend allservas nuppu Käivita.
2.  Kui Käivita on lõpule jõudnud, klõpsake **Juurutada veebiteenuse** ja valige **Juurutamine veebiteenuse [Classic]**.
3.  Web teenuste armatuurlaua, otsige üles oma API võti. Kopeerige ja salvestage see hilisemaks kasutamiseks.
4.  Tabelis **Vaikimisi lõpp-punkti** linki **Paketi täitmise** API aitab lehe avamiseks.
5.  Looge Visual Studios, C# konsooli rakendus.
6.  Leidke API spikri lehel **Proovi kood** jaotis lehe allosas.
7.  Kopeerida ja kleepida Program.cs faili C# proovi kood ja eemaldada kogu viiteid selle bloobimälu.
8.  Varem salvestatud API võti värskendada *apiKey* muutuja väärtus.
9.  Leidke taotluse deklareerimise ja värskendada edastatakse *Andmete importimine* ja *Eksportimine andmete* moodulid Web teenuse parameetrite väärtused. Sel juhul on kasutada algset päringut, kuid määratlemine uue tabeli nimi.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. Käivitage rakendus. 

Käivita lõpetamisel, lisatakse uus tabel hinded tulemuste andmebaasi.

### <a name="deploy-a-new-web-service"></a>Uue veebiteenuse juurutamine

Kui uus veebiteenus juurutada ja luua rakenduse kasutamine selle:

1.  Katse allosas lõuend, klõpsake nuppu **Käivita**.
2.  Kui Käivita on lõpule jõudnud, klõpsake **Juurutada veebiteenuse** ja valige **Juurutamine veebiteenuse [uus]**.
3.  Katse juurutada lehel sisestage oma veebiteenuse nimi ja valige hinnakirjad leping, klõpsake nuppu **Deploy**.
4.  Klõpsake lehel **Kiirjuhend** **tarbida**.
5.  Klõpsake jaotises **Proovi kood** **paketi**.
6.  Looge Visual Studios, C# konsooli rakendus.
7.  Kopeerige ja kleepige oma Program.cs faili C# proovi kood.
8.  Värskendage **Primaarvõtme** asub **lihtsa tarbimine teave** jaotises *apiKey* muutuja väärtus.
9.  Leidke *scoreRequest* deklareerimise ja värskendada edastatakse *Andmete importimine* ja *Eksportimine andmete* moodulid Web teenuse parameetrite väärtused. Sel juhul on kasutada algset päringut, kuid määratlemine uue tabeli nimi.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. Käivitage rakendus. 
 

