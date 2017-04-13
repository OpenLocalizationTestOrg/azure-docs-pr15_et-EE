<properties
    pageTitle="Voo Analytics andmete ja lookup tabelid kasutamine | Microsoft Azure'i"
    description="Viide voo Analytics päringu andmete kasutamine"
    keywords="otsingutabel lähteandmed"
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

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Voo Analytics Sisestuskeel voo viide andmete või otsingu tabelite abil

Viide andmed (tuntud ka kui otsingutabel) on piiratud andmehulga, mis on staatiline või aeglustavad laadi, muutes kasutada otsingu või vastab teie andmevoos. Saate oma töösse Azure'i voo Analytics andmete kasutada tavaliselt kasutatakse [Viide andmete liitumine](https://msdn.microsoft.com/library/azure/dn949258.aspx) päringus. Voo Analytics kasutab Azure'i bloobimälu salvestusruumi kiht lähteandmed ja Azure'i andmed Factory viitega andmete saate olla ümber ja/või kopeeritud Azure'i bloobimälu andmeid kasutada [mis tahes arv pilve vastavalt ja kohapealsete andmete](../data-factory/data-factory-data-movement-activities.md). Viide andmete vastendamise plekid tõusvas järjestuses, kuupäev/kellaaeg määratud bloobimälu nimi (määratletud Sisestuskeel konfiguratsiooni) jada. See toetab **ainult** lisamine lõpus jada abil kuupäeva/kellaaja **suurem** kui määratud viimase bloobimälu järjekorras.

## <a name="configuring-reference-data"></a>Viide andmete konfigureerimine

Viide andmete konfigureerimiseks peate esmalt sisendit, mis tüüpi **Lähteandmed**on loomine. Järgmises tabelis kirjeldatakse iga atribuut, mida teil on vaja loomisel viide andmeid sisestada ja selle kirjeldus:

<table>
<tbody>
<tr>
<td>Atribuudi nimi</td>
<td>Kirjeldus</td>
</tr>
<tr>
<td>Sisestuskeel alias (pseudonüüm)</td>
<td>Sõbralik nimi, mida kasutatakse töö päringu aluseks järgmist sisestusviisi.</td>
</tr>
<tr>
<td>Salvestusruumi konto</td>
<td>Kus asub bloobimälu failide salvestusruumi konto nimi. Kui see on sama nimega tööpäevad voo Analytics tellimuse, saate selle rippmenüüst alla.</td>
</tr>
<tr>
<td>Salvestusruumi konto võti</td>
<td>Salajane võti salvestusruumi kontoga seotud. See toob täidetakse automaatselt, kui salvestusruumi konto on sama tellimuse voo Analytics oma tööd.</td>
</tr>
<tr>
<td>Salvestusruumi Container</td>
<td>Ümbriste pakuvad loogiline rühmitamine plekid salvestatud Microsoft Azure'i bloobimälu teenus. Kui laadite üles soovitud bloobimälu bloobimälu teenusega, määrake see bloobimälu ümbris.</td>
</tr>
<tr>
<td>Tee mustri</td>
<td>Faili tee teie määratud pakendi sees plekid otsimiseks kasutada. Path, võite määrata ühe või mitme eksemplari järgmiste muutujate 2:<BR>{date} {aeg}<BR>Näide 1: products/{date}/{time}/product-list.csv<BR>Näide 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Kuupäevavormingu [valikulised]</td>
<td>Kui olete kasutanud {date} tee mustri teie määratud sees, siis saate valida failide korraldust kuupäevavormingu rippmenüüst toetatud vormingus. Näide: PP/AAAA/MM</td>
</tr>
<tr>
<td>Kellaajavorming [valikulised]</td>
<td>Kui olete kasutanud {aeg} tee mustri teie määratud sees, siis saate valida, kus on korraldatud failide kellaajavormingu rippmenüüst toetatud vormingus. Näide: HH</td>
</tr>
<tr>
<td>Sündmuse serialiseerimisvormingus</td>
<td>Veendumaks, et teie päringud töötavad eeldatult, voo Analytics on vaja teada, millised serialiseerimisvormingus kasutate sissetulevate andmevoogu. Andmeid on toetatud vormingud CSV- ja JSON.</td>
</tr>
<tr>
<td>Kodeerimine</td>
<td>UTF-8 on ainus toetatud kodeerimise vorming sel ajal</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Viide andmete ajakava loomisel

