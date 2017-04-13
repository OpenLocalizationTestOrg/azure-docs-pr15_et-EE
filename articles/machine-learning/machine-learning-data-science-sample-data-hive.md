<properties
    pageTitle="Näidisandmed Azure Hdinsightiga taru tabelite | Microsoft Azure'i"
    description="Windows Azure Hdinsightiga (Hadopop) taru tabeli andmete proovide allapoole"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Näidisandmete Azure Hdinsightiga taru tabelites

Selles artiklis kirjeldame kuidas down valimiga taru päringute kasutamine Azure Hdinsightiga taru tabelites talletatavad andmed. Käsitleme rahva kasutatud valimite kolm võimalust:

* Ühtne juhusliku valimi
* Juhusliku valimi rühmad
* Stratifitseeritud valimite

**Miks teie näidisandmed?**
Kui kavatsete analüüsida andmekomplekti on suur, on tavaliselt on hea mõte kasutada alla valimiga väiksem, kuid esindaja ja paremini hallataval mahu vähendamiseks andmed. See hõlbustab andmete mõistmine, avastamine ja funktsioon matemaatika erifunktsioonid. Selle rolli andmete meeskonna teadus protsess on kiire prototüüpimine töötlemise funktsioonidest ja seadme õppe mudelid lubamine.

**Menüü** allpool näidisandmed erinevate salvestusruumi keskkonnas, kuidas kirjeldavate teemade lingid.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Selle toimingu valimite on samm [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Kuidas esitada taru päringud
Hadoopi käsurea konsooli Hadoopi kobar pea sõlme saab esitada taru päringud. Selleks sisse logida Hadoopi kobar pea sõlme, avage Hadoopi käsurea konsool ja taru päringute seal esitada. Esitamise taru päringute Hadoopi käsurea konsooli juhised leiate teemast [taru esitada päringuid](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Ühtne juhusliku valimi
Ühtne juhusliku valimi tähendab, et iga rea andmehulga on võrdne võimalus millest. See saab rakendada, lisades mõne eest välja rand() andmehulga sisemine "valige" päringu ja väline "valige" päringu see tingimus selle juhusliku välja.

Siin on näide päring.

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Siin `<sample rate, 0-1>` määrab kirjeid, mida kasutajad soovite Kuulake osa.

## <a name="group"></a>Juhusliku valimi rühmad

Kui valimite kategooriline andmeid, võiksite kaasamiseks või kõik eksemplarid mõne kindla kategooriline muutuja väärtust. See on, mida tähendab "valimite rühma alusel".
Näiteks, kui teil on kategooriline muutuja "Riik", mis on väärtused NY, MA, CA, NJ, PA jne, soovite sama riigi kirjed tuleb alati, kas nad kogutakse või mitte.

Siin on näide päring selle näidised rühma alusel.

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Stratifitseeritud valimite

Juhusliku valimi on stratifitseeritud kategooriline muutuja suhtes kui saadud proovide on väärtused, et kategooriline, mis on sama suhe nagu populatsioon, millest saadi näidised. Sama nimega näite ülaltoodud Oletame, et teie andmed on rühmad, riigid, öelda NJ on 100 vaatluste, NY on 60 vaatluste ja WA on vaatluste 300. Kui määrate stratifitseeritud valimite olema 0,5 määr, siis saadud proovi peaks olema umbes 50, 30 ja 150 vaatluste NJ, NY ja WA vastavalt.

Siin on näide päring.

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Täpsemate valimite meetodid, mis on saadaval taru kohta leiate teavet teemast [LanguageManual valimite](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
