<properties 
   pageTitle="Teatiste Log Analytics | Microsoft Azure'i"
   description="Teatiste Log Analytics oluline teave oma OMS hoidlas tuvastamine ja saate aktiivselt märku probleemidest või seda autonoomsest proovib vea neid toiminguid.  Selles artiklis kirjeldatakse, kuidas luua reegli ja üksikasjad erinevaid toiminguid, nad saavad võtta."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Log Analytics teatised

Teatiste Log Analytics tuvastada olulist teavet oma OMS hoidla.  Teatiste reeglid automaatselt käivitada log otsingud skeemi järgi ja teatiste kirje loomine, kui tulemused vastavad teatud kriteeriumide.  Seejärel saate reegli automaatselt käivitada üks või mitu toimingut aktiivselt märku teatise või muu protsess autonoomsest.   

![Logige Analytics teatised](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Reegli loomine
Reegli loomiseks alustate log otsida kirjeid, mida tuleks autonoomsest teatise loomine.  Nupp **teatise** seejärel on saadaval nii, et saate luua ja konfigureerida reegli.

1.  Klõpsake lehe OMS ülevaade **Log otsing**.
2.  Kas saate luua uue päringu log otsing või valige salvestatud log otsing. 
3.  Klõpsake käsku **Teavita** lehe ülaosas **Reegli lisamine** Kuva avamiseks.
4. Vaadake lisateavet konfigureerimine teatise suvandid all tabelitest.
5. Kui selle ajaakna ette reegli, kuvatakse otsingukriteeriumidele jaoks selle ajaakna olemasolevate kirjete arvu.  See aitab teil otsustada, sagedus, mis annab teile eeldatavat tulemite arv.
4.  Klõpsake nuppu **Salvesta** reegli lõpuleviimiseks.  See hakatakse kohe.


![Reegli lisamine](media/log-analytics-alerts/add-alert-rule.png)

| Atribuut | Kirjeldus |
|:--|:--|
| **Teatiste teave** | |
| Nimi |  Reegli kordumatu nimi. |
| Raskusaste | Teade, mis on loodud selle reegli tõsidust. |
| Otsingupäringu | Valige **Otsingu praeguse päringu kasutamine** kasutada praeguse päringu või valige olemasoleva salvestatud otsingu loendist.  Otsingupäringu süntaks on toodud tekstiväli, kuhu saate muuta selle vajaduse korral.  |
| Ajaakna | Saate määrata ajavahemiku päringu.  Päring tagastab ainult kirjed, mis on loodud selles vahemikus praegune aeg.  See võib olla mis tahes väärtus 5 minutit kuni 24 tundi.  Peaks olema suurem või võrdne teatise sagedusega.  <br><br> Näiteks kui aknas on seatud 60 minutit ja päringu käivitada kell 01:15, tagastatakse ainult kirjete 12:15 pl ja 1:15 pl vahel. |
| **Ajakava** |     
| Lävi | Kriteeriumid, millal teatise loomine.  Kui päringu tagastatavad kirjete arv vastab kriteeriumid, luuakse teatise. |
| Teatiste sagedus | Saate määrata, kui sageli tuleks teha päring.  Võib olla mis tahes väärtus 5 minutit kuni 24 tundi.  Peaks olema võrdne või väiksem kui aknas. |
| Teatiste tõkestamine | Kui lülitate summutamist reegli, reegli toimingud on keelatud määratletud pika aja pärast luua uus teatis.  Reegel töötab endiselt ja loob teatiste kirjed, kui kriteeriumid on täidetud.  See on luba käivitamata dubleeritud toimingud probleemi lahendamiseks aega. |
| **Toimingud** | |
| E-posti teatis | Määrake **Jah** , kui te ei soovi meilisõnumit saata, kui selle reegli. |
| Teema    | E-posti teema.  Meili sisu ei saa muuta. |
| Adressaadid | Kõikide adressaatide aadressid.  Kui määrate mitu aadressi, siis eraldi aadressid semikooloniga (;). |
| Webhook | Määrake **Jah** , kui soovite kõne on webhook teatise käivitamisel. |
| Webhook URL-i | Funktsiooni webhook URL-i. |
| Lisada kohandatud JSON last | Valige see suvand, kui soovite vaikimisi last asendamine kohandatud last. |
| Sisestage oma kohandatud JSON last | Kohandatud last jaoks soovitud webhook.  Leiate eelmisest jaotisest. |
| Käitusjuhendi | Määrake **Jah** , kui soovite alustada ka Azure automatiseerimine käitusjuhendi teatise käivitamisel. |
| Valige soovitud käitusjuhendi | Valige käitusjuhendi käivitamiseks tegevusraamatud automatiseerimise konto konfigureeritud lahendusse automatiseerimine. |
| Käivita | Valige **Azure** käivitamiseks käitusjuhendi Azure pilveteenuses.  Valige **Hübriid töötaja** käivitamiseks käitusjuhendi [Hübriid Käitusjuhendi töötaja](..\automation\automation-hybrid-runbook-worker.md) teie kohaliku keskkonnas. |


## <a name="manage-alert-rules"></a>Hallata
Saate kõik teatiste reeglite loendi **teatiste** menüüs Logi Analytics **sätted**.  

![Teatiste haldamine](./media/log-analytics-alerts/configure.png)

1. Valige OMS konsooli paani **sätted** .
2. Valige **teatised**.

Selles vaates saate teha mitu toimingut.

- Reegli keelata, valides **välja** kõrval.
- Redigeerige reegli, klõpsake selle kõrval olevat pliiatsiikooni.
- Reegli eemaldamiseks klõpsake selle kõrval ikooni **X** . 


## <a name="setting-time-windows"></a>Kellaaeg Windowsi seadmine 

### <a name="event-alerts"></a>Sündmuste teatised

Sündmuste hulka kuuluvad andmeallikad Windows sündmuselogide, Logi, nt ja kohandatud logib.  Soovite teatise loomine kindla tõrke korral saab loomisel või mitme tõrgete sündmuste loomisel kindla teatud aja jooksul.

Hoiata ühe sündmus, määrake tulemite arv olla suurem kui 0 ja nii sagedust ja ajaakna 5 minutit.  See käivitab päringu iga 5 minuti ja ühe sündmuse, mis on loodud pärast viimast päringu käivitanud otsida.  Rohkem sagedus edasi lükata ajavahemik kogutakse sündmus ja loodava teatise.

Mõni rakendus võib aeg-ajalt viga, mida ei tohiks tingimata tõsta teatise logige sisse.  Näiteks võib rakendus uuesti protsessi loodud tõrke korral ja seejärel õnnestub järgmine kord, kui.  Sel juhul võib-olla ei soovite teatise luua, kui mitu sündmused on loodud kindla teatud aja jooksul.  

Mõnel juhul soovite teatise luua sündmuse puudumisel.  Näiteks võib protsessi logige tavaline sündmusi, näitamaks, et see ei tööta õigesti.  Kui see pole üks neist sündmustest kindla teatud aja jooksul sisse logida, siis teatise tuleks luua.  Sel juhul tuleks seada piirmäär *väiksem kui*1.

### <a name="performance-alerts"></a>Jõudluse teatised

[Jõudluse andmed](log-analytics-data-sources-performance-counters.md) on salvestatud kirjetena OMS hoidlas, mis on sarnane sündmused.  Iga kirje väärtus mõõta eelmise 30 minuti jooksul keskmise.  Kui soovite Teavita, kui jõudluse vastuolus ületab teatud piiri, siis selle piirmäära tuleks lisada päringusse.

Oletagem näiteks, et soovite Teavita, kui protsessor töötab üle 90% 30 minuti kasutaksite päringu nagu *Tüüp = täiuslik ObjectName = protsessor CounterName = "% protsessori aja" kaubeldes > 90* ja reegli *suurem kui 0*.  

 Kuna [jõudluse kirjed](log-analytics-data-sources-performance-counters.md) liidetakse iga 30 minuti järel koguda iga counter sagedus sõltumata sellest, on väiksem kui 30 minutit ajaakna võib tagastada kirjeid.  Säte aknas 30 minuti tagab üheks kirjeks saamiseks iga ühendatud andmeallika, mis tähistab keskmise selle aja jooksul.

## <a name="alert-actions"></a>Teatiste toimingud

Lisaks teatis-kirje loomise saate konfigureerida reegli automaatselt käivitada üks või mitu toimingut.  Toiminguid saate aktiivselt märku teatise või seda autonoomsest mõned protsess, mis avaldab tuvastatud probleemi lahendamiseks.  Järgmistes jaotistes kirjeldatakse toiminguid, mis on praegu saadaval.

### <a name="email-actions"></a>E-posti toimingud
E-posti toimingud saatmine e-posti teatise üksikasju ühele või mitmele adressaadile.  Saate määrata meili teema, kuid see sisu on standardne vorming ehitatud Log Analytics.  See sisaldab kokkuvõtva teabe, näiteks nimi teatise üksikasjade kuni kümme kirjete Logi otsinguga kõrval.  See hõlmab ka link Logi otsingu Log Analytics, mis tagastab kirjete terve kogumi päring.   Meili saatja on *Microsoft toimingute haldus komplekti meeskond &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Webhook toimingud

Webhook toimingud võimaldavad autonoomsest ühe HTTP POST taotlust läbi välise käigus.  Teenuse kutsutud peaks toetavad webhooks ja määrata, kuidas nad kasutavad mis tahes last saab.  Võite ka nimedeks REST API-ga, mis webhooks spetsiaalselt ei toeta, kui kutse on API mõistab vormingus.  Näiteid kasutamise on webhook vastuseks teatise kasutate teenust nagu [vaikne](http://slack.com) teatise või juhtum loomine teenuse nt [PagerDuty](http://pagerduty.com/)üksikasjadega sõnumi saatmiseks.  

Täielik samm-sammult juhendi abil webhook, helistada valimi teenuse reegli loomiseks on saadaval [Webhooks Log Analytics teatiste](log-analytics-alerts-webhooks.md).

Webhooks sisaldavad URL-i ja last, mis on vormindatud JSON, mis on saadetud välised andmed.  Vaikimisi sisaldab selle last väärtused järgmises tabelis.  Kui soovite, et see last asendamine oma kohandatud kaardiga.  Sel juhul saate muutujate tabelis iga parameetrid lisada oma kohandatud last nende väärtust.


| Parameetri | Muutuja | Kirjeldus |
|:--|:--|:--|
| AlertRuleName | #alertrulename | Reegli nimi. |
| AlertThresholdOperator | #thresholdoperator | Lävi tehtemärk reegli jaoks.  *Suurem* või *väiksem kui*. |
| AlertThresholdValue | #thresholdvalue | Reegli lävi väärtus. |
| LinkToSearchResults | #linktosearchresults | Link Logi Analytics log otsing, mis teatise loodud päring tagastab kirjed. |
| ResultCount  | #searchresultcount | Otsingutulemite kirjete arvu. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | Lõppkellaaeg päringu UTC-vormingus. |
| SearchIntervalInSeconds | #searchinterval | Reegli ajaakna. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | Alguskellaaeg päringu UTC-vormingus. |
| SearchQuery | #searchquery | Log otsingupäringu kasutatavaid reegli. |
| SearchResults | Allpool on | JSON-vormingus päringu tagastatavad andmed.  Piiratud esimese 5000 kirjed. |
| WorkspaceID | #workspaceid | ID oma OMS tööruumi. |


Näiteks võite määrata järgmised kohandatud last, mis sisaldab ühe parameetri nimi *tekst*.  Teenus, mis selle webhook helistab oleks oodanud parameeter.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Selles näites last oleks umbes järgmine saatmise funktsiooni webhook lahendamiseks.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Lisada kohandatud last otsingutulemite, lisage järgmine rida ülataseme kinnisvara json last.  

    "IncludeSearchResults":true

Näiteks saate luua kohandatud last, mis sisaldab ainult Teavita nime ja otsingutulemite, võite järgmist. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Te saate tutvustavad täieliku näide luua reegli webhook, veebisaidil [Log Analytics Teavita webhook valimi](log-analytics-alerts-webhooks.md)välise teenuse käivitamine.

### <a name="runbook-actions"></a>Käitusjuhendi toimingud

Käitusjuhendi toimingud käivitada soovitud käitusjuhendi Azure automatiseerimine.  Seda tüüpi toimingu kasutamiseks peab teil olema [automatiseerimise lahenduse](log-analytics-add-solutions.md) installinud ja konfigureerinud oma OMS tööruumis.  Kui teil pole installitud, kui loote uue teatise reegel, kuvatakse selle installi link.  Saate valida tegevusraamatud automatiseerimise konto automatiseerimise lahenduse konfigureeritud.

Käitusjuhendi toimingute abil soovitud [webhook](../automation/automation-webhooks.md)käitusjuhendi käivitamine  Kui loote reegli, selle automaatselt luua uue webhook jaoks käitusjuhendi **OMS teatise parandamise** , millele järgneb GUID nimega.  

Te ei saa otse asustada parameetrid käitusjuhendi raames, kuid [$WebhookData parameetri](../automation/automation-webhooks.md) üksikasju teatise, sh loodud log Otsingu tulemused.  Käitusjuhendi tuleb määratleda **$WebhookData** parameetrina seda juurdepääsu teatise atribuudid.  Teatise andmete on saadaval ühe atribuudi **SearchResults** **$WebhookData**atribuudis **RequestBody** json-vormingus.  See on järgmise tabeli atribuudid.


| Sõlm | Kirjeldus |
|:--|:--|
| ID         | Tee ja GUID otsing. |
| __metadata | Teave teatise, sh kirjete arvu ja otsingutulemite olekut. |
|  väärtus     |  Otsingutulemite iga kirje jaoks eraldi real.  Kirje üksikasjad sobib atribuudid ja väärtused kirje.   |

Näiteks järgmine käitusjuhendi kirjete Logi otsinguga ekstraktida ja määrata eri atribuutide alusel iga kirje tüüp.  Pange tähele, et käitusjuhendi hakkab teisendamine **RequestBody** json nii, et see saab töötas PowerShellis objektina.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Teatiste kirjed

Teatiste kirjete loodud teatiste reeglid Log Analytics on **Tippige** **Teatise** ja **SourceSystem** **OMS**.  Järgmises tabelis on neil atribuudid.

| Atribuut | Kirjeldus |
|:--|:--|
| Tüüp          | *Teavita* |
| SourceSystem  | *OMS* |
| AlertSeverity | Raskusaste taseme teatise. |
| AlertName     | Teatise nime. |
| Päringu         | Päringu käivitamine on tekst.  |
| QueryExecutionEndTime   | Ajavahemiku päringu lõpus. |
| QueryExecutionStartTime | Ajavahemiku päringu algust.  |
| TimeGenerated | Teatise loomise kuupäev ja kellaaeg. |

On muud tüüpi loodud [teatise lahendus](log-analytics-solution-alert-management.md) ja [Power BI ekspordib](log-analytics-powerbi.md)Teavita kirjed.  Need on **Tippige** **teatis** , kuid nende **SourceSystem**eristatakse.




## <a name="next-steps"></a>Järgmised sammud

- Installige [teatiste haldamise lahendus](log-analytics-solution-alert-management.md) teatiste loodud logifailide Analytics teatiste kogutud kaudu System Center toimingute Manager (SCOM) koos analüüsida.
- Lugege lisateavet [log otsingud](log-analytics-log-searches.md) andvate teatised.
- Täitke lühiülevaade [konfigureerimine on webook](log-analytics-alerts-webhooks.md) reegli abil.  
- Saate teada, kuidas kirjutada [Azure'i automaatika tegevusraamatud](https://azure.microsoft.com/documentation/services/automation) migreerimisel ning probleemide lahendamisel probleeme, mis on tähistatud teatiste abil.