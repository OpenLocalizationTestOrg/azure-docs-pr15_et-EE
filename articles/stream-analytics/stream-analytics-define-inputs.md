<properties
    pageTitle="Andmeühenduse: andmete voona sisendit voogu sündmuse | Microsoft Azure'i"
    description="Lisateavet andmeühenduse voo Analytics nimega 'sisendina' häälestamise kohta. Sisendina kaasa andmed voo sündmused ja viidata ka andmed."
    keywords="andmevoos, andmeühendust, sündmuse voo"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Andmeühenduse: teave andmete voo sisendit sündmused voo Analytics

Voo Analytics andmete ühendus on andmevoos sündmuste andmeallikast. Seda nimetatakse "sisend." Voo Analytics on esimese klassi integreerimine Azure andmete voo allikad sündmuse jaoturi ning asjade jaoturi bloobimälu salvestamise, mis võib olla samas või mõnes muus Azure tellimuselt analytics oma tööd.

## <a name="data-input-types-data-stream-and-reference-data"></a>Andmete sisend tüüpi: andmed voo- ja viitamisfunktsioonid
Nagu andmeallika filtreerima tarbitud voo Analytics töö ja reaalajas töödeldud. Sisendeid on kaks erinevat tüüpi jagatud: andmete voona sisendina ja andmete sisendina viitamine.

### <a name="data-stream-inputs"></a>Andmete voo sisendina
Andmete voo on ääretu jada sündmustest jpm aja jooksul. Voo Analytics töö peab sisaldama vähemalt ühte andmesisestuse voo consumed ja kujundab töö. Sündmuse jaoturi bloobimälu ning asjade jaoturi on toetatud andmeallikate voo Sisestuskeel nimega. Sündmuse jaoturi kasutatakse koguda sündmuse voogu mitme seadmed ja teenuseid, näiteks sotsiaalmeedia tegevuste kanalid, börsidiagrammi teavet või andmeid anduritelt. Andmete kogumine ühendatud seadmete Internet, asjade stsenaariumide on optimeeritud asjade jaoturi.  Bloobimälu saab kasutada Sisestuskeel allikas söömisega voo hulgi andmed.  

