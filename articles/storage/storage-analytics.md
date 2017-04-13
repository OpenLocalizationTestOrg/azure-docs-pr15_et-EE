<properties
    pageTitle="Logide ja mõõdikute andmete kogumise talletusmahu Analytics abil | Microsoft Azure'i"
    description="Salvestusruumi Analytics võimaldab teil jälgida mõõdikute andmete salvestusruumi kõigi teenuste ja koguda bloobimälu, järjekord ja tabel Storage logid."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Salvestusruumi Analytics

## <a name="overview"></a>Ülevaade

Azure'i salvestusruumi Analytics sooritab logimine ja antakse mõõdikute andmete salvestusruumi konto. Andmete abil saate jälgida taotlusi, analüüsida kasutamise ja diagnoosimine probleeme salvestusruumi kontoga.

Salvestusruumi Analytics kasutamiseks tuleb lubada selle eraldi iga teenuse, mida soovite jälgida. Saate lubada [Azure portaali](https://portal.azure.com). Lisateavet leiate teemast [kuvari salvestusruumi konto Azure'i portaalis](storage-monitor-storage-account.md). Samuti saate lubada salvestusruumi Analytics programmiliselt REST API-ga või kliendi teegi kaudu. [Saada bloobimälu atribuutide](https://msdn.microsoft.com/library/hh452239.aspx), [Saada järjekorra atribuutide](https://msdn.microsoft.com/library/hh452243.aspx), [Saada tabeli atribuutide](https://msdn.microsoft.com/library/hh452238.aspx)ja [Saada faili atribuutide](https://msdn.microsoft.com/library/mt427369.aspx) toimingute abil saate lubada salvestusruumi Analytics iga teenuse jaoks.

Kokkuvõtlike andmete talletatakse tuntud bloobimälu (jaoks logimine) ja tuntud tabelid (mõõdikute), mis on kättesaadavad bloobimälu teenuse ja tabeli teenuse API-de abil.

Salvestusruumi Analytics on piiratud 20 TB salvestatud andmeid, mis ei sõltu konto salvestusruum kokku limiit summa. Arveldus- ja andmete säilituspoliitikate kohta leiate lisateavet teemast [salvestusruumi Analytics ja arveldamine](https://msdn.microsoft.com/library/hh360997.aspx). Salvestuslimiitide konto kohta leiate lisateavet teemast [Azure salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md).

Põhjalikud juhised salvestusruumi analüüsi- ja muude tööriistade abil tuvastada, diagnoosimine ja Azure Storage seotud probleemide tõrkeotsing, vt [kuvaris, diagnoosimine, ja Microsoft Azure'i Tabelimäluga tõrkeotsing](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>Salvestusruumi Analytics logimise kohta

Salvestusruumi Analytics logib üksikasjalikku teavet õnnestunud ja nurjunud taotlusi salvestusruumi teenus. Selle teabe saab jälgida üksikuid taotlusi ja probleemide salvestusruumi teenust. Parim – peegeldav alus sisse logitud taotlused.

Logi kirjed on loodud ainult siis, kui salvestusruumi tegevuse. Näiteks kui salvestusruumi konto on tegevuse selle bloobimälu teenuses, kuid mitte selle tabeli või järjekorda teenused, luuakse ainult logid bloobimälu teenusega seotud.

Salvestusruumi Analytics logimine ei ole saadaval Azure'i teenus.

### <a name="logging-authenticated-requests"></a>Logimise autenditud taotlused

Järgmist tüüpi autenditud taotlusi logitakse:

- Eduka taotlused.

- Nurjus taotlusi, sh ajalõpp, pidurdamise, võrku, autoriseerimine ja muud.

- Taotlused on ühiskasutusse antud juurdepääs allkirja (SAS), sh nurjunud ja eduka taotlusi abil.

- Taotluste analytics andmete.

Salvestusruumi Analytics ise, nt Logi loomine või kustutamine, taotlusi on sisse logitud. Teemade [Analytics sisse logitud ja Status Messages](https://msdn.microsoft.com/library/hh343260.aspx) ja [Salvestusruumi Analytics Logi vorming](https://msdn.microsoft.com/library/hh343259.aspx) on dokumenteerida logitud andmeid täieliku loendi.

### <a name="logging-anonymous-requests"></a>Anonüümse taotlusi logimine
Järgmist tüüpi anonüümse taotlusi logitakse:

- Eduka taotlused.

- Serveri tõrkeid.

- Kliendi-ja serveri ajalõpp tõrkeid.

- Nurjunud Saada kutsed tõrkekood 304 (mitte muuta).

Kõik nurjunud anonüümse taotlusi on sisse logitud. Teemade [Analytics sisse logitud ja Status Messages](https://msdn.microsoft.com/library/hh343260.aspx) ja [Salvestusruumi Analytics Logi vorming](https://msdn.microsoft.com/library/hh343259.aspx) on dokumenteerida logitud andmeid täieliku loendi.

### <a name="how-logs-are-stored"></a>Logide salvestamise
Kõik logid on talletatud Blokeeri plekid ümbrises, nimega $logs, mis luuakse automaatselt, kui salvestusruumi Analytics on lubatud salvestusruumi konto. $logs container asub bloobimälu nimeruumi salvestusruumi konto, näiteks: `http://<accountname>.blob.core.windows.net/$logs`. See ümbris ei saa kustutada pärast salvestusruumi Analytics on lubatud, kuigi saate kustutada selle sisu.

>[Azure.NOTE] $logs container ei kuvata, kui toimub loetelu toiming ümbris, nt [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) meetod. See peab otse juurde. Näiteks saate kasutada [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) meetodit juurde pääseda rakenduses plekid on `$logs` ümbrises.
Nagu taotlusi olete sisse logitud, laadige salvestusruumi Analytics vahe tulemuste plokid nimega. Aeg-ajalt salvestusruumi Analytics Kinnita need lahtrid ja kättesaadavaks on bloobimälu.

Duplikaatkirjete võivad esineda logide sama tunni jaoks luua. Saate määrata, kui kirje on duplikaat, märkides ruudu **RequestId** ja **toiming** .

### <a name="log-naming-conventions"></a>Logige nimereeglid
Iga Logi kirjutatakse järgmises vormingus.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Järgmises tabelis kirjeldatakse iga atribuut logi nime.

| Atribuut         | Kirjeldus                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < teenuse nimi >    | Salvestusruumi teenuse nimi. Näide: bloobimälu, tabelis või järjekorda.                                                                                                                          |
| AAAA              | Neljakohalise aastaarvu Logi. Näide: 2011.                                                                                                                                           |
| MM                | Kahe-kohalise kuu Logi. Näide: 07.                                                                                                                                             |
| DD                | Kahe-kohalise kuu Logi. Näide: 07.                                                                                                                                             |
| hh                | Kahekohalise tund, mis näitab, alustades tunni tuleb logide 24-tunnise UTC-vormingus. Näide: 18.                                                                                     |
| mm                | Kahekohalise arv, mis näitab, alustades minutid logide. Salvestusruumi Analytics praegune versioon ei toeta seda väärtust, ja selle väärtus on alati 00.     |
| <counter>         | 0-põhine vastu kuus numbrit, mis näitab, mitu Logi plekid loodud teenuse salvestusruumi tunni aja jooksul. See loendur algab 000000. Näide: 000001.     |

Järgnevalt on lõpule viidud valimi logi nime, mis ühendab eelmistes näidetes.

    blob/2011/07/31/1800/000001.log

Järgnevalt on valimi eelmise Logi juurdepääsemiseks kasutatavate URI.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Salvestusruumi kutse logimisel tulemuseks Logi nimi vastab loendis efektidele tund, kui toiming on lõpule viidud. Näiteks kui GetBlob taotlus on lõpule viidud kell 6:30. 7/31/2011 oleks Logi kirjutada eesliitega järgmist:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Logige metaandmed
Kõik Logi plekid on talletatud metaandmed, mida saab kasutada, mis sisaldab soovitud bloobimälu logimine andmete tuvastamiseks. Järgmises tabelis kirjeldatakse iga metaandmete atribuudi.

| Atribuut     | Kirjeldus                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | Kirjeldab, kas logi sisaldab teavet lugeda, kirjutamiseks või kustutada. Seda väärtust saate lisada ühe või kõigi kolme, komadega kombinatsioon. Näide 1: kirjutada; Näide 2: lugeda, Näide 3: lugeda, kirjutada, kustutada.    |
| StartTime     | Varaseim aeg kirje Logi kujul YYYY-MM-DDThh:mm:ssZ. Näide: 2011-07-31T18:21:46Z.                                                                                                                                             |
| EndTime       | Viimane tähtaeg kirje Logi kujul YYYY-MM-DDThh:mm:ssZ. Näide: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | Logi vormi versioon. Praegu on ainus toetatud väärtus 1.0.                                                                                                                                                                                     |

Järgmises loendis kuvatakse täielik valimi metaandmete eelmistes näidetes abil.

- LogType = kirjutamine

- StartTime = 2011-07-31T18:21:46Z

- EndTime = 2011-07-31T18:22:09Z

- LogVersion = 1.0

### <a name="accessing-logging-data"></a>Juurdepääs andmete logimine

Kõik andmed on `$logs` ümbris pääseb bloobimälu teenuse API-de abil, sh esitatud on Azure .net-i API-de hallatavate teek. Salvestusruumi konto administraator saab lugeda ja logid, kustutamine, kuid ei saa luua ega Värskenda neid. Kui päringu jaoks Logi saab kasutada nii funktsiooni log metaandmete ja logi nime. On võimalik, et antud tund logid vales järjestuses kuvada, kuid metaandmete alati määrab kuuline ajavahemik logi kannete Logi sisse. Seega saate log nimed kombinatsiooni metaandmete kindla Logi otsimisel.

## <a name="about-storage-analytics-metrics"></a>Salvestusruumi Analytics mõõdikute kohta

Salvestusruumi Analytics saate talletada mõõdikute taotluste salvestusruumi teenuse liidetud tehingu statistika ja võimsus andmeid sisaldavad. Tehingud on teatatud API toiming tase ka tasandil salvestusruumi teenuse ja võimsus esitatakse salvestusruumi tasemel. Mõõdikute andmeid saab analüüsida salvestusruumi teenuse kasutuskoha, probleemide diagnoosimine koos taotlusi salvestusruumi teenuse vastu ja rakendusi, mis kasutavad teenuse jõudluse parandamiseks.

Salvestusruumi Analytics kasutamiseks tuleb lubada selle eraldi iga teenuse, mida soovite jälgida. Saate lubada [Azure portaali](https://portal.azure.com). Lisateavet leiate teemast [kuvari salvestusruumi konto Azure'i portaalis](storage-monitor-storage-account.md). Samuti saate lubada salvestusruumi Analytics programmiliselt REST API-ga või kliendi teegi kaudu. **Saada atribuutide** toimingute abil saate lubada salvestusruumi Analytics iga teenuse jaoks.

### <a name="transaction-metrics"></a>Tehingu mõõdikud

Komplekti andmed on salvestatud salvestusteenus iga kord tunnis või minute ajavahemike nõutakse API toiming, sh sissepääsu/sealt, kättesaadavus, tõrgete, ja kategoriseeritud taotluse protsendid. Saate vaadata kande üksikasjad [Salvestusruumi Analytics mõõdikute tabeli skeemi](https://msdn.microsoft.com/library/hh343264.aspx) teemas täieliku loendi.

Tehingu andmed on salvestatud kahel viisil – teenusetaseme ja API toiming tase. Tasemel teenuse statistika kokkuvõttev sõnum kõik nõutud API toimingute kirjutada tabeli üksusele tunnis isegi juhul, kui teenus pole taotlusi tehtud. Tasemel API toiming kirjutada statistika ainult üksusele kui toiming on nõutud selle tunni jooksul.

Näiteks kui teie bloobimälu teenusest **GetBlob** toimingut sooritada, talletusruumi Analytics mõõdikute logida taotlus ja kaasamine kokkuvõtlike andmete nii teenuse bloobimälu kui ka **GetBlob** selle toimingu jaoks. Juhul, kui ei **GetBlob** toiming nõuab tunni jooksul, üksus ei kirjutatakse selle `$MetricsTransactionsBlob` selle toimingu jaoks.

Tehingu mõõdikute salvestatakse nii kasutaja taotlusi ja salvestusruumi Analytics ise taotlused. Taotlusi salvestusruumi Analytics logid ja tabeli üksuste kirjutamiseks on salvestatud. Lisateavet selle kohta, kuidas neid taotlusi on arve, lugege teemat [salvestusruumi Analytics ja arveldamine](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Võimsuse mõõdikud

>[AZURE.NOTE] Praegu on ainult võimsus mõõdikute bloobimälu teenuse jaoks saadaval. Võimsuse mõõdikute tabeli teenuse ja järjekorda teenuse jaoks on saadaval tulevaste versioonide salvestusruumi Analytics.

Võimsuse andmed on salvestatud iga päev salvestusruumi konto bloobimälu teenuse ja kahe tabeli üksuste kirjutada. Ühe üksuse annab statistika kasutaja andmete jaoks, ja teine statistika kohta on `$logs` bloobimälu container salvestusruumi Analytics kasutada. Funktsiooni `$MetricsCapacityBlob` tabel sisaldab statistikud on järgmised:

- **Võimsus**: mäluruumi kasutatavaid salvestusruumi konto bloobimälu teenuse baitides hulk.

- **ContainerCount**: bloobimälu ümbriste salvestusruumi konto bloobimälu teenuses arv.

- **ObjectCount**: arv kinnitatud ja sisseviimata blokeerimine või lehe plekid salvestusruumi konto bloobimälu teenus.

Võimsus mõõdikute kohta leiate lisateavet teemast [Salvestusruumi Analytics mõõdikute tabeli skeemi](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Kuidas salvestatakse mõõdikud

Kõik mõõdikute andmed iga salvestusruumi teenused on talletatud kolme tabeli reserveeritud selle teenuse: ühe tabeli tehingu teavet, minuti tehingu teavet ühest tabelist ja teise tabeli energiavõimsus teavet. Tehing ja minutid tehingu teavet koosneb koosolekukutsete ja kutsele vastamise andmete ja võimsus teave on salvestusruumi kasutuse andmeid. Tund mõõdikute, minute mõõdikute ja võimsus salvestusruumi konto bloobimälu teenuse pääseb tabeli, mis on nimetatud, nagu on kirjeldatud järgmises tabelis.

| Mõõdikute tase                         | Tabelinimed                                                                                                                   | Toetatud versioonid                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Kord tunnis mõõdikute esmane asukoht      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | 2013 – 08-15 ainult versioonides. Kuigi need nimed on endiselt toetatud, on soovitatav vahetate abil alltoodud tabelid.  |
| Kord tunnis mõõdikute esmane asukoht      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Kõik versioonid, sh 2013 – 08-15.                                                                                                               |
| Minute mõõdikute esmane asukoht      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Kõik versioonid, sh 2013 – 08-15.                                                                                                               |
| Kord tunnis mõõdikute teisene asukoht    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Kõik versioonid, sh 2013 – 08-15. Lugemisõigus – geograafilise liigne dispersioonanalüüs peab olema lubatud.                                                    |
| Minute mõõdikute teisene asukoht    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Kõik versioonid, sh 2013 – 08-15. Lugemisõigus – geograafilise liigne dispersioonanalüüs peab olema lubatud.                                                    |
| Võimsus (ainult bloobimälu teenus)          | $MetricsCapacityBlob                                                                                                          | Kõik versioonid, sh 2013 – 08-15.                                                                                                               |


Järgmised tabelid luuakse automaatselt salvestusruumi Analytics lubamisel salvestusruumi konto. Need on kättesaadav nimeruumi salvestusruumi konto kaudu näiteks:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Juurdepääs mõõdikute andmed

Kõik andmed mõõdikute tabelite tabeli teenuse API-de abil pääseb, sh esitatud on Azure .net-i API-de õnnestus teek. Salvestusruumi konto administraator saab lugeda ja tabeli üksuste kustutamine, kuid ei saa luua ega Värskenda neid.

## <a name="billing-for-storage-analytics"></a>Salvestusruumi Analytics arveldus

Salvestusruumi Analytics on lubatud salvestusruumi konto omanik; See on vaikimisi lubatud. Kõik mõõdikute andmed on kirjutatud salvestusruumi konto teenuseid. Seetõttu on tasustatav salvestusruumi Analytics iga kirjutamine tegevus. Lisaks on tasustatav talletusmahtu kasutatavaid mõõdikute andmed.

Salvestusruumi Analytics järgmised toimingud on tasustatav.

- Taotlusi plekid logimiseks loomiseks. 

- Tabeli üksuste jaoks mõõdikute loomiseks taotlused.

Kui olete konfigureerinud andmete säilituspoliitika, saate ei võeta Kustuta tehingute puhul kui salvestusruumi Analytics kustutab vana logimine ja mõõdikute andmeid. Kustuta tehingud klientrakenduses on siiski Tasustatav. Säilituspoliitikate kohta leiate lisateavet teemast [salvestusruumi Analytics andmete säilituspoliitika seadmine](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Arveldatavate taotlusi mõistmine

Iga konto salvestusteenus taotlustest on tasustatav või mitte arveldatavate. Salvestusruumi Analytics logib iga üksiku taotlus teenus, sealhulgas oleku teade, mis näitab taotluse käsitlemise viisi. Samuti salvestusruumi Analytics salvestab mõõdikute nii teenuse ja teenuse, sh protsentide ja teatud oleku sõnumite API toimingud. Koos need funktsioonid aitavad teil analüüsida oma arveldatavate taotlusi, paremaks oma taotlus ja diagnoosimine taotluste teenuste probleeme. Arveldamine kohta leiate lisateavet teemast [Mõistmine Azure'i salvestusruumi arveldus - ribalaiuse, tehingute ja võimsus](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Vaadates salvestusruumi Analytics andmete, saate tabelite [Analytics sisse logitud ja Status Messages](https://msdn.microsoft.com/library/azure/hh343260.aspx) teemas teha kindlaks, millised taotlused on tasustatav. Seejärel saate võrrelda oma logid ja mõõdikute oleku sõnumite kuvamiseks, kui teie eest tasu võeti kindla taotluse andmeid. Samuti saate tabeleid eelmises uurida kättesaadavus salvestusteenus või API toimingu.

## <a name="next-steps"></a>Järgmised sammud

### <a name="setting-up-storage-analytics"></a>Salvestusruumi Analytics seadistamine
- [Salvestusruumi konto Azure'i portaalis jälgimine](storage-monitor-storage-account.md)
- [Lubamine ja konfigureerimine salvestusruumi Analytics](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Salvestusruumi Analytics logimine  
- [Salvestusruumi Analytics logimise kohta](https://msdn.microsoft.com/library/hh343262.aspx)
- [Salvestusruumi Analytics Logi vorming](https://msdn.microsoft.com/library/hh343259.aspx)
- [Salvestusruumi Analytics logitud toimingute ja olekuteade](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Salvestusruumi Analytics mõõdikud
- [Talletusruumi Analytics mõõdikute kohta](https://msdn.microsoft.com/library/hh343258.aspx)
- [Salvestusruumi Analytics mõõdikute tabeli skeemi](https://msdn.microsoft.com/library/hh343264.aspx)
- [Salvestusruumi Analytics logitud toimingute ja olekuteade](https://msdn.microsoft.com/library/hh343260.aspx)  
