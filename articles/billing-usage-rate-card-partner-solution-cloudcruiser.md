<properties
   pageTitle="Pilvepõhine Cruiser ja arveldamine API integreerimine Microsoft Azure'i | Microsoft Azure'i"
   description="Microsoft Azure'i arveldus partnerilt Cloud ristleja, oma kogemusi integreerimine Azure arveldus API nende toode pakub ainulaadse perspektiivi.  See on eriti kasulik Azure ja Cloud Cruiser klientidele, kes on huvitatud abil/proovimise Cloud Cruiser Microsoft Azure'i paketi."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Pilvepõhine Cruiser ja Microsoft Azure'i arveldus API integreerimine

Selles artiklis kirjeldatakse, kuidas saate uue Microsoft Azure'i arveldus API kogutud teavet kasutada Cloud Cruiser töövoo maksumus simuleerimine ja analüüsi.

## <a name="azure-ratecard-api"></a>Azure'i RateCard API
RateCard API pärinevat teavet määr Azure. Pärast autentimist õige mandaat, saate päringu API koguda koos oma pakkumine ID-ga seostatud määrade Azure, metaandmete saadaval teenuste kohta

Järgmine on näide vastus API, kus on kuvatud hinnad on A0 (Windows), näiteks:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Pilvepõhine Cruiser's liidest Azure'i RateCard API
Pilveteenuse Cruiser saate kasutada RateCard API teavet erineval viisil. Selles artiklis näitame, kuidas seda saab teha IaaS töökoormus maksumus simulatsioon ja analüüs.

Näitamaks, et sel juhul kasutamine, oletage töökoormus, mitu eksemplari töötab Microsoft Azure Pack (WAP). Eesmärk on see sama töökoormus Azure simuleerida ja seda sellise migreerimise kulude. Selle simulatsiooni loomiseks on kaks peamist ülesandeid täita.

1. **Impordi- ja protsessi kogutud teavet teenuse RateCard API.** See toiming on ka läbi töövihikud, kus RateCard API väljavõte ümber ja avaldatud määr uuele lepingule. Selle uue lepingu määr kasutatakse simulatsioonid prognoosimiseks Azure'i eest.

2. **Normaliseerimine uuem ja Azure IaaS teenused.** Vaikimisi on aluseks uuem üksikute ressursid (CPU, mälu suurus, ketta suurus jne) ajal Azure'i teenused põhinevad mõõtmed (A0, A1, A2 jne). Selle esimese tööülesande saate sooritatud Cloud Cruiser ETL engine, nimetatakse töövihikuid, kus saate kombineeritud need ressursid, eksemplari suuruse, sarnane Azure eksemplari teenuste kohta.

### <a name="import-data-from-the-ratecard-api"></a>Andmete importimine RateCard API

Pilveteenuse Cruiser töövihikute pakuvad automatiseeritud võimalus RateCard API teabe kogumine ja töötlemine.  Töövihikute ETL (ekstrakti transformatsioon laaditav) võimaldavad teil konfigureerida saidikogumi, transformatsioon ja pilveteenuse Cruiser andmebaasi andmete avaldamine.

Iga töövihik võib olla üks või mitu saidikogumite, mis võimaldab teil oleksid täiendada või tõsta andmete kasutamine eri allikatest pärit teabe. Järgmised kaks kuvatõmmised näitab, kuidas luua uue *saidikogumi* olemasoleva töövihiku ja importimisest teabe *kogumine* RateCard API:

![Joonis 1 - uue saidikogumi loomine][1]

![Joonis 2 - uue saidikogumi andmete importimine][2]

Pärast andmete importimist töövihikusse, on võimalik luua mitme juhiseid ja transformatsioon protsesse, muuta ja andmeid mudel. Selle näite puhul, kuna oleme huvitatud taristu-kui-a-Service (IaaS) kasutame teisendus juhiseid mittevajalike ridade eemaldamine või kirjete, seotud teenuseid, välja arvatud IaaS.

Järgmine pilt on kuvatud RateCard API kogutud andmete töötlemiseks kasutatavate teisendus juhiseid.

![Joonis 3 - transformatsioon juhiseid RateCard API kogutud andmete töötlemiseks][3]

### <a name="defining-new-services-and-rate-plans"></a>Uusi teenuseid ja määr lepingud