### <a name="reference-data"></a>Lähteandmed
Voo Analytics toetab teist tüüpi lähteandmed tuntud sisendit. See on abistava andmeid, mis on staatiline või aeglaselt muutmisega aja jooksul ja tavaliselt kasutatakse korrelatsiooni ja look-ups. Azure'i bloobimälu pole praegu toetatud Sisestuskeel ainuke viide andmete jaoks. Viide andmete allikas plekid on piiratud suuruses 100MB.
Viide andmete sisendina loomise kohta leiate teemast [Kasutamine lähteandmed](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Andmete voo sisestamine abil soovitud sündmus keskuse loomine

[Azure'i sündmus jaoturi](https://azure.microsoft.com/services/event-hubs/) on väga paindlik sündmus ingestor avaldada-Telli. Sündmuste sekundis, miljonid saab koguda nii, et saate töötlemine ja analüüsida suurel hulgal ühendatud seadmete ja rakenduste andmed. See on üks kõige sagedamini kasutatav sisendeid voo Analytics. Sündmuse jaoturi ja voo Analytics koos anda kliendid lõpuni lahendus reaalajas analytics. Sündmuse jaoturi luba klientide sündmuste siseneda Azure'i reaalajas ja voo Analytics töö saab reaalajas protsessi. Näiteks saate kliendid saata sündmuse jaoturi web hiireklõpsuga, andur lugemist, online sündmused ja luua voo Analytics töökohta kasutamiseks sündmuse jaoturi Sisestuskeel andmevoogu reaalajas filtreerimiseks, liitmise ja korrelatsiooni.

See on oluline Pange tähele, et vaikimisi ajatempli sündmuste pärit sündmuse jaoturi voo Analytics ajatempli, mis saabusid sündmus, sündmuse keskuses, mis on EventEnqueuedUtcTime. Nimega voo sündmus last ajatempel kasutamine andmete töötlemiseks, tuleb kasutada märksõna [AJATEMPLI järgi](https://msdn.microsoft.com/library/azure/dn834998.aspx) .

### <a name="consumer-groups"></a>Rühmi

Iga voo Analytics sündmus jaoturi sisendit peaks olema konfigureeritud olema oma tarbija rühm. Kui töö sisaldab iseendaga ühendamine või mitut sisendit, võib-olla rohkem kui üks lugejale, mis mõjutab lugejate ühe tarbija jaotises arv tagasisidet lugeda. Sündmuse jaoturi arvu 5 lugejad tarbija rühma kohta sektsiooni kohta ületamise vältimiseks on parim tarbija rühma iga voo Analytics töö määrata. Pange tähele, et ka kuni 20 rühmi sündmuse keskuse kohta. Lisateavet leiate teemast [Sündmus jaoturi programmeerimisega juhend](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-event-hub-as-an-input-data-stream"></a>Kui sisendandmete voogu sündmuse jaoturi konfigureerimine

Järgmises tabelis kirjeldatakse iga atribuudi juhul, kui keskuse sisestusteade menüüd ja selle kirjeldus:

| ATRIBUUDI NIMI | KIRJELDUS |
|------|------|
| Sisestuskeel alias (pseudonüüm) | Sõbralik nimi, mida kasutatakse töö päringu aluseks järgmist sisestusviisi |
| Teenuse siini Namespace | Teenuse siini nimeruum on ümbris kogum, sõnumside üksuste jaoks. Kui olete loonud uue sündmuse jaoturi, loodud ka teenuse siini nimeruum. |
| Sündmuse jaoturi | Oma sündmuse jaoturi sisestusmeetodi nimi. |
| Sündmuse jaoturi poliitika nimi | Ühiskasutusega juurdepääsu poliitika, mis saab luua menüü sündmus jaoturi konfigureerimine. Iga ühiskasutusega juurdepääsu poliitika on nimi, määratavad, õigused ja kiirklahvide. |
| Sündmuse jaoturi poliitika võti | Ühiskasutuses kiirklahv juurdepääsu teenuse siini nimeruumi autentimiseks kasutada. |
| Sündmuse jaoturi tarbija rühma (valikuline) | Tarbija rühma neelata andmete sündmuse keskuse kaudu. Kui pole määratud, kasutage voo Analytics töö vaikimisi tarbija rühma neelata andmete sündmuse keskuse kaudu. Soovitatav on kasutada erinevate tarbija rühma iga voo Analytics töö. |
| Sündmuse serialiseerimisvormingus | Veendumaks, et teie päringud töötavad eeldatult, voo Analytics on vaja teada, millised serialiseerimisvormingus (JSON, CSV- või Avro) kasutate sissetulevate andmevoogu. |
| Kodeerimine | UTF-8 on ainus toetatud kodeerimise vorming sel ajal. |

Kui teie andmed on sündmuse jaoturi allikast, võite tutvuda mõned metaandmete väljad voo Analytics päringus. Järgmises tabelis on loetletud väljad ja nende kirjeldused.

| ATRIBUUT | KIRJELDUS |
|------|------|
| EventProcessedUtcTime | Kuupäev ja kellaaeg, mille sündmuse voo Analytics töötlemist. |
| EventEnqueuedUtcTime | Kuupäev ja kellaaeg, mille sündmuse võttis sündmuse jaoturi. |
| PartitionId | 0-põhine sektsiooni ID Sisestuskeel adapterit. |

Näiteks võib kirjutada päringu umbes selline:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Asjade jaoturi andmesisestuse voo loomine

Azure'i asjade jaoturi on väga paindlik avaldada-Telli sündmuse ingestor asjade stsenaariumid jaoks optimeeritud.
See on oluline Pange tähele, et vaikimisi ajatempli sündmuste pärit asjade jaoturi voo Analytics ajatempli, mis saabusid sündmus, asjade keskuses, mis on EventEnqueuedUtcTime. Nimega voo sündmuse last ajatempel kasutamine andmete töötlemiseks, tuleb kasutada märksõna [AJATEMPLI järgi](https://msdn.microsoft.com/library/azure/dn834998.aspx) .

> [AZURE.NOTE] Ainult DeviceClient atribuut on saadetud sõnumid saab töödelda.

### <a name="consumer-groups"></a>Rühmi

Iga voo Analytics asjade jaoturi sisendit peaks olema konfigureeritud olema oma tarbija rühm. Kui töö sisaldab iseendaga ühendamine või mitut sisendit, võib-olla rohkem kui üks lugejale, mis mõjutab lugejate ühe tarbija jaotises arv tagasisidet lugeda. 5 lugejad tarbija rühma kohta sektsiooni kohta asjade jaoturi piirmäära ületamise vältimiseks on parim tarbija rühma iga voo Analytics töö määrata.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>Konfigureerida andmete voogu sisestusteade asjade jaoturi

Järgmises tabelis kirjeldatakse iga atribuudi asjade jaoturi Sisestuskeel menüüs ja selle kirjeldus:

| ATRIBUUDI NIMI | KIRJELDUS |
|------|------|
| Sisestuskeel alias (pseudonüüm) | Sõbralik nimi, mida kasutatakse töö päringu aluseks järgmist sisestusviisi. |
| Asjade jaoturi | Soovitud asjade on määratud sõnumside üksuste ümbris. |
| Lõpp-punkti | Oma asjade jaoturi lõpp-punkti nimi. |
| Ühiskasutusega juurdepääsu poliitika nimi | Ühiskasutusega juurdepääsu poliitika asjade jaoturi juurdepääsu andmiseks. Iga ühiskasutusega juurdepääsu poliitika on nimi, määratavad, õigused ja kiirklahvide. |
| Ühiskasutusega poliitika kiirklahv | Ühiskasutuses kiirklahv asjade jaoturi juurdepääsu autentimiseks kasutada. |
| Tarbija rühma (valikuline) | Tarbija rühma neelata andmete asjade keskuse kaudu. Kui pole määratud, kasutatakse voo Analytics töö vaikimisi tarbija rühma neelata andmete asjade keskuse kaudu. Soovitatav on kasutada erinevate tarbija rühma iga voo Analytics töö. |
| Sündmuse serialiseerimisvormingus | Veendumaks, et teie päringud töötavad eeldatult, voo Analytics on vaja teada, millised serialiseerimisvormingus (JSON, CSV- või Avro) kasutate sissetulevate andmevoogu. |
| Kodeerimine | UTF-8 on ainus toetatud kodeerimise vorming sel ajal. |

Kui teie andmed on asjade jaoturi allikast, võite tutvuda mõned metaandmete väljad voo Analytics päringus. Järgmises tabelis on loetletud väljad ja nende kirjeldused.

| ATRIBUUT | KIRJELDUS |
|------|------|
| EventProcessedUtcTime | Kuupäev ja kellaaeg, mille sündmuse töötlemist. |
| EventEnqueuedUtcTime | Kuupäev ja kellaaeg, mille sündmus võttis asjade keskuse. |
| PartitionId | 0-põhine sektsiooni ID Sisestuskeel adapterit. |
| IoTHub.MessageId | Teabevahetus asjade keskuses kasutada. |
| IoTHub.CorrelationId | Kasutada sõnumi vastused ja tagasiside asjade keskuses. |
| IoTHub.ConnectionDeviceId | Autenditud ID-d kasutatakse seda sõnumit saata, tempel servicebound sõnumitele asjade jaoturi abil. |
| IoTHub.ConnectionDeviceGenerationId | Selle sõnumi saatmiseks autenditud seadme generationId tempel servicebound sõnumitele asjade jaoturi abil. |
| IoTHub.EnqueuedTime | Kui sõnumi võttis asjade jaoturi aeg. |
| IoTHub.StreamId | Kohandatud sündmus atribuudi lisanud saatja seadmete lõikes. |

## <a name="create-a-blob-storage-data-stream-input"></a>Looge bloobimälu salvestusruumi andmete voo sisestamine

Stsenaariumid suurte struktureerimata andmed talletatakse pilves, pakub bloobimälu tulusate ja scalable lahendust. Andmete [bloobimälu](https://azure.microsoft.com/services/storage/blobs/) peetakse üldiselt ülejäänud"andmed", kuid seda saab töödelda nimega andmete voo voo Analytics. Üks stsenaarium bloobimälu salvestusruumi sisendeid voo Analytics on töötlemine, kus telemeetria on registreerimise süsteemist ning tuleb sõeluda ja mõtestatud andmete eraldamiseks töödeldud.

See on oluline Pange tähele, et vaikimisi ajatemplit bloobimälu salvestusruumi sündmuse voo Analytics viimati muudeti selle bloobimälu mis *isBlobLastModifiedUtcTime*ajatemplit. Nimega voo sündmus last ajatempel kasutamine andmete töötlemiseks, tuleb kasutada märksõna [AJATEMPLI järgi](https://msdn.microsoft.com/library/azure/dn834998.aspx) .

Samuti võtke arvesse, et CSV vormindatud sisendina **nõua** määratleda väljad andmehulga päiserida. Edasise päise reaväljade peab kõik olema **kordumatu**.

> [AZURE.NOTE] Voo Analytics ei toeta mõne olemasoleva bloobimälu lisamise sisu. Üks kord ja kõik muudatused teinud pärast seda lugeda ei töödelda ainult vaadata voo Analytics on bloobimälu. Parim on üles laadida kõik andmed üks kord ja ei saa lisada mis tahes täiendavaid sündmuste bloobimälu pood.

Järgmises tabelis kirjeldatakse iga atribuudi bloobimälu salvestusruumi Sisestuskeel menüüs ja selle kirjeldus:

<table>
<tbody>
<tr>
<td>ATRIBUUDI NIMI</td>
<td>KIRJELDUS</td>
</tr>
<tr>
<td>Sisestuskeel alias (pseudonüüm)</td>
<td>Sõbralik nimi, mida kasutatakse töö päringu aluseks järgmist sisestusviisi.</td>
</tr>
<tr>
<td>Salvestusruumi konto</td>
<td>Kus asub bloobimälu failide salvestusruumi konto nimi.</td>
</tr>
<tr>
<td>Salvestusruumi konto võti</td>
<td>Salajane võti salvestusruumi kontoga seotud.</td>
</tr>
<tr>
<td>Salvestusruumi Container
</td>
<td>Ümbriste pakuvad loogiline rühmitamine plekid salvestatud Microsoft Azure'i bloobimälu teenus. Kui laadite üles soovitud bloobimälu bloobimälu teenusega, määrake see bloobimälu ümbris.</td>
</tr>
<tr>
<td>Tee eesliite muster [valikulised]</td>
<td>Faili tee teie määratud pakendi sees plekid otsimiseks kasutada.
Path, võite määrata ühe või mitme eksemplari järgmiste muutujate 3:<BR>{date}, {time}<BR>{partition}<BR>Näide 1: cluster1/logid / {date} / {time} / {partition}<BR>Näide 2: cluster1/logid / {date}<P>Pange tähele, et "*" ei ole lubatud väärtus pathprefix jaoks. Ainult lubatud <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure'i bloobimälu märgid</a> on lubatud.</td>
</tr>
<tr>
<td>Kuupäevavormingu [valikulised]</td>
<td>Kui eesliide tee kasutatakse kuupäeva luba, saate valida, kus on korraldatud failide kuupäevavormingu. Näide: PP/AAAA/MM</td>
</tr>
<tr>
<td>Kellaajavorming [valikulised]</td>
<td>Kui aega luba kasutatakse eesliite tee, määrata failide korraldust kellaajavormingu. Praegu on ainus toetatud väärtus HH.</td>
</tr>
<tr>
<td>Sündmuse serialiseerimisvormingus</td>
<td>Veendumaks, et teie päringud töötavad eeldatult, voo Analytics on vaja teada, millised serialiseerimisvormingus (JSON, CSV- või Avro) kasutate sissetulevate andmevoogu.</td>
</tr>
<tr>
<td>Kodeerimine</td>
<td>CSV-ja JSON UTF-8 on ainus toetatud kodeerimise vorming sel ajal.</td>
</tr>
<tr>
<td>Eraldaja</td>
<td>Voo Analytics toetab paljusid levinud eraldajaid jaotamine andmete CSV-vormingus. Toetatud väärtused on koma, semikoolon, ruumi, menüüd ja vertikaaljoon.</td>
</tr>
</tbody>
</table>

Kui teie andmed on pärit bloobimälu salvestusruumi allika, pääsete mõned metaandmete väljad voo Analytics päringus. Järgmises tabelis on loetletud väljad ja nende kirjeldused.

| ATRIBUUT | KIRJELDUS |
|------|------|
| BlobName | Sisestuskeel bloobimälu, kus sündmus pärineb nimi. |
| EventProcessedUtcTime | Kuupäev ja kellaaeg, mille sündmuse voo Analytics töötlemist. |
| BlobLastModifiedUtcTime | Kuupäev ja kellaaeg viimati muudeti selle bloobimälu. |
| PartitionId | 0-põhine sektsiooni ID Sisestuskeel adapterit. |

Näiteks võib kirjutada päringu umbes selline:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud
Olete õppinud andmete ühendussuvandid Azure voo Analytics oma töö jaoks. Voo Analytics kohta leiate lisateavet järgmistest teemadest.

- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
