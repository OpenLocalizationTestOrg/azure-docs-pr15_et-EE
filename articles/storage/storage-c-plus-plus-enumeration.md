<properties
    pageTitle="Loendi Azure Storage ressursid Microsoft Azure'i salvestusruumi kliendi teegis c++ | Microsoft Azure'i"
    description="Saate teada, kuidas loendada ümbriste, plekid, järjekorrad, tabelite ja üksuste Microsoft Azure'i salvestusruumi kliendi teek C++ kirjet API-de kasutamine."
    documentationCenter=".net"
    services="storage"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="list-azure-storage-resources-in-c"></a>Loendi Azure Storage ressursid c++

Loendi toimingud on palju arengu stsenaariumid Azure Storage. Selles artiklis kirjeldatakse, kuidas kõige tõhusamalt loendada objektide Azure Storage API-de jaoks C++ teegis Microsoft Azure'i salvestusruumi kliendi esitatud kirjet abil.

>[AZURE.NOTE] Sellest juhendist suunatud Azure salvestusruumi kliendi teek C++ versiooni 2.x, mis on saadaval [Nugeti](http://www.nuget.org/packages/wastorage) või [GitHub](https://github.com/Azure/azure-storage-cpp)kaudu.

Salvestusruumi kliendi teek pakub mitmesuguseid võimalusi Azure Storage objektide loendi või päringu. Selles artiklis käsitletakse järgmisi toiminguid:

-   Loendi ümbriste kontol
-   Loendi plekid container või virtuaalse bloobimälu kataloog
-   Loendi järjekorrad konto
-   Konto loend tabelid
-   Päringu üksuste tabelis

Eri viiside on kuvatud erinevate ülekoormuse kasutamise erinevaid stsenaariumeid lähemalt.

## <a name="asynchronous-versus-synchronous"></a>Sünkroonse või asünkroonse

Kuna c++ salvestusruumi kliendi teek on ehitatud [C++ ülejäänud teeki](https://github.com/Microsoft/cpprestsdk), toetame potentsiaalselt asünkroonsete toimingute abil [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Näiteks:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Toimingute sünkroonse Murra vastavate asünkroonsete toimingute:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Kui te töötate mitme väliskeermestamiseks rakenduste või teenustega, soovitame kasutada helistamiseks sünkroonimine API-d, mis märkimisväärselt mõjutab jõudluse otse teemat loomise asemel asünkroonse API-d.

## <a name="segmented-listing"></a>Segmenditud kirjet

Salvestusruumi skaala nõuab segmenditud kirjet. Näiteks saate määrata üle miljoni plekid on Azure'i bloobimälu ümbrises või miljardit üksuste Azure'i tabelist. Need pole teoreetilised arvud, kuid tegelik klient kasutus juhtudel.

Seetõttu on otstarbekas ühe vastuse kõigi objektide loendi. Selle asemel saate loendis Piipar objekte. Iga kirjet API-d on soovitud *segmenditud* ülekoormuse.

Segmenditud kirjet toimingu jaoks vastuse sisaldab järgmist:

-   <i>_segment</i>, mis sisaldab kirjet API ühe kõne tagastatud tulemuste komplekti.
-   *continuation_token*, mis on edasi järgmisele kõne järgmisel lehel tulemuste saamiseks. Kui see pole veel tulemeid tagastama, jätkamiseks luba on tühi.

Näiteks tüüpiline kõne loendis kõik plekid nõus võib tunduda järgmised koodilõigu. Kood on saadaval meie [näidised](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Pange tähele, et lehele tagastatud tulemite arv saab kontrollida selle parameetri *max_results* iga API ülekoormuse näiteks:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Kui määrate parameetri *max_results* , tagastatakse vaikimisi suurima väärtuse kuni 5000 tulemuste ühele lehele.

Samuti võtke arvesse, et päring Azure'i tabelimälu võib tagastada pole, või vähem kirjete *max_results* parameeter, mis on määratud, kui ka siis, kui jätkamiseks luba pole tühi. Üheks põhjuseks võib olla, et päring ei saanud lõpule viia viis sekundit. Kui jätkamiseks luba pole tühi, päringu jätkama ja oma kood tuleks vastuta lõigu tulemuste suurust.

Soovitatav kodeerimine mustri enamik stsenaariumid on segmenditud loetelu, mis annab konkreetsete edenemist kirjet või päringud ja kuidas teenuse vastab iga taotluse. Eriti C++ rakenduste või teenuste, madalama taseme juhtimise kirjet edenemist võib aidata juhtelemendi mälu ja jõudluse.

## <a name="greedy-listing"></a>Ahne kirjet

Varasemate versioonide salvestusruumi kliendi teek c++ (versioonid 0.5.0 toetama eelvaade ja varem) sisalduv segmenditud kirjet API-de tabelid ja järjekordade, nagu järgmises näites:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Segmenditud API-de ümbriste olid rakendada järgmised võimalused. Iga vastuse segmenditud loendikirjet, koodi lisatud tulemused on vektorkuju ja kõik tulemused tagastatakse pärast täielik ümbriste skannimise.

Seda moodust võib töötada, kui salvestusruumi konto või tabel sisaldab väheste objektid. Siiski objektide arvu suurenemine mälu on vaja võib suurendada ilma piiranguteta, kuna kõik tulemused jäid mällu. Ühe loendi toiming saavad võtta väga kaua aega, mil helistaja puudus edenemisega teave.

Nende ahne kirjet SDK API-d ei ole C#, Java või JavaScripti Node.js keskkonnas. Võimalikud probleemid, mis neid ahne API-de kasutamise vältimiseks me eemaldada versioon 0.6.0 eelvaade.

Kui teie kood helistab need ahne API:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Seejärel muutke oma koodi segmenditud kirjet API-de kasutamine:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Lõigu *max_results* parameetri määramisega saate saldo taotluste arvud ja mälukasutuse täita rakenduse kohta.

Lisaks, kui kasutate segmenditud kirjet API-d, kuid andmed salvestada "kuus" laadiga kohaliku kogumi, ka soovitame, et te refaktoorime oma koodi käsitlema andmete talletamine kohaliku saidikogumi hoolikalt tasandil.

## <a name="lazy-listing"></a>Rongile kirjet

Kuigi ahne kirjet astmes võimalikke probleeme, on mugav juhul, kui pole liiga palju objekte ümbrises.

Kui te kasutate C# või Oracle Java SDK-d, tuleb teil tuttav sihttüüp programmeerimise mudelit, mis pakub rongile-laadi loetelu, kus on teatud nihke andmete ainult tõmmatud kui see on nõutav. C++, pakub Mall iteraatori vastavalt ka sarnast lähenemisviisi.

Tüüpilised rongile kirjet API, kasutades **list_blobs** , nagu näiteks näeb välja umbes järgmine:

    list_blob_item_iterator list_blobs() const;

Tüüpilised koodilõigu, mis kasutab rongile kirjet mustri võib välja nägema selline:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Pange tähele, et rongile kirjet on sünkroonse režiimis saadaval ainult.

Võrreldes ahne kirjet, rongile loetelu tõmbab andmed ainult siis, kui see on vajalik. All hõlmab, see toob andmed: Azure'i salvestusruumi, ainult siis, kui järgmise iteraatori liigub järgmise lõigu. Seetõttu mälukasutuse juhitakse piiratud suurus ja toiming on kiire.

Rongile kirjet API-de sisalduvad versioon 2.2.0 c++ salvestusruumi kliendi teek.

## <a name="conclusion"></a>Kokkuvõte

Selles artiklis me arutada erinevate ülekoormuse loetelu API-de objektide salvestusruumi kliendi teegis c++ jaoks. Kokkuvõte

-   Asünkroonse API-de soovitame alusel mitu väliskeermestamiseks stsenaariumi.
-   Enamiku stsenaariumide jaoks on soovitatav segmenditud kirjet.
-   Rongile kirjet on mugav ümbris sünkroonse stsenaariumide teegis saadaval.
-   Ahne kirjet ei ole soovitatav ja teek on eemaldatud.

##<a name="next-steps"></a>Järgmised sammud

Teemast Azure Storage ja kliendi teek c++ leiate lisateavet järgmistest teemadest.

-   [Kuidas kasutada bloobimälu C++:](storage-c-plus-plus-how-to-use-blobs.md)
-   [Kuidas kasutada Tabelimälu C++:](storage-c-plus-plus-how-to-use-tables.md)
-   [Kuidas kasutada järjekorda salvestusruumi C++:](storage-c-plus-plus-how-to-use-queues.md)
-   [Azure'i salvestusruumi kliendi teek C++ API dokumentatsiooni.](http://azure.github.io/azure-storage-cpp/)
-   [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)