On määratleda teenuste Cloud Cruiser võimalust. Üks variantidest on teenuste kasutamine andmete importimine. See meetod on levinud mis avaliku õhupalli, kus teenused on juba määratletud pakkuja töötamisel.

Määr, kus leping on määr või eest, mida saab rakendada erinevad teenused, efektiivse kuupäevade või rühma klientide muude suvandite vahel. Rate lepingute saate kasutada ka pilveteenuses Cruiser loomiseks simuleerimiseks või "Juhul kui" stsenaariumid, et aru saada, kuidas mõjutavad muutused teenuste kogumaksumus on töökoormus.

Selles näites kasutame teenuse teavet RateCard API määratleda Cloud Cruiser uusi teenuseid. Samal viisil me kasutame seotud teenuste määrade luua uue määr kavandamine Cloud Cruiser.

Teisendus protsessi lõpus on võimalik uuele etapile luua ja avaldada andmeid RateCard API uusi teenuseid ja määra.

![Joonis 4 - RateCard API andmete avaldamiseks uue teenused ja hinnad][4]

### <a name="verify-azure-services-and-rates"></a>Veenduge, et Azure'i teenused ja hinnad

Pärast avaldamist teenused ja hinnad, saate kontrollida Cloud Cruiser *teenuste* vahekaardil imporditud teenuste loend.

![Joonis 5 - kinnitatava uusi teenuseid][5]

Menüü *Määr lepingud* saate uue nimega "AzureSimulation" määr RateCard API imporditud määr leping.

![Joonis 6 – uus määr kavandamine ja seotud määr][6]

### <a name="normalize-wap-and-azure-services"></a>Normaliseerimine WAP ja Azure teenused

Vaikimisi WAP pakub kasutamise kohta, mis põhineb Arvuta, mälu ja võrgu ressursse. Pilveteenuse ristleja, saate määrata oma teenuste kohta otse eraldatud või nende ressursside mahupõhise kasutamist. Näiteks saate lihtsa määra iga tunni CPU hõivatus või tasuta GB mälu eraldada eksemplari.

Selle näite puhul vahelise WAP ja Azure, kulude võrdlemiseks läheb vaja liitmine ressursikasutuse WAP sisse, siis saab vastendada Azure'i teenuste kimbud. See teisendus saab hõlpsasti rakendada töövihikud:

![Joonis 7 - muutes WAP andmete normaliseerimine teenused][7]

Viimases etapis veebisaidil töövihik on Cloud Cruiser andmebaasi andmete avaldamiseks. Selles etapis tuleb ajal kasutamine andmete on kohe kombineeritud teenustesse (mis vastendamine Azure teenused) ja seotud vaikimisi määr olevate kulude loomiseks.

Pärast töövihiku, saate automatiseerida andmete töötlemise lisamine tööülesandele ajasti ja määrates sagedus ja kellaaeg käivitamiseks töövihiku jaoks.

