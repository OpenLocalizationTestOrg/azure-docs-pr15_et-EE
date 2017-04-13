<properties
    pageTitle="Azure'i kuvari mõõdikud – toetatud mõõdikute ressursi liigi kohta | Microsoft Azure'i"
    description="Saadaval iga ressursi tüübi Azure'i monitoris mõõdikute loend."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Toetatud mõõdikute Azure'i monitoris

Azure'i kuvar on mitu võimalust suhelda mõõdikud, sh diagrammide neid portaalis, juurdepääs failidele peaaegu REST API kaudu või need päringud PowerShelli või CLI. Allpool on kõik mõõdikud täieliku loendi Azure'i kuvari argumendil müügivõimaluste praegu saadaval.

> [AZURE.NOTE] Muude mõõdikute võib olla saadaval portaali või pärand API-de abil. See loend sisaldab ainult avaliku eelvaade mõõdikute saadaval konsolideeritud Azure'i kuvari argumendil müügivõimaluste avaliku eelvaade abil.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Meetermõõdustik|Argumendil kuvatav nimi|Üksuse|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|CoreCount|Core loendamine|Count|Kogusumma|Koguarv valdkond paketi konto|
|TotalNodeCount|Sõlm loendamine|Count|Kogusumma|Koguarvu sõlmed paketi konto|
|CreatingNodeCount|Sõlm Count loomine|Count|Kogusumma|Loodava sõlmed arv|
|StartingNodeCount|Alates sõlm loendamine|Count|Kogusumma|Arvu sõlmed käivitamine|
|WaitingForStartTaskNodeCount|Ootel Start tööülesannete sõlm arv|Count|Kogusumma|Arvu sõlmed ootamine käivitamine tööülesande lõpuleviimiseks.|
|StartTaskFailedNodeCount|Tööülesande käivitamine nurjus sõlm loendamine|Count|Kogusumma|Kui tööülesande käivitamine ei ole sõlmed arv|
|IdleNodeCount|Jõude sõlm loendamine|Count|Kogusumma|Arvu jõude sõlmed.|
|OfflineNodeCount|Ühenduseta sõlm loendamine|Count|Kogusumma|Ühenduseta sõlmed arv|
|RebootingNodeCount|Rakenduste installimine sõlm loendamine|Count|Kogusumma|Arvu sõlmed taaskäivitamine|
|ReimagingNodeCount|Reimaging sõlm loendamine|Count|Kogusumma|Arvu reimaging sõlmed.|
|RunningNodeCount|Töötava sõlm loendamine|Count|Kogusumma|Töötavate sõlmed arvu|
|LeavingPoolNodeCount|Jättes Pool sõlm loendamine|Count|Kogusumma|Arvu sõlmed Pool lahkumata.|
|UnusableNodeCount|Kasutuskõlbmatuks sõlm loendamine|Count|Kogusumma|Arvu kasutuskõlbmatuks sõlmed.|
|TaskStartEvent|Tööülesande Start sündmused|Count|Kogusumma|Tööülesandeid, mida on alustanud arv|
|TaskCompleteEvent|Tööülesande lõpule viidud sündmused|Count|Kogusumma|Toimingud on maksimaalne arv|
|TaskFailEvent|Tööülesande Fail sündmused|Count|Kogusumma|Toimingud on tõrkeolekus arv|
|PoolCreateEvent|Kausta loomine sündmused|Count|Kogusumma|Koguarvu kaustu, mis on loodud|
|PoolResizeStartEvent|Kausta suuruse muutmise Start sündmused|Count|Kogusumma|Rakenduskausta muudab, mida on alustanud arv|
|PoolResizeCompleteEvent|Kausta suuruse muutmise lõpetamine sündmused|Count|Kogusumma|Koguarvu pool muudab, mis on lõpetatud|
|PoolDeleteStartEvent|Kausta kustutamine Start sündmused|Count|Kogusumma|Rakenduskausta kustutab, mida on alustanud arv|
|PoolDeleteCompleteEvent|Kausta kustutamine täieliku sündmused|Count|Kogusumma|Koguarvu rakenduskausta kustutab, mis on lõpetatud|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|connectedclients|Ühendatud kliendid|Count|Maksimum||
|totalcommandsprocessed|Tegevus kokku|Count|Kogusumma||
|cachehits|Vahemälutabamusi|Count|Kogusumma||
|cachemisses|Vahemälu jätab|Count|Kogusumma||
|getcommands|Saab|Count|Kogusumma||
|setcommands|Komplektid|Count|Kogusumma||
|evictedkeys|Väljatõstetud võtmed|Count|Kogusumma||
|totalkeys|Kokku võtmed|Count|Maksimum||
|expiredkeys|Aegunud võtmed|Count|Kogusumma||
|usedmemory|Mällu|Baiti|Maksimum||
|usedmemoryRss|Mällu RSS-i|Baiti|Maksimum||
|serverLoad|Serveri laadimine|%|Maksimum||
|cacheWrite|Vahemälu kirjutamine|BytesPerSecond|Maksimum||
|cacheRead|Vahemälu lugemine|BytesPerSecond|Maksimum||
|percentProcessorTime|CPU|%|Maksimum||
|connectedclients0|Seotud klientide (Kildu 0)|Count|Maksimum||
|totalcommandsprocessed0|Tegevus kokku (Kildu 0)|Count|Kogusumma||
|cachehits0|Vahemälutabamusi (Kildu 0)|Count|Kogusumma||
|cachemisses0|Vahemälu jätab (Kildu 0)|Count|Kogusumma||
|getcommands0|Saab (Kildu 0)|Count|Kogusumma||
|setcommands0|Komplektid (Kildu 0)|Count|Kogusumma||
|evictedkeys0|Väljatõstetud klahvid (Kildu 0)|Count|Kogusumma||
|totalkeys0|Kokku klahvid (sõlme 0)|Count|Maksimum||
|expiredkeys0|Aegunud klahvid (Kildu 0)|Count|Kogusumma||
|usedmemory0|Mällu (Kildu 0)|Baiti|Maksimum||
|usedmemoryRss0|Mällu RSS-i (Kildu 0)|Baiti|Maksimum||
|serverLoad0|Serveri koormus (Kildu 0)|%|Maksimum||
|cacheWrite0|Vahemälu kirjutamine (Kildu 0)|BytesPerSecond|Maksimum||
|cacheRead0|Vahemälu lugemine (Kildu 0)|BytesPerSecond|Maksimum||
|percentProcessorTime0|CPU (Kildu 0)|%|Maksimum||
|connectedclients1|Seotud klientide (Kildu 1)|Count|Maksimum||
|totalcommandsprocessed1|Tegevus kokku (Kildu 1)|Count|Kogusumma||
|cachehits1|Vahemälutabamusi (Kildu 1)|Count|Kogusumma||
|cachemisses1|Vahemälu jätab (Kildu 1)|Count|Kogusumma||
|getcommands1|Saab (Kildu 1)|Count|Kogusumma||
|setcommands1|Komplektid (Kildu 1)|Count|Kogusumma||
|evictedkeys1|Väljatõstetud klahvid (Kildu 1)|Count|Kogusumma||
|totalkeys1|Kokku klahvid (sõlm 1)|Count|Maksimum||
|expiredkeys1|Aegunud klahvid (Kildu 1)|Count|Kogusumma||
|usedmemory1|Mällu (Kildu 1)|Baiti|Maksimum||
|usedmemoryRss1|Mällu RSS-i (Kildu 1)|Baiti|Maksimum||
|serverLoad1|Serveri koormus (Kildu 1)|%|Maksimum||
|cacheWrite1|Vahemälu kirjutamine (Kildu 1)|BytesPerSecond|Maksimum||
|cacheRead1|Vahemälu lugemine (Kildu 1)|BytesPerSecond|Maksimum||
|percentProcessorTime1|CPU (Kildu 1)|%|Maksimum||
|connectedclients2|Seotud klientide (Kildu 2)|Count|Maksimum||
|totalcommandsprocessed2|Tegevus kokku (Kildu 2)|Count|Kogusumma||
|cachehits2|Vahemälutabamusi (Kildu 2)|Count|Kogusumma||
|cachemisses2|Vahemälu jätab (Kildu 2)|Count|Kogusumma||
|getcommands2|Saab (Kildu 2)|Count|Kogusumma||
|setcommands2|Komplektid (Kildu 2)|Count|Kogusumma||
|evictedkeys2|Väljatõstetud klahvid (Kildu 2)|Count|Kogusumma||
|totalkeys2|Kokku klahvid (sõlme 2)|Count|Maksimum||
|expiredkeys2|Aegunud klahvid (Kildu 2)|Count|Kogusumma||
|usedmemory2|Mällu (Kildu 2)|Baiti|Maksimum||
|usedmemoryRss2|Mällu RSS-i (Kildu 2)|Baiti|Maksimum||
|serverLoad2|Serveri koormus (Kildu 2)|%|Maksimum||
|cacheWrite2|Vahemälu kirjutamine (Kildu 2)|BytesPerSecond|Maksimum||
|cacheRead2|Vahemälu Read (2 Kildu)|BytesPerSecond|Maksimum||
|percentProcessorTime2|CPU (Kildu 2)|%|Maksimum||
|connectedclients3|Seotud klientide (Kildu 3)|Count|Maksimum||
|totalcommandsprocessed3|Tegevus kokku (Kildu 3)|Count|Kogusumma||
|cachehits3|Vahemälutabamusi (3 Kildu)|Count|Kogusumma||
|cachemisses3|Vahemälu jätab (Kildu 3)|Count|Kogusumma||
|getcommands3|Saab (Kildu 3)|Count|Kogusumma||
|setcommands3|Komplektid (3 Kildu)|Count|Kogusumma||
|evictedkeys3|Väljatõstetud klahvid (Kildu 3)|Count|Kogusumma||
|totalkeys3|Kokku klahvid (sõlme 3)|Count|Maksimum||
|expiredkeys3|Aegunud klahvid (Kildu 3)|Count|Kogusumma||
|usedmemory3|Mällu (Kildu 3)|Baiti|Maksimum||
|usedmemoryRss3|Mällu RSS-i (Kildu 3)|Baiti|Maksimum||
|serverLoad3|Serveri koormus (Kildu 3)|%|Maksimum||
|cacheWrite3|Kirjutage vahemälu (3 Kildu)|BytesPerSecond|Maksimum||
|cacheRead3|Vahemälu Read (3 Kildu)|BytesPerSecond|Maksimum||
|percentProcessorTime3|CPU (Kildu 3)|%|Maksimum||
|connectedclients4|Seotud klientide (Kildu 4)|Count|Maksimum||
|totalcommandsprocessed4|Tegevus kokku (Kildu 4)|Count|Kogusumma||
|cachehits4|Vahemälutabamusi (Kildu 4)|Count|Kogusumma||
|cachemisses4|Vahemälu jätab (Kildu 4)|Count|Kogusumma||
|getcommands4|Saab (Kildu 4)|Count|Kogusumma||
|setcommands4|Komplektid (Kildu 4)|Count|Kogusumma||
|evictedkeys4|Väljatõstetud klahvid (Kildu 4)|Count|Kogusumma||
|totalkeys4|Kokku klahvid (sõlm 4)|Count|Maksimum||
|expiredkeys4|Aegunud klahvid (Kildu 4)|Count|Kogusumma||
|usedmemory4|Mällu (Kildu 4)|Baiti|Maksimum||
|usedmemoryRss4|Mällu RSS-i (Kildu 4)|Baiti|Maksimum||
|serverLoad4|Serveri koormus (Kildu 4)|%|Maksimum||
|cacheWrite4|Vahemälu kirjutamine (Kildu 4)|BytesPerSecond|Maksimum||
|cacheRead4|Vahemälu lugemine (Kildu 4)|BytesPerSecond|Maksimum||
|percentProcessorTime4|CPU (Kildu 4)|%|Maksimum||
|connectedclients5|Seotud klientide (Kildu 5)|Count|Maksimum||
|totalcommandsprocessed5|Tegevus kokku (Kildu 5)|Count|Kogusumma||
|cachehits5|Vahemälutabamusi (Kildu 5)|Count|Kogusumma||
|cachemisses5|Vahemälu jätab (Kildu 5)|Count|Kogusumma||
|getcommands5|Saab (Kildu 5)|Count|Kogusumma||
|setcommands5|Komplektid (Kildu 5)|Count|Kogusumma||
|evictedkeys5|Väljatõstetud klahvid (Kildu 5)|Count|Kogusumma||
|totalkeys5|Kokku klahvid (sõlme 5)|Count|Maksimum||
|expiredkeys5|Aegunud klahvid (Kildu 5)|Count|Kogusumma||
|usedmemory5|Mällu (Kildu 5)|Baiti|Maksimum||
|usedmemoryRss5|Mällu RSS-i (Kildu 5)|Baiti|Maksimum||
|serverLoad5|Serveri koormus (Kildu 5)|%|Maksimum||
|cacheWrite5|Vahemälu kirjutamine (Kildu 5)|BytesPerSecond|Maksimum||
|cacheRead5|Vahemälu lugemine (Kildu 5)|BytesPerSecond|Maksimum||
|percentProcessorTime5|CPU (Kildu 5)|%|Maksimum||
|connectedclients6|Seotud klientide (Kildu 6)|Count|Maksimum||
|totalcommandsprocessed6|Tegevus kokku (Kildu 6)|Count|Kogusumma||
|cachehits6|Vahemälutabamusi (Kildu 6)|Count|Kogusumma||
|cachemisses6|Vahemälu jätab (Kildu 6)|Count|Kogusumma||
|getcommands6|Saab (Kildu 6)|Count|Kogusumma||
|setcommands6|Komplektid (Kildu 6)|Count|Kogusumma||
|evictedkeys6|Väljatõstetud klahvid (Kildu 6)|Count|Kogusumma||
|totalkeys6|Kokku klahvid (sõlme 6)|Count|Maksimum||
|expiredkeys6|Aegunud klahvid (Kildu 6)|Count|Kogusumma||
|usedmemory6|Mällu (Kildu 6)|Baiti|Maksimum||
|usedmemoryRss6|Mällu RSS-i (Kildu 6)|Baiti|Maksimum||
|serverLoad6|Serveri koormus (Kildu 6)|%|Maksimum||
|cacheWrite6|Vahemälu kirjutamine (Kildu 6)|BytesPerSecond|Maksimum||
|cacheRead6|Vahemälu lugemine (Kildu 6)|BytesPerSecond|Maksimum||
|percentProcessorTime6|CPU (Kildu 6)|%|Maksimum||
|connectedclients7|Seotud klientide (Kildu 7)|Count|Maksimum||
|totalcommandsprocessed7|Tegevus kokku (Kildu 7)|Count|Kogusumma||
|cachehits7|Vahemälutabamusi (Kildu 7)|Count|Kogusumma||
|cachemisses7|Vahemälu jätab (Kildu 7)|Count|Kogusumma||
|getcommands7|Saab (Kildu 7)|Count|Kogusumma||
|setcommands7|Komplektid (Kildu 7)|Count|Kogusumma||
|evictedkeys7|Väljatõstetud klahvid (Kildu 7)|Count|Kogusumma||
|totalkeys7|Kokku klahvid (sõlme 7)|Count|Maksimum||
|expiredkeys7|Aegunud klahvid (Kildu 7)|Count|Kogusumma||
|usedmemory7|Mällu (Kildu 7)|Baiti|Maksimum||
|usedmemoryRss7|Mällu RSS-i (Kildu 7)|Baiti|Maksimum||
|serverLoad7|Serveri koormus (Kildu 7)|%|Maksimum||
|cacheWrite7|Vahemälu kirjutamine (Kildu 7)|BytesPerSecond|Maksimum||
|cacheRead7|Vahemälu lugemine (Kildu 7)|BytesPerSecond|Maksimum||
|percentProcessorTime7|CPU (Kildu 7)|%|Maksimum||
|connectedclients8|Seotud klientide (Kildu 8)|Count|Maksimum||
|totalcommandsprocessed8|Tegevus kokku (Kildu 8)|Count|Kogusumma||
|cachehits8|Vahemälutabamusi (Kildu 8)|Count|Kogusumma||
|cachemisses8|Vahemälu jätab (Kildu 8)|Count|Kogusumma||
|getcommands8|Saab (Kildu 8)|Count|Kogusumma||
|setcommands8|Komplektid (Kildu 8)|Count|Kogusumma||
|evictedkeys8|Väljatõstetud klahvid (Kildu 8)|Count|Kogusumma||
|totalkeys8|Kokku klahvid (sõlme 8)|Count|Maksimum||
|expiredkeys8|Aegunud klahvid (Kildu 8)|Count|Kogusumma||
|usedmemory8|Mällu (Kildu 8)|Baiti|Maksimum||
|usedmemoryRss8|Mällu RSS-i (Kildu 8)|Baiti|Maksimum||
|serverLoad8|Serveri koormus (Kildu 8)|%|Maksimum||
|cacheWrite8|Vahemälu kirjutamine (Kildu 8)|BytesPerSecond|Maksimum||
|cacheRead8|Vahemälu lugemine (Kildu 8)|BytesPerSecond|Maksimum||
|percentProcessorTime8|CPU (Kildu 8)|%|Maksimum||
|connectedclients9|Seotud klientide (Kildu 9)|Count|Maksimum||
|totalcommandsprocessed9|Tegevus kokku (Kildu 9)|Count|Kogusumma||
|cachehits9|Vahemälutabamusi (Kildu 9)|Count|Kogusumma||
|cachemisses9|Vahemälu jätab (Kildu 9)|Count|Kogusumma||
|getcommands9|Saab (Kildu 9)|Count|Kogusumma||
|setcommands9|Komplektid (Kildu 9)|Count|Kogusumma||
|evictedkeys9|Väljatõstetud klahvid (Kildu 9)|Count|Kogusumma||
|totalkeys9|Kokku klahvid (sõlme 9)|Count|Maksimum||
|expiredkeys9|Aegunud klahvid (Kildu 9)|Count|Kogusumma||
|usedmemory9|Mällu (Kildu 9)|Baiti|Maksimum||
|usedmemoryRss9|Mällu RSS-i (Kildu 9)|Baiti|Maksimum||
|serverLoad9|Serveri koormus (Kildu 9)|%|Maksimum||
|cacheWrite9|Vahemälu kirjutamine (Kildu 9)|BytesPerSecond|Maksimum||
|cacheRead9|Vahemälu lugemine (Kildu 9)|BytesPerSecond|Maksimum||
|percentProcessorTime9|CPU (Kildu 9)|%|Maksimum||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Meetermõõdustik|Argumendil kuvatav nimi|Üksuse|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|NumberOfCalls|Koguarvu API kõned|Count|Kogusumma|API kõned koguarv.|
|NumberOfFailedCalls|Nurjunud API kõned arv|Count|Kogusumma|Nurjunud API kõned koguarv.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|Protsent CPU|Protsent CPU|%|Keskmine|Eraldatud Arvuta üksused, mis on praegu kasutusel on Virtual Machine(s) protsent|
|Võrgu|Võrgu|Baiti|Kogusumma|Kõigi võrgu liideste Virtual Machine(s) (sissetulev liiklus) saadud baiti|
|Võrgu välja|Võrgu välja|Baiti|Kogusumma|Kõigi võrgu liidesed, Virtual Machine(s) (väljamineva liikluse) kohta baiti|
|Ketas lugemine baiti|Ketas lugemine baiti|Baiti|Kogusumma|Kokku baiti lugeda kettalt jälgimise perioodi kohta.|
|Kettale kirjutamine baiti|Kettale kirjutamine baiti|Baiti|Kogusumma|Kokku kirjutatud kettale jälgimise perioodi jooksul baiti|
|Ketas lugema sekundis|Ketas lugema sekundis|CountPerSecond|Keskmine|Ketas lugemine IOPS|
|Kettale kirjutamine sekundis|Kettale kirjutamine sekundis|CountPerSecond|Keskmine|Kettale kirjutamine IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|Protsent CPU|Protsent CPU|%|Keskmine|Eraldatud Arvuta üksused, mis on praegu kasutusel on Virtual Machine(s) protsent|
|Võrgu|Võrgu|Baiti|Kogusumma|Kõigi võrgu liideste Virtual Machine(s) (sissetulev liiklus) saadud baiti|
|Võrgu välja|Võrgu välja|Baiti|Kogusumma|Kõigi võrgu liidesed, Virtual Machine(s) (väljamineva liikluse) kohta baiti|
|Ketas lugemine baiti|Ketas lugemine baiti|Baiti|Kogusumma|Kokku baiti lugeda kettalt jälgimise perioodi kohta.|
|Kettale kirjutamine baiti|Kettale kirjutamine baiti|Baiti|Kogusumma|Kokku kirjutatud kettale jälgimise perioodi jooksul baiti|
|Ketas lugema sekundis|Ketas lugema sekundis|CountPerSecond|Keskmine|Ketas lugemine IOPS|
|Kettale kirjutamine sekundis|Kettale kirjutamine sekundis|CountPerSecond|Keskmine|Kettale kirjutamine IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Meetermõõdustik|Argumendil kuvatav nimi|Üksuse|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|Protsent CPU|Protsent CPU|%|Keskmine|Eraldatud Arvuta üksused, mis on praegu kasutusel on Virtual Machine(s) protsent|
|Võrgu|Võrgu|Baiti|Kogusumma|Kõigi võrgu liideste Virtual Machine(s) (sissetulev liiklus) saadud baiti|
|Võrgu välja|Võrgu välja|Baiti|Kogusumma|Kõigi võrgu liidesed, Virtual Machine(s) (väljamineva liikluse) kohta baiti|
|Ketas lugemine baiti|Ketas lugemine baiti|Baiti|Kogusumma|Kokku baiti lugeda kettalt jälgimise perioodi kohta.|
|Kettale kirjutamine baiti|Kettale kirjutamine baiti|Baiti|Kogusumma|Kokku kirjutatud kettale jälgimise perioodi jooksul baiti|
|Ketas lugema sekundis|Ketas lugema sekundis|CountPerSecond|Keskmine|Ketas lugemine IOPS|
|Kettale kirjutamine sekundis|Kettale kirjutamine sekundis|CountPerSecond|Keskmine|Kettale kirjutamine IOPS|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Meetermõõdustik|Argumendil kuvatav nimi|Üksuse|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|D2C.telemetry.Ingress.allProtocol|Telemeetria sõnumi saatmine katsete|Count|Kogusumma|Seadme Cloud telemeetria sõnumite arvu proovitakse oma asjade jaoturi saadetakse|
|D2C.telemetry.Ingress.Success|Telemeetria saadetud sõnumid|Count|Kogusumma|Pilveteenuse telemeetria sõnumite saatmine oma asjade jaoturi seadme arvu|
|C2D.Commands.egress.Complete.Success|Käsud, mis on lõpetatud|Count|Kogusumma|Pilveteenuse arvu lõpule viidud, seade seadme käsud|
|C2D.Commands.egress.Abandon.Success|Hüljatud käsud|Count|Kogusumma|Pilveteenuse arvu loobunud seade seadme käsud|
|C2D.Commands.egress.Reject.Success|Käsud, mis on tagasi lükatud|Count|Kogusumma|Pilveteenuse arv tagasi lükanud seade seadme käsud|
|devices.totalDevices|Kokku seadmed|Count|Kogusumma|Oma asjade jaoturi registreeritud seadmete arv|
|devices.connectedDevices.allProtocol|Ühendatud seadmete|Count|Kogusumma|Oma asjade jaoturiga ühendatud seadmete arv|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Meetermõõdustik|Argumendil kuvatav nimi|Üksuse|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|INREQS|Sissetulevad taotlused|Count|Kogusumma|Sündmuse jaoturi sissetuleva sõnumi läbilaskevõime nimeruumi jaoks|
|SUCCREQ|Eduka taotlused.|Count|Kogusumma|Nimeruumi kokku eduka taotlemine|
|FAILREQ|Nurjunud taotlusi|Count|Kogusumma|Nimeruumi kokku nurjunud taotlemine|
|SVRBSY|Server hõivatud tõrked|Count|Kogusumma|Nimeruumi kokku server hõivatud tõrked|
|TEADETE|Sisemise serveri tõrked|Count|Kogusumma|Nimeruumi kokku sisemise serveri tõrked|
|MISCERR|Muud tõrked|Count|Kogusumma|Nimeruumi kokku nurjunud taotlemine|
|INMSGS|Sissetulevate sõnumite|Count|Kogusumma|Nimeruumi sissetulevaid sõnumeid kokku|
|OUTMSGS|Väljaminevate sõnumite|Count|Kogusumma|Väljaminevate sõnumite nimeruumi kokku|
|EHINMBS|Sissetuleva baiti sekundis|BytesPerSecond|Kogusumma|Sündmuse jaoturi sissetuleva sõnumi läbilaskevõime nimeruumi jaoks|
|EHOUTMBS|Väljaminevad baiti sekundis|BytesPerSecond|Kogusumma|Väljaminevate sõnumite nimeruumi kokku|
|EHABL|Sõnumite arhiivimine mahajäämus|Count|Kogusumma|Sündmuse jaoturi arhiivi sõnumite jaoks nimeruumi mahajäämus|
|EHAMSGS|Sõnumite arhiivimine|Count|Kogusumma|Sündmuse jaoturi arhiivitud sõnumid nimeruumi|
|EHAMBS|Arhiivi sõnumi läbilaskevõime|BytesPerSecond|Kogusumma|Sündmuse jaoturi arhiivitud sõnumi läbilaskevõime nimeruumi|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|RunsStarted|Käivitatakse alustamine|Count|Kogusumma|Arvu töövoog käivitatakse alustamine.|
|RunsCompleted|Käivitatakse lõpetatud|Count|Kogusumma|Arvu töövoog käivitatakse lõpule viidud.|
|RunsSucceeded|Käivitatakse õnnestus|Count|Kogusumma|Arvu töövoog käivitatakse õnnestus.|
|RunsFailed|Käivitatakse nurjus.|Count|Kogusumma|Arvu töövoog käivitatakse nurjunud.|
|RunsCancelled|Tühistatud käivitatakse|Count|Kogusumma|Arvu töövoog käivitatakse tühistatud.|
|RunLatency|Latentsus käivitamine|Minutit|Keskmine|Latentsus lõplikus töövoog töötab.|
|RunSuccessLatency|Käivitage edu latentsus|Minutit|Keskmine|Latentsus õnnestus töövoog töötab.|
|RunThrottledEvents|Käivitage rakendus sündmused|Count|Kogusumma|Arvu töövoo toimingu või Käivita rakendus sündmused.|
|RunFailurePercentage|Tõrge protsent käivitamine|%|Kogusumma|Protsendi töövoog käivitatakse nurjunud.|
|ActionsStarted|Tegevuste alustamine |Count|Kogusumma|Töövoo tegevuste alustamine arv.|
|ActionsCompleted|Lõpetatud toimingud |Count|Kogusumma|Töövootoimingud lõpetatud arv.|
|ActionsSucceeded|Õnnestus toimingud |Count|Kogusumma|Töövootoimingud arvu õnnestus.|
|ActionsFailed|Toimingud nurjus.|Count|Kogusumma|Töövootoimingud arvu nurjus.|
|ActionsSkipped|Toimingud vahele |Count|Kogusumma|Töövootoimingud arvu vahele.|
|ActionLatency|Toimingu latentsus |Minutit|Keskmine|Latentsus töövootoimingud on lõpule viidud.|
|ActionSuccessLatency|Toimingu õnnestumise latentsus |Minutit|Keskmine|Latentsus õnnestus töövootoimingud.|
|ActionThrottledEvents|Toimingu rakendus sündmused|Count|Kogusumma|Töövoo toimingu arvu rakendus sündmused.|
|TriggersStarted|Päästikute alustamine |Count|Kogusumma|Töövoo käivitab alustamine arv.|
|TriggersCompleted|Päästikute lõpetatud |Count|Kogusumma|Töövoo käivitab lõpetatud arv.|
|TriggersSucceeded|Päästikute õnnestus |Count|Kogusumma|Töövoo käivitab arvu õnnestus.|
|TriggersFailed|Päästikute nurjus. |Count|Kogusumma|Töövoo käivitab arvu nurjus.|
|TriggersSkipped|Päästikute vahele|Count|Kogusumma|Töövoo käivitab arvu vahele.|
|TriggersFired|Päästikute töötavad |Count|Kogusumma|Töövoo käivitab arv töötavad.|
|TriggerLatency|Päästik latentsus |Minutit|Keskmine|Latentsus lõplikus töövoo käivitab.|
|TriggerFireLatency|Päästik Fire latentsus |Minutit|Keskmine|Latentsus töötas töövoo käivitab.|
|TriggerSuccessLatency|Päästik edu latentsus |Minutit|Keskmine|Latentsus õnnestus töövoo käivitab.|
|TriggerThrottledEvents|Rakendus sündmuste käivitamine|Count|Kogusumma|Arvu töövoo käivitada rakendus sündmused.|
|BillableActionExecutions|Arveldatavate toimingu täitmised|Count|Kogusumma|Töövoo toimingu täitmised saada arve number.|
|BillableTriggerExecutions|Arveldatavate päästik täitmised.|Count|Kogusumma|Töövoo käivitada täitmised saada arve number.|
|TotalBillableExecutions|Kokku arveldatavate täitmised|Count|Kogusumma|Töövoo täitmised saada arve number.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|Läbilaskevõime|Läbilaskevõime|BytesPerSecond|Keskmine||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|SearchLatency|Otsingu latentsus|Minutit|Keskmine|Otsingu keskmise latentsuse otsinguteenuse jaoks|
|SearchQueriesPerSecond|Otsimispäringud sekundis|CountPerSecond|Keskmine|Otsingupäringuid sekundis otsinguteenuse jaoks|
|ThrottledSearchQueriesPercentage|Rakendus otsing päringute protsent|%|Keskmine|Protsent otsingupäringuid, mis olid rakendus otsinguteenuse jaoks|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|CPUXNS|CPU hõivatus nimeruum kohta.|%|Maksimum|Teenuse siini premium nimeruum CPU kasutus meetermõõdustik|
|WSXNS|Suurus mälukasutuse nimeruum kohta.|%|Maksimum|Teenuse siini premium nimeruum mälu kasutus meetermõõdustik|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|cpu_percent|CPU protsent|%|Keskmine|CPU protsent|
|physical_data_read_percent|Andmete IO protsent|%|Keskmine|Andmete IO protsent|
|log_write_percent|Log IO protsent|%|Keskmine|Log IO protsent|
|dtu_consumption_percent|DTU protsent|%|Keskmine|DTU protsent|
|salvestusruumi|Andmebaasi suurus|Baiti|Maksimum|Andmebaasi suurus|
|connection_successful|Eduka ühendused|Count|Kogusumma|Eduka ühendused|
|connection_failed|Nurjunud ühendused|Count|Kogusumma|Nurjunud ühendused|
|blocked_by_firewall|Tulemüür blokeerib|Count|Kogusumma|Tulemüür blokeerib|
|tupik|Tupikuid|Count|Kogusumma|Tupikuid|
|storage_percent|Andmebaasi suurus protsent|%|Maksimum|Andmebaasi suurus protsent|
|xtp_storage_percent|-Mälu OLTP salvestusruumi percent(Preview)|%|Keskmine|Percent(Preview)-mälu OLTP salvestusruum|
|workers_percent|Töötajate protsent|%|Keskmine|Töötajate protsent|
|sessions_percent|Seansid protsenti|%|Keskmine|Seansid protsenti|
|dtu_limit|DTU limiit|Count|Keskmine|DTU limiit|
|dtu_used|DTU kasutatud|Count|Keskmine|DTU kasutatud|
|service_level_objective|Andmebaasi teenuse eesmärk|Count|Kogusumma|Andmebaasi teenuse eesmärk|
|dwu_limit|DWU limiit|Count|Maksimum|DWU limiit|
|dwu_consumption_percent|DWU protsent|%|Keskmine|DWU protsent|
|dwu_used|DWU kasutatud|Count|Keskmine|Kasutatud DWU|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|cpu_percent|CPU protsent|%|Keskmine|CPU protsent|
|physical_data_read_percent|Andmete IO protsent|%|Keskmine|Andmete IO protsent|
|log_write_percent|Log IO protsent|%|Keskmine|Log IO protsent|
|dtu_consumption_percent|DTU protsent|%|Keskmine|DTU protsent|
|storage_percent|Salvestusruumi protsent|%|Keskmine|Salvestusruumi protsent|
|workers_percent|Töötajate protsent|%|Keskmine|Töötajate protsent|
|sessions_percent|Seansid protsenti|%|Keskmine|Seansid protsenti|
|eDTU_limit|eDTU limiit|Count|Keskmine|eDTU limiit|
|storage_limit|Talletuslimiit|Baiti|Keskmine|Talletuslimiit|
|eDTU_used|kasutatud eDTU|Count|Keskmine|kasutatud eDTU|
|storage_used|Kasutatav salvestusruum|Baiti|Keskmine|Kasutatav salvestusruum|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|ResourceUtilization|SU % kasutamine|%|Maksimum|SU % kasutamine|
|InputEvents|Sisestuskeel sündmused|Count|Kogusumma|Sisestuskeel sündmused|
|InputEventBytes|Sisestage sündmuse baiti|Baiti|Kogusumma|Sisestage sündmuse baiti|
|LateInputEvents|Hilinenud Sisestuskeel sündmused|Count|Kogusumma|Hilinenud Sisestuskeel sündmused|
|OutputEvents|Väljundi sündmused|Count|Kogusumma|Väljundi sündmused|
|ConversionErrors|Andmete teisendamine tõrked|Count|Kogusumma|Andmete teisendamine tõrked|
|Tõrked|Käitusaja tõrked|Count|Kogusumma|Käitusaja tõrked|
|DroppedOrAdjustedEvents|Vales järjestuses sündmused|Count|Kogusumma|Vales järjestuses sündmused|
|AMLCalloutRequests|Funktsioon taotlused|Count|Kogusumma|Funktsioon taotlused|
|AMLCalloutFailedRequests|Nurjunud funktsioon taotlused|Count|Kogusumma|Nurjunud funktsioon taotlused|
|AMLCalloutInputEvents|Funktsioon sündmused|Count|Kogusumma|Funktsioon sündmused|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|CpuPercentage|CPU protsent|%|Keskmine|CPU protsent|
|MemoryPercentage|Mälu protsent|%|Keskmine|Mälu protsent|
|DiskQueueLength|Ketas järjekorra pikkus|Count|Kogusumma|Ketas järjekorra pikkus|
|HttpQueueLength|Http järjekorra pikkus|Count|Kogusumma|Http järjekorra pikkus|
|BytesReceived|Andmete|Baiti|Kogusumma|Andmete|
|BytesSent|Andmete välja|Baiti|Kogusumma|Andmete välja|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|CpuTime|CPU aega|Minutit|Kogusumma|CPU aega|
|Taotlused|Taotlused|Count|Kogusumma|Taotlused|
|BytesReceived|Andmete|Baiti|Kogusumma|Andmete|
|BytesSent|Andmete välja|Baiti|Kogusumma|Andmete välja|
|Http2xx|Http 2xx|Count|Kogusumma|Http 2xx|
|Http3xx|Http 3xx|Count|Kogusumma|Http 3xx.|
|Http401|Http 401|Count|Kogusumma|Http 401|
|Http403|Http 403|Count|Kogusumma|Http 403|
|Http404|Http 404|Count|Kogusumma|Http 404|
|Http406|Http 406|Count|Kogusumma|Http 406|
|Http4xx|Http 4xx.|Count|Kogusumma|Http 4xx.|
|Http5xx|Http Serveri tõrked|Count|Kogusumma|Http Serveri tõrked|
|MemoryWorkingSet|Mälu töökomplekt|Baiti|Kogusumma|Mälu töökomplekt|
|AverageMemoryWorkingSet|Keskmine Mälu töökomplekt|Baiti|Keskmine|Keskmine Mälu töökomplekt|
|AverageResponseTime|Vastuse Keskmine kellaaeg|Minutit|Keskmine|Vastuse Keskmine kellaaeg|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Meetermõõdustik|Argumendil kuvatav nimi|Ühiku|Koondamise tüüp|Kirjeldus|
|---|---|---|---|---|
|CpuTime|CPU aega|Minutit|Kogusumma|CPU aega|
|Taotlused|Taotlused|Count|Kogusumma|Taotlused|
|BytesReceived|Andmete|Baiti|Kogusumma|Andmete|
|BytesSent|Andmete välja|Baiti|Kogusumma|Andmete välja|
|Http2xx|Http 2xx|Count|Kogusumma|Http 2xx|
|Http3xx|Http 3xx|Count|Kogusumma|Http 3xx|
|Http401|Http 401|Count|Kogusumma|Http 401|
|Http403|Http 403|Count|Kogusumma|Http 403|
|Http404|Http 404|Count|Kogusumma|Http 404|
|Http406|Http 406|Count|Kogusumma|Http 406|
|Http4xx|Http 4xx.|Count|Kogusumma|Http 4xx.|
|Http5xx|Http Serveri tõrked|Count|Kogusumma|Http Serveri tõrked|
|MemoryWorkingSet|Mälu töökomplekt|Baiti|Kogusumma|Mälu töökomplekt|
|AverageMemoryWorkingSet|Keskmine Mälu töökomplekt|Baiti|Keskmine|Keskmine Mälu töökomplekt|
|AverageResponseTime|Vastuse Keskmine kellaaeg|Minutit|Keskmine|Vastuse Keskmine kellaaeg|

## <a name="next-steps"></a>Järgmised sammud

- [Lugege Azure'i monitoris mõõdikud](./monitoring-overview.md#monitoring-sources)
- [Klõpsake mõõdikute teatiste loomine](./insights-receive-alert-notifications.md)
- [Salvestusruumi, sündmuse jaoturi või Log Analytics mõõdikute eksportimine](./monitoring-overview-of-diagnostic-logs.md)