Kui teie viide andmed on aeglaselt muutuvate andmete kogum, siis toeta värskendamise viide andmed on lubatud, määrates tee mustri Sisestuskeel konfiguratsiooni abil {date} ja {ajal} asendus märgid. Voo Analytics valivad selle tee mustri põhjal värskendatud viide andmete määratlusi. Näiteks muster `sample/{date}/{time}/products.csv` kuupäeva vormingus **"YYYY-MM-DD"** ja kellaaja vormingut **"Hh: mm"** juhendab voo Analytics hooleks värskendatud bloobimälu `sample/2015-04-16/17:30/products.csv` juures 5:30 PM aprill 16 2015 UTC ajavööndi määramine.

> [AZURE.NOTE] Praegu voo Analytics töö otsige bloobimälu Värskenda ainult siis, kui seadme kellaaeg ettemaksete bloobimälu nimi kodeeritud aega. Näiteks otsib töö `sample/2015-04-16/17:30/products.csv` niipea, kui võimalik, kuid mitte varem kui 5:30 PM aprill 16 2015 UTC aega tsooni. See on *kunagi* Vaata faili encoded aeg enne viimast, mis on avastatud.
> 
> Nt pärast töö leiab soovitud bloobimälu `sample/2015-04-16/17:30/products.csv` ignoreerib seda kõik failid on encoded varasema kuupäevaga 5:30 PM 16 aprill 2015 nii kui hilinenud saabuvad `sample/2015-04-16/17:25/products.csv` bloobimälu saab loodud sama ümbrises töö ei kasuta seda.
> 
> Samuti kui `sample/2015-04-16/17:30/products.csv` on toodeti ainult kell 10:03 16 aprill 2015, kuid pole bloobimälu koos varasema kuupäeva esineb ümbris, töö on kasutada seda faili, alustades kell 10:03 16 aprill 2015 ja eelmine viide andmete seni.
> 
> Erandi käivitatakse kui töö vaja uuesti töötlemine andmete ajas tagasi või kui töö on esimene. Töö otsib viimase bloobimälu, töö enne toodetud käivitamise ajal alguskellaaeg määratud. Seda tehakse veenduge, et töö käivitamisel on **tühi** viide andmehulgas. Kui ei leita, töö kuvatakse järgmised diagnostika: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure'i andmed Factory](https://azure.microsoft.com/documentation/services/data-factory/) saab korraldab ülesande värskendatud plekid määratlused viide andmete värskendamiseks voo Analytics vaja luua. Andmete Factory on pilvepõhist andmete integreerimine teenus, mis orchestrates ja automatiseerib liikumine ja andmete teisendus. Andmete Factory toetab [ühendamisel suure hulga pilvepõhine ja kohapealse andmete poed](../data-factory/data-factory-data-movement-activities.md) ja jooksva andmeid hõlpsalt teie määratud regulaarse intervalliga. Lisateavet ja samm-sammult juhised selle kohta, kuidas häälestada andmete Factory kohaletoimetamisel viide andmeid voo Analyticsi, mis värskendab eelmääratletud ajakavas luua, vaadake selle [GitHub valimi](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Näpunäiteid viide andmete värskendamine ##

1. Ülekirjutamise viide andmete plekid ei põhjusta voo Analytics soovitud bloobimälu uuesti laadimiseks ja mõnel juhul põhjustada töö nurjumise. Soovitatav viis viide andmete muutmiseks on lisada uue bloobimälu, kasutades sama ümbris ja tee mustri määratletud töö sisendit ja kasutada kuupäeva/kellaaja **suurem** kui määratud viimase bloobimälu järjekorras.
2.  Lähteandmed plekid on määratud, kuvatakse bloobimälu "Viimati muudetud" aeg, vaid ainult selle bloobimälu määratud kuupäeva ja kellaaja nime abil {date} ja {aega} asendusi.
3.  Mitu korda tööd tuleb minna ajas tagasi, seega viide andmete plekid peab ei muutmist või kustutatud.

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud
Te olete võetud voo Analytics streaming analytics andmete põhjal asjade Interneti hallatav teenus. Selle teenuse kohta leiate lisateavet järgmistest teemadest.

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
