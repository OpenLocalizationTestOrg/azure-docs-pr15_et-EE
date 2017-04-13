<properties
    pageTitle="Stsenaariumid tuvastamiseks ja kavandamine Täpsemad andmete analüüs | Microsoft Azure'i"
    description="Täiustatud analüüsi sarja otsiti leides kavandamine."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Kuidas teha kindlaks stsenaariumid ja täiustatud analytics andmete töötlemise kavandamine

Millised ressursid tuleks kavatsete teha töötlemiseks andmekogumis täiustatud analüüsi keskkonnas häälestamisel kaasata? Selles artiklis soovitab küsida küsimusi, mis aitab tuvastada, ülesannete ja ressursside stsenaariumist sarja. Üksikasjalik juhiseid järjestust ennustav on ära toodud [mis on meeskonnatöö andmete teadus protsess (TDSP)?](data-science-process-overview.md). Neid juhiseid iga nõuab teatud ressursid oluline kindla stsenaariumist tööülesanded. Võtme küsimused tuvastamiseks stsenaariumist seotud andmete logistika, omaduste andmekomplektide, tööriistad ja keeled, mida eelistate teha analüüsi kvaliteet.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistilist küsimused: andmete asukohad ja liikumine
Logistilist küsimused käsitlevad asukoha **andmeallika**, soovitud **sihtkohta suunata** Azure ja teisaldada andmed, sh ajakava, summa ja ressursse, mis on seotud nõuded. Teisaldada mitu korda analüüsi käigus vajalikud andmed. Stsenaarium on kohalikud andmed viimine salvestusruumi mingi Azure ja siis arvuti õ Studio.

1. **Mis on teie andmeallika?** See on kohaliku või pilves? Näiteks:
    - Andmed on avalikult saadaval HTTP-aadress.
    - Andmete asub kohalik/network faili asukoht.
    - Andmed on SQL serveri andmebaasi.
    - Andmed on salvestatud on Azure storage ümbrises

2. **Mis on Azure sihtkoht?** Kui see ei vaja või töötlemise modelleerimine olla? Näiteks:
    - Azure'i bloobimälu
    - SQL Azure'i andmebaasid
    - SQL Server Azure'i VM
    - HDInsight (Hadoopi Azure) või taru tabelid
    - Azure'i masina õpetused
    - Paigaldatud Azure virtuaalse kõvaketta.

3. **Kuidas kavatsete andmeid teisaldada?** Toimingute ja -ressursside neelata või mitmesuguseid eri salvestusruumi ja töötlemine keskkonnas andmete laadimine on kirjeldatud järgmistes teemades.

    -  [Salvestusruumi keskkonnas analytics andmete laadimine](machine-learning-data-science-ingest-data.md)
    -  [Koolitus üheks Azure seadme õ Studio mitmesugustest andmeallikatest pärinevate andmete importimine](machine-learning-data-science-import-data.md).

4. **Andmeid vajab regulaarselt teisaldada või muudetud migreerimisel?** Kaaluge Azure'i Factory (ADF) kui andmeid on vaja migreerida pidevalt, eriti siis, kui hübriidjuurutuse stsenaarium, mis kasutab nii kohapealne ja pilveteenuse ressursid on seotud, või kui andmed on tehingu või muuta või selle käigus migreerida lisatakse äriloogika on vaja. Lisateabe saamiseks vt [kohapealne SQL serveri SQL Azure'i Azure andmete Factory abil andmete teisaldamine](machine-learning-data-science-move-sql-azure-adf.md)

