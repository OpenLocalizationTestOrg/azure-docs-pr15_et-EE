<properties
    pageTitle="Azure seadme õ funktsioonidega tööpäevad voo Analytics mastaapimiseks | Microsoft Azure'i"
    description="Saate teada, kuidas õigesti mastaapimiseks voo Analytics tööde haldamine (eraldamine, SU kogus ja muud) kui Azure seadme õ funktsioonide abil."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Azure'i masina õ funktsioonidega tööpäevad voo Analytics skaala

Sageli on üsna lihtne seadistada mõne voo Analytics töö ja Näidisandmete läbida. Mida me teha, kui peame suurema mahuga sama töö? See nõuab, et aru saada, kuidas konfigureerida voo Analytics töö nii, et see muudab suurust. Selles dokumendis me keskenduda teisiti aspektide skaleerimist voo Analytics töö arvuti õ funktsioonidega. Mastaapimiseks voo Analytics töö üldiselt kohta leiate artiklist [mastaapimine tööde haldamine](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Mis on Azure seadme õ funktsioonis voo Analytics?

Voo Analytics masina õ funktsiooni saab kasutada nagu tavalise funktsioon kõne voo Analytics päringu keeles. Siiski taga stseeni, funktsioon kõned on tegelikult Azure seadme õ veebiteenuse taotlused. Seadme õ veebiteenuste tugi "partiide" mitu rida, mida nimetatakse mini-paketi, samas web teenuse API kõne ajal parandamiseks üldine jõudlus. Lugege järgmisi artikleid üksikasjalikumat teavet; [Azure'i kohapeal õ funktsioonid voo Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) ja [Azure kohapeal õ veebiteenused](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Voo Analytics töö arvuti õ funktsioonide konfigureerimine

Seadme õ funktsioon voo Analytics töö konfigureerimisel on kaks parameetrite paketi suurus ja seadme õ funktsiooni kõned Streaming ühikute (SUs) ette valmistatud voo Analytics töö pidada. Määratlemiseks väärtused nende esmalt olema otsustada, latentsus ja läbilaskevõime, st latentsus voo Analytics töö ja läbilaskevõime on iga SU. SUs võib alati lisada ka sektsioonitud voo Analytics päringu läbilaskevõime suurendamiseks töö kuigi täiendavad SUs suurendab töö.

Seetõttu on oluline määratleda latentsus voo Analytics töö *hälbe* . Täiendavad latentsus käivitumist Azure seadme õ hooldustaotlused suureneb loomulikult on ühendi töö voo analüüsi latentsus paketi suurus. Teisalt, paketi suuruse suurendamine võimaldab voo Analytics töö töödelda *veel sündmusega on *sama arv * seadme õ web hooldustaotlused.. Sageli suurendamine kohapeal õ web teenuse latentsus on sub lineaarse paketi suuruse suurendamiseks nii, et see on oluline silmas pidada kohapeal õ veebiteenuse antud olukorras kõige suurusega. Web hooldustaotlused paketi vaikesuurust on 1000 ja võib kas kasutades [Voo Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Voo Analytics REST API -ga") või [PowerShelli kliendi voo Analyticsi](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShelli kliendi voo Analyticsi")muuta.

Kui paketi maht on määratud, saab määrata Streaming mõõtühikute (SUs) summa, sündmused, mis funktsioon peab arvu põhjal sekundis protsess. Konsulteerige Streaming üksuste kohta lisateabe saamiseks artiklis [voo Analytics skaala tööde haldamine](stream-analytics-scale-jobs.md#configuring-streaming-units).

Üldiselt on 20 samaaegseid ühendused selle seadme õ veebiteenuse iga 6 SUs peale, et saab ka 1 SU töökohta ja 3 SU töökohta 20 samaaegseid ühendusi.  Kui sisendandmete on sekundis 200 000 sündmused ja paketi maht on jäetud 1000 vaikimisi on tulemuseks web teenuse latentsus 1000 sündmuste mini-paketi 200ms. See tähendab, et iga ühenduse saab teha 5 taotleb seadme õ veebiteenuse teine. 20 ühendused, saab voo Analytics töö protsessi 20 000 sündmuste 200ms ja seetõttu 100000 sündmuste teine. Nii töötlemine 200 000 sündmuste sekundis, voo Analytics töö peab 40 samaaegseid ühendusi, mis väljub 12 SUs. Alloleval joonisel on näidatud taotluste voo Analytics töö kohapeal õ web teenuse lõpp-punkti – iga 6 SUs on 20 samaaegseid ühenduste kohapeal õ veebiteenuse max.

![Skaala voo Analytics masina õ funktsioonide 2 töö näide] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Skaala voo Analytics masina õ funktsioonide 2 töö näide")

Üldiselt ***B*** paketi suurus, ***L*** -paketi suurus B millisekundites, web teenuse latentsus on voo Analytics töökohtade ***N*** SUs läbilaskevõime on:

![Skaala voo Analytics masina Õppekeskuse funktsioonide valemi abil] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Skaala voo Analytics masina Õppekeskuse funktsioonide valemi abil")

Lisaks tuleb võib max samaaegseid kõned masina õ veebis teenuse külg, on soovitatav seda määrata suurima väärtuse (praegu 200).

Lisateavet selle sätte palun vaadake üle [mastaapimine artiklis arvuti õ veebiteenuste jaoks](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Näide – meeleolu analüüs

Järgmises näites sisaldab voo Analytics töökoha meeleolu analüüsi masina õ funktsioon, [voo Analytics masina õ integreerimine õpetuse](stream-analytics-machine-learning-integration-tutorial.md)kirjeldatud.

Päring on lihtne täielikult sektsioonitud päringu funktsiooni **meeleolu** , millele järgneb, nagu allpool näidatud:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Võtke arvesse järgmist stsenaariumi; läbilaskevõime 10 000 tweets sekundis luuakse voo Analytics tööd teha meeleolu analüüsi tweets (sündmused). Kasutades 1 SU, võib selle voo Analytics töö võimalik liiklus käsitlema? Kasutades 1000 paketi vaikesuuruse töö peaks oskama sisend kursis. Täpsemaks lisatud seadme õ funktsiooni tuleb luua üle teise, mis on üldise latentsuse vaikimisi latentsuse meeleolu analüüsi masina õ veebiteenuse (koos vaikimisi suurusega 1000). Voo Analytics töö **Üldine** või lõpuni latentsus oleks tavaliselt paar minutit. Täpsemat teavet arvesse võtta selle voo Analytics töö, *eriti* masina funktsioonide tundmaõppimiseks. Paketi suurus kui 1000, mille läbilaskevõime on 10 000 sündmused võtab umbes 10 taotlusi veebiteenusele. Isegi kui tasute 1 SU, on see Sisestuskeel liiklus mahutamiseks piisavalt samaaegseid ühendusi.

Kuid mida teha, kui Sisestuskeel sündmuse määr suurendatakse 100 x ja voo Analytics töö peab nüüd töötlemine 1 000 000 tweets sekundis? On kaks võimalust.

1.  Paketi suurendamine või
2.  Partition Sisestuskeel voo töödelda sündmusi, samal ajal

Esimene variant suureneb töö **latentsus** .

Teine võimalus, rohkem SUs oleks vaja ette valmistada ja seega luua rohkem samaaegseid masina õ web hooldustaotlused. See tähendab, et töö **maksumus** suureneb.


Endale meeleolu kohapeal õ veebiteenuse analüüsi latentsus on 200ms 1000 – sündmuse töölehed või all, 250ms jaoks 5000 – sündmuse pakettidena, 300ms jaoks 10 000 – sündmuse töölehed või 500 ms pakettidena 25 000-sündmuste jaoks.

1. Esimene variant (**pole** veel SUs ettevalmistamise) kasutamisel võib suurendada paketi suurus **25 000**. See omakorda võimaldab töö töödelda 1 000 000 sündmuste 20 samaaegseid ühendustega masina õ veebiteenusele (koos latentsus 500 ms kõne kohta). Nii täiendavad latentsus voo Analytics töö tõttu meeleolu funktsioon taotlusi vastu masina õ web hooldustaotlused soovite suurendada kaudu **200ms** **500 ms**. Pange tähele, et partii suurust **ei saa** siiski lõputult suurem kui seadme õ veebiteenuste jaoks on vaja last taotluse olema 4 MB või väiksema veebiteenuse taotleb ajalõpp toiming 100 sekundi pärast.
2. Teine suvandi abil paketi maht on jäänud 1000, 200ms web teenuse latentsus, iga 20 samaaegseid ühenduste veebiteenuse oleks võimalik töödelda 1000 *20* 5 sündmused = 100000 sekundis. Et töödelda 1 000 000 sündmuste sekundis, töö oleks vaja 60 SUs. Võrreldes esimene suvand, voo Analytics töö oleks rohkem web paketi hooldustaotlused, omakorda loomisel on suurem eest.

Allpool on tabeli läbilaskevõime voo Analytics töö jaoks eri SUs ja paketi suurused (arvus sündmuste sekundis).

| paketi suurus (ML latentsus)  | 500 (200ms) | 1000 (200ms) | 5000 (250ms) | 10 000 (300ms) | 25 000 (500 ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SU** | 2 500 | 5000 | 20 000 | 30 000 | 50,000 |
| **3 SUs** | 2 500 | 5000 | 20 000 | 30 000 | 50,000 |
| **6 SUs** | 2 500 | 5000 | 20 000 | 30 000 | 50,000 |
| **12 SUs** | 5000 | 10 000 | 40 000 | 60 000 | 100 000 |
| **18 SUs** | 7500 | 15000 | 60 000 | 90 000 | 150 000 |
| **24 SUs** | 10 000 | 20 000 | 80 000 | 120 000 | 200 000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25 000 | 50,000 | 200 000 | 300 000 | 500 000 |

Nüüd peaks teil olema juba hea voo Analytics masina õ funktsioonide kasutamise mõistmine. Saate tõenäoliselt ka aru, et andmeallikast pärinevate andmete voo Analytics töö "pull" ja "pull" iga tagastab paketi voo Analytics töö töödelda sündmusi. Kuidas pull mudeli mõju masina õ veebi hooldustaotlused?

Tavaliselt me kohapeal õ funktsioonide paketi suuruse täpselt ei saa poolt tagastatud iga voo Analytics töö "pull" sündmuste arv. Kui see juhtub kohapeal õ veebiteenuse nimi "osaline" pakettidena olla. Seda tegema ei pea täiendavad töö latentsus kohal eemaldamisviisiks piires pull pull.

## <a name="new-function-related-monitoring-metrics"></a>Uus funktsioon seotud jälgimisega seotud mõõdikud

Voo Analytics töö alal kuvari kolme täiendavad funktsioon seotud mõõdikute lisanud. Need on funktsioon TAOTLUSI, funktsioon sündmused ja nurjus funktsioon TAOTLUSI, nagu on näidatud alloleval pildil.

![Õppekeskuse funktsioonide mõõdikute masina skaala voo Analytics] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Õppekeskuse funktsioonide mõõdikute masina skaala voo Analytics")

Kas määratletud järgmiselt:

**Funktsioon taotlused**: funktsioon taotluste arv.

**Funktsioon sündmused**: funktsioon taotlusi sündmuste arv.

**Funktsioon TAOTLUSI nurjus**: nurjunud funktsioon taotluste arv.

## <a name="key-takeaways"></a>Võtme Takeaways  

Kokkuvõttes põhipunktid, et mastaapimiseks kohapeal õ funktsioonid on voo Analytics töökohtade, tuleb arvestada järgmist:

1.  Sisestuskeel sündmuse määr
2.  Lubatud latentsus töötava voo Analytics töö (ja masina õ web hooldustaotlused paketi suurus)
3.  Ettevalmistatud voo Analytics SUs ja arvu kohapeal õ web hooldustaotlused (täiendavate funktsiooni seotud kulud).

Täielikult sektsioonitud voo Analytics päringu kasutati näide. Keerukama päringu vajadusel [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) on suurepäraseks ressursiks voo Analytics meeskond täiendava abi saamiseks.

## <a name="next-steps"></a>Järgmised sammud

Voo Analytics kohta leiate lisateavet järgmistest teemadest.

- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