![Joonis 8 – töövihiku plaanimine][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Töökoormus kulude simulatsioon analüüsi jaoks aruannete loomine

Pärast kasutamine kogutakse ja kulud laaditakse pilve Cruiser andmebaasi, saame saate kasutada Cloud Cruiser ülevaateid mooduli maksumus simulatsioon, mille soovime töökoormus loomiseks.

Selleks, et kujutavad seda stsenaariumi, lõime järgmine aruanne:

![Kulude võrdlus][9]

Ülemise graafik esitab maksumus võrdlus teenused, võrdlus hind käitamise töökoormus iga teenuse jaoks WAP (tumesinine) ja Azure (helesinine) vahel.

Alumine graafik esitab samad andmed, kuid osakonna järgi. See näitab iga osakonna juhtida tema töökoormus WAP ja Azure, nende vahe säästude ribal (roheline) koos kulud.

## <a name="azure-usage-api"></a>Azure'i kasutus API


### <a name="introduction"></a>Sissejuhatus

Hiljuti kasutusele Microsoft Azure'i kasutus API lubamisel tellijad programmiliselt tõmmata kasutusandmete saada ülevaate nende tarbimine. See on suurepärane uudiste Cloud Cruiser klientidele, kes saab seda API kaudu saadaolevad rikkalikumat andmekomplekti ära.

Pilveteenuse Cruiser saate kasutada integreerimine kasutus API on mitu võimalust. Granulaarsus (tunni kasutusteave) ja vajalikud andmekomplekti toetamiseks paindlik Showback või tagasinõude mudelite metaandmete Ressursiteave API kaudu saadaval. 

Selles õpetuses tutvustame üks näide sellest, kuidas saavad Cloud Cruiser API kasutamine teabe. Täpsemalt me ei loomine ressursirühma Azure, seostada sildid konto struktuuri ja seejärel kirjeldada tõmbamine ja pilveteenuse Cruiser sildi teabe töötlemine.
 
Lõplik eesmärk on võimalik luua aruandeid, nagu järgmises ja saama analüüsida maksumuse ja tarbimine konto struktuuri sildid, mille põhjal.

![Joonis 10 - aruande rikkeid siltide kasutamine][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure'i Sildid

Azure'i kasutuse kaudu kättesaadavad andmed sisaldavad teavet, vaid ka allika metaandmeid, sh sildid, sellega seotud. Siltide pakkuda lihtne viis korraldada oma ressursse, kuid selleks, et olla tõhus, peate tagamaks, et:

- Siltide rakendatakse õigesti ressursside sätte ajal.
- Siltide kasutatakse õigesti Showback/tagasinõude protsessi siduda kasutus ettevõtte konto struktuuri.

Mõlemad nendele nõuetele võib olla keeruline, eriti siis, kui seal on käsitsi protsessi sätte või laadimise pool. Valesti sisestatud, valed või isegi puuduvad sildid on levinud kaebusi klientide kui sildid ja nende vigade abil saab teha elu laadimise küljel väga raske.

Uue Azure kasutuse API Cloud Cruiser saate tõmmata Ressursiteave Suhtlussiltide ja keerukaid ETL tööriista nimega töövihikuid, kuni need levinud Suhtlussiltide vigade parandamine. Regulaaravaldised ja andmete korrelatsiooni abil teisendus, kuni Cloud Cruiser valesti sildistatud ressursside tuvastamine ja õige silte, tagada õige seost ressursside tarbija.

Laadimise pool Cloud Cruiser automatiseerib Showback/tagasinõude protsessi ja saate kasutada sildi teavet siduda kasutus vastav tarbija (osakonna, osakond, Project, jne). See automatiseerimine pakub suur täiustamise ja saate tagada järjepidevaid ja auditeeritavad laadimise protsess.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Ressursirühma loomine Microsoft Azure siltidega
Selles õpetuses esimene samm on luua ressursirühma Azure'i portaalis, siis saate luua uusi silte ressursside seostada. Selle näite puhul loome järgmised sildid: osakonna keskkonnas, omaniku, projekti.

Pildil kujutatud valimi ressursirühm seotud siltidega.

![Joonis 11 - ressursirühm seotud siltidega Azure'i portaalis][11]

Järgmise sammuna tõmmata teabe kasutus API Cloud Cruiser. Kasutus API pakub praegu vastuste JSON-vormingus. Siin on näide andmeid tuua:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Andmete importimine kasutus API Cloud Cruiser

Pilveteenuse Cruiser töövihikud pakuvad automatiseeritud võimalus API kasutamine teabe kogumine ja töötlemine. Tööraamatus ETL (ekstrakti transformatsioon laaditav) saate konfigureerida saidikogumi, transformatsioon ja pilveteenuse Cruiser andmebaasi andmete avaldamine.

Iga töövihik võib olla üks või mitu saidikogumid. See võimaldab teil oleksid täiendada või tõsta andmete kasutamine eri allikatest pärit teabe. Selle näite puhul loome uuele lehele töövihikus Azure mall (_UsageAPI)_ ja määrata uue _saidikogumi_ importida andmeid API kasutamine.

![Joonis 3 – imporditud UsageAPI lehe kasutusandmete API][12]

Pange tähele, et selles töövihikus juba muid lehti teenuste Azure (_ImportServices_) importimine ja töötlemine tarbimine teavet arveldus API (_PublishData_).

Järgmine me kasutus API _UsageAPI_ lehe asustamiseks kasutada, ja oleksid tarbimine andmetega arveldus API _PublishData_ lehel teave.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Sildi teabe kasutus API töötlemine

Pärast andmete importimist töövihikusse, loome teisendus juhiseid _UsageAPI_ lehe selleks, et töödelda API. Esimese sammuna tuleb ekstraktida sildid üksiku välja, siis iga üks neist (osakonna, Project, omanik ja keskkonna) väljade loomine "JSON tükeldamine" protsessor abil.

![Joonis 4 - silt teavet uute väljade loomine][13]

Teade "Võrgunduse" teenus pole sildi teavet (kollane kast), kuid me saab kinnitada, et osa sellest sama ressursirühm vaadates _ResourceGroupName_ välja. Kuna oleme teiste ressursside kaudu selle ressursirühma sildid, saate kasutame seda teavet puuduvad sildid rakendamiseks selle ressursi hiljem protsess.

Järgmiseks on seostamise kaudu sildid _ResourceGroupName_teabe otsing tabeli loomiseks. See otsingutabel kasutatakse rikastamiseks tarbimine andmed sildi teavet järgmise juhise juurde.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Sildi teabe lisamine tarbimine andmed

Nüüd saate liikuda _PublishData_ lehele, mille tarbimine teavet arveldus API, ja lisage väljad ekstraktimist sildid. Selle protsessi on läbi vaadates otsingutabel, mis on loodud eelmises etapis, kasutades _ResourceGroupName_ otsingud võti.

![Joonis 5 – ilma vaevata luua konto struktuur teabega otsingud][14]

Pange tähele, et vastav konto struktuuri väljad "Võrgunduse" teenus on rakendatud, on probleemi lahendamine puuduva siltidega. Me ka täidetud konto struktuuri väljad ressursid peale meie suunata ressursirühm "Muu", et eristada neid aruannete.

Nüüd lihtsalt tuleb lisada etapi kasutamine andmete avaldamiseks. Selle toimingu käigus rakendatakse vastav määr iga teenuse jaoks määratletud meie määr kavandamine kasutusteavet, et koos tulemiks oleva tasu laadimisel andmebaasi.

Parim osa on, et teil on ainult üks kord selle protsessi läbida. Kui töövihik on lõpule viidud, peate lihtsalt ajasti lisamiseks ja see töötab tund või iga päev määratud ajal. Seejärel on vaid paari uute aruannete loomine või kohandamine olemasolevate, et saada mõtestatud ülevaateid oma pilveteenuse kasutamine andmete analüüsimiseks.

### <a name="next-steps"></a>Järgmised sammud

+ Üksikasjalikud juhised Cloud Cruiser töövihikuid ja aruannete loomine, vaadake Cloud Cruiser online [dokumentatsiooni](http://docs.cloudcruiser.com/) (lubatud sisselogimine nõutud).  Pilveteenuse Cruiser kohta lisateabe saamiseks pöörduge [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Leiate [oma Microsoft Azure'i ressursside tarbimine ülevaate saada](billing-usage-rate-card-overview.md) ülevaate Azure'i ressursikasutus ja RateCard API-d.
+ [Azure'i arveldus REST API viide](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) nii API, mis on esitatud Azure ressursihaldur API-de määramine kohta lisateabe saamiseks vaadake.
+ Kui soovite alustada paremale proovi kood, vaadake meie Microsoft Azure'i arveldus API koodinäiteid [Azure'i koodinäiteid](https://azure.microsoft.com/documentation/samples/?term=billing)kohta.

### <a name="learn-more"></a>Lisateave
+ Lisateavet Azure ressursihaldur kohta leiate teemast [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md) .

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Joonis 1 - uue saidikogumi loomine"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Joonis 2 - uue saidikogumi andmete importimine"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Joonis 3 - transformatsioon juhiseid RateCard API kogutud andmete töötlemiseks"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Joonis 4 - RateCard API andmete avaldamiseks uue teenused ja hinnad"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Joonis 5 - kinnitatava uusi teenuseid"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Joonis 6 – uus määr kavandamine ja seotud määr"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Joonis 7 - muutes WAP andmete normaliseerimine teenused"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Joonis 8 – töövihiku plaanimine"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Joonis 9 – Näidisaruande stsenaariumi töökoormus kulu võrdlus"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Joonis 10 - aruande rikkeid siltide kasutamine"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Joonis 11 - ressursirühm seotud siltidega Azure'i portaalis"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Joonis 12 - imporditud UsageAPI lehe kasutusandmete API"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Joonis 13 - silt teavet uute väljade loomine"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Joonis 14 - ilma vaevata luua konto struktuur teabega otsingud"
