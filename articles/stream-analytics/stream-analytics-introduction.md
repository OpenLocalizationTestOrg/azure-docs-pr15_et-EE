<properties 
    pageTitle="Sissejuhatus voo Analytics | Microsoft Azure'i" 
    description="Lisateavet voo Analytics hallatav teenus, mis aitab teil soovitud Internet, asjade streaming andmeid analüüsida reaalajas." 
    keywords="Kasutusanalüüsi hallatav teenus, teenused, voo töötlemine, streaming analytics, mis on voo analytics"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Mis on voo Analytics?

Azure'i voo Analytics on täielikult hallatud, tõhus reaalajas sündmuse töötlemine mootori, mis aitab avada sügav ülevaateid andmete põhjal. Voo Analytics lihtne häälestamine reaalajas analüütiliste arvutuste andmete voogesitamine seadmed, andurid, veebisaitide, sotsiaalmeedia, rakendused, taristu süsteemid ja palju muud.

Azure'i portaalis vaid mõne hiireklõpsuga te saate Autor voo Analytics töö määramine sisendandmete allikas streaming andmete väljundi valamu tulemused oma töö ja andmete teisendus väljendatud SQL-like keeles. Saate jälgida ja kohandada oma töö Azure portaali kaudu mõne KB GB või rohkem sekundis töödelda sündmusi skaala skaala ja kiiruse.

Voo Analytics mõjutab aasta Microsoft Researchi töö väljatöötamisel väga häälestatud streaming mootorite kiirete töötlemiseks, samuti keele integratsioon intuitiivne kirjelduste sellise.

## <a name="what-can-i-use-stream-analytics-for"></a>Mida saab voo analüüsiteabe kasutada?
Suure hulga andmete voolavad veebisaidil kõrge kiiruse kaabel täna. Ettevõtted, mida saab töödelda ja toimivad streaming reaalajas andmed oluliselt saate parandada tõhusust ja ise turu eristada. Reaalajas streaming Analyticsi stsenaariumid leiate kõigis tööstusharudes: isikupärastatud, reaalajas stock börsipäev analüüsimise ja teatiste pakutud finants ettevõtetele; reaalajas pettuse tuvastamise; andmete ja identiteedi kaitse teenused; usaldusväärne manustamisest ja andmeanalüüsi andurid ja täiturseadised loodud manustatud füüsilise objektid (asjade Interneti või asjade); veebianalüütika Click Stream; ja kliendi seose haldus (CRM) rakenduste välja teateid, kui on halvenenud klientide programmikasutuskogemuse aja jooksul. Ettevõtted otsivad paindlik, usaldusväärseid ja kulutõhus viis seda teha sellise reaalajas sündmuse-voo Andmeanalüüs õnnestub konkurentsiga tänapäevane business geograafiline asukoht.

## <a name="key-capabilities-and-benefits"></a>Võtme funktsioonid ja eelised
-   **Kasutuslihtsus:** Voo Analytics toetab lihtne, deklaratiivseid päringu mudeli teisendused kirjeldamiseks. Kasutuslihtsus optimeerida voo Analytics kasutab T-SQL-i variandi ja eemaldab vaja klientide voo töötlemine süsteemide tehnilise keerulisi tegelema. Päringuredaktoris-brauseri abil [voo Analytics päringu keel](https://msdn.microsoft.com/library/azure/dn834998.aspx) , saate intelli-tunnet automaatteksti funktsiooni abil saate kiiresti ja hõlpsalt aja sarja päringud, sh ajaline vastavalt ühendused, akendega ja agregaadid, ajaline filtrite ja muude levinud toiminguid nagu ühendused, agregaadid, prognooside ja filtreid rakendada. Lisaks võimaldab brauseris päringu testimise vastu näidisfaili andmete kiire, iteratiivne areng.  

-   **Skaleeritavus:** Suure sündmuse läbilaskevõime on kuni 1GB sekundis töötlemise suudab voo Analytics. Integreerimine [Azure sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) ja [Azure asjade jaoturi](https://azure.microsoft.com/services/iot-hub/) luba neelata sündmuste kohta teise pärit ühendatud seadmete clickstreams, miljonid lahenduse ja logifailid nimi mõne. Selleks voo Analytics mõjutab sündmuse jaoturi, mis annavad 1 MB/s partition eraldamine võimaldab. Kasutajad saavad partition arvutus loogilise juhiseid päringu määratluse mitmeks, igal versioonil, võib-olla võimalus liigendatud suurendamiseks skaleeritavus.  

-   **Töökindluse, korratavuse ja kiire taastamine:** Hallatav teenus pilves, voo Analytics aitab vältida andmete kaotsimineku ja pakub järjepidevuse kaudu sisseehitatud funktsioonid ebaõnnestumise korral. Võimalus ettevõttesiseselt säilitada, teenus pakub korratavad tulemite tagamiseks on võimalik arhiivimine sündmused ja rakendage töötlemine tulevikus, alati saada sama tulemuse. See võimaldab klientide minna ajas tagasi ja arvutuste uurida, tehes juur-põhjuse analüüsi, mõjuanalüüsi jne.  

-   **Madal:** Nimega pilveteenus voo Analytics on optimeeritud kasutajatele väga väikese hinnaga alustamine ja reaalajas analytics lahenduste haldamine. Teenus on loodud teile maksma lähete vastavalt Streaming ühiku kasutus ja summa süsteemi töödeldavate andmete jaoks. Kasutus on tuletatud maht töödelda sündmusi ja Arvuta power ette valmistatud sees klaster voo Analytics töökohta käsitlema summa alusel.  

-   **Viitavad andmetele:** Voo Analytics võimaldab kasutajatel määrata ja kasutada andmeid. See võib olla ajaloolisi andmeid või lihtsalt-streaming andmeid, mis harvem aja jooksul muutub. Süsteemi lihtsustab lähteandmed käsitletakse nagu mis tahes muu sissetuleva sündmuse voo muu sündmuse voogu märkimisväärselt teha muudatusi reaalajas koos kasutamist.  

-   **Kasutaja määratletud funktsioonid:** Voo Analytics on integreerimine Azure seadme õ määratleda funktsiooni kõned masina õ teenus voo Analytics päringu osana. See avardab voo Analytics võimaluste võimendada Azure seadme õ olemasolevaid lahendusi. Lisateavet selle kohta, vaadake [seadme õ integreerimine õpetuse](stream-analytics-machine-learning-integration-tutorial.md).

-   **Ühenduvus:** Voo Analytics ühendab otse Azure'i sündmuse jaoturi ja Azure asjade jaoturi kohta voo ja Azure'i bloobimälu teenuse neelata andmeid. Tulemused saab kirjutada voo Analytics Azure salvestusruumi plekid või tabelite, Azure SQL-i DB, Azure'i andmed Lake poed, DocumentDB, sündmuse jaoturi, Azure'i teenus siini teemade või järjekorrad ja Power BI, kus seda saab siis visualiseerimise, täpsemaks töödelda töövood, kasutatud paketi Analytics [Azure Hdinsighti](https://azure.microsoft.com/services/hdinsight/) kaudu või töödelda uuesti üritused. Sündmuse jaoturi kasutamisel on võimalik koostada mitme voo Kasutusanalüüsi töötlemise mootorite koos muude andmeallikate voogesituse laadi arvutuste kaotamata.  

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud
Te olete võetud voo Analytics streaming analytics andmete põhjal asjade Interneti hallatav teenus. Selle teenuse kohta leiate lisateavet järgmistest teemadest.

- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)