5. **Kui palju andmeid on Azure teisaldada?** Mis on väga suurte andmekomplektide tohi mälumaht keskkondades teatud. Näiteks vt arutelu mahupiirangud masina õ Studio järgmise jaotise. Sellisel juhul võib kasutada andmete valimi analüüsi käigus. Lisateavet alla valimiga andmekomplekt erinevate Azure keskkonnas leiate teemast [Näidisandmete andmete meeskonna teadus käigus](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Andmete omadused küsimused: tüüp, vorming ja maht
Neile küsimustele on salvestusruumi plaanimine ja töötlemine keskkonnas, mis sobivad erinevat tüüpi andmetega ja mis kehtivad teatud piirangud.

1. **Mis on andmetüüpide?** Näiteks:
    - Arvuline
    - Kategooriline
    - Stringide
    - Binaarsed

2. **Kuidas oma andmeid vormindatud?** Näiteks:
    - Komaga eraldatud (CSV) või tabeleraldusega (TSV) tasapinnalise failid
    - Tihendatud või tihendamata
    - Azure'i plekid
    - Hadoopi taru tabelid
    - SQL serveri tabelid

2. **Kui suur on teie andmeid?**
    - Väike: väiksem kui 2GB
    - Keskmise: Üle 2GB ja väiksem kui 10GB
    - Suur: Suurem kui 10GB

Võtke Azure seadme õ Studio keskkonnas, näiteks:

- Andmevormingute ja Azure seadme õ Studio, vt jaotist [vormingud ja andmete andmetüübid toetatud](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) toetatud tüüpi loendi.
- Andmete piirangud Azure seadme õ Studio kohta leiate teavet teemast funktsiooni **kui suur andmehulga saab minu moodulid?** jaotises [seadme õppe andmete eksportimine](machine-learning-faq.md#machine-learning-studio-questions) ja importimine

Analüüsi käigus kasutada Azure teenuste piirangud on teile kohta leiate teavet teemast [Azure tellimuse ja teenuste piirangud, piirmäära, ja piiranguid](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Andmete kvaliteedi küsimused: uurimine ja eelse töötlemine

1. **Mida teha andmete kohta teadma?** Andmete uurimine, kui teil on vaja selle peamised omadused on aru saada. Mida mustrite või trende selle eksponaate, mis on võõrväärtuste on või mitu väärtused on puudu. See on oluline eelse töötlemine vajalik, hüpoteesid, mis kõige paremini funktsioonide soovitamine või tippige analüüsi kujundamiseks ja kujundamiseks lepingute täiendavate andmete kogumise ulatuse kindlakstegemiseks. Kirjeldav statistika ja joonestamist visualiseeringud on kasulik meetodite abil andmete kontrollimiseks. Sellest, kuidas uurida erinevates Azure keskkondades andmekomplekt leiate teemast [andmete meeskonna andmete teadus protsessi uurimine](machine-learning-data-science-explore-data.md).

2. **Kas andmed on nõutav eeltöötlus või puhastamiseks?**
Eeltöötlus ja andmete puhastamine on tähtsad tööülesanded, mis tavaliselt tuleb teha enne andmekomplekti saab kasutada tõhusalt masina õ. Lähteandmed on sageli lärmakas ja usaldusväärsed ja võib olla puudu väärtused. Selliste andmete modelleerimiseks abil saate eksitavat tulemusi. Kirjelduse leiate teemast [ülesannete ettevalmistamiseks andmete täiustatud seadme Õppekeskuse](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Tööriistad ja keeled küsimused
On palju võimalusi siin, olenevalt sellest, millised keeled ja arengu keskkonnas või Tööriistad vaja või on kõige kuulekas abil.

1. **Milliseid keeli te eelistate kasutada analüüsi jaoks?**  
    - R
    - Python
    - SQL-I

2. **Mis tööriistad tuleks kasutada andmete analüüsimiseks?**
    - [Microsoft Azure'i PowerShelli](powershell-install-configure.md) - skript keel, mida kasutatakse hallata oma Azure ressursse skripti keeles.
    - [Azure'i masina õ Studio](machine-learning-what-is-ml-studio/)
    - [Revolutsioon Analytics](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python Tools for Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter märkmikud](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Täiustatud analüüsi stsenaariumist tuvastamine
Kui olete vastanud küsimustele eelmises jaotises, olete valmis, määrata, milline stsenaarium, mis on parim sobib teie puhul. [Täiustatud analüüsi Azure seadme õppe stsenaariumid](machine-learning-data-science-plan-sample-scenarios.md)on esitatud valimi stsenaariumid.
