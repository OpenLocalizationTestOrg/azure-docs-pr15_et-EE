<properties 
    pageTitle="Voo Analytics vabastage märkmete | Microsoft Azure'i" 
    description="Voo Analytics väljalaskemärkmed" 
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

#<a name="stream-analytics-release-notes"></a>Voo Analytics vabastage märkmed

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Märkmete 04/15/2016 versiooni voo Analytics ##

See väljaanne sisaldab järgmisi update.

Pealkiri | Kirjeldus
---|---
Power BI väljundeid üldine kättesaadavus  | [Power BI väljundid](stream-analytics-power-bi-dashboard.md) on nüüd üldiselt kättesaadav. 90 päeva autoriseerimine aegumise Power BI on eemaldatud. Lisateavet stsenaariumid, kus luba on vaja uuendada jaotisest [pikenda luba](stream-analytics-power-bi-dashboard.md#Renew-authorization) luua Power BI armatuurlaua.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Märkmete 03/03/2016 versiooni voo Analytics ##

See väljaanne sisaldab järgmisi update.

Pealkiri | Kirjeldus
---|---
Uued voo Analytics päringukeele üksused  | SAQL on nüüd [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN-i lehe") [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN-i lehe") ja [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN-i leht").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Märkmete voo Analyticsi lubamise 12/10/2015 ##

See väljaanne sisaldab järgmisi update.

Pealkiri | Kirjeldus
---|---
REST API versiooni värskendus | REST API versioon on uuendatud 2015-10-01. Üksikasjad leiate MSDN-i [Voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx) ja [seadme õ integreerimise voo Analytics](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md).
Azure'i masina õ integreerimine | Selles versioonis tuleb kasutajatugi Azure kohapeal Õppekeskuse kasutaja määratletud funktsioonid. Lugege teemat [õppeteema](stream-analytics-machine-learning-integration-tutorial.md) Lisateavet kui ka [üldist ajaveebi teadaanne](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx).

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Märkmete voo Analyticsi lubamise 11/12/2015 ##

See väljaanne sisaldab järgmisi update.

Pealkiri | Kirjeldus
---|---
Valige uus käitumine | Valige voo Analytics on laiendatud luba * atribuudi juurdepääsumeetodi pesastatud kirje nimega. Lisateabe saamiseks konsulteerige [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Keerukate andmetüübid").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Märkmete 10/22/2015 versiooni voo Analytics ##

See väljaanne sisaldab järgmised värskendused.

Pealkiri | Kirjeldus
---|---
Päringu keele funktsioonid | Voo Analytics on laiendatud päringukeele, sh järgmised funktsioonid: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [LOGIGE](https://msdn.microsoft.com/library/azure/mt605290.aspx), [ruutu](https://msdn.microsoft.com/library/azure/mt605288.aspx)ja [SQRT](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Eemaldatud liitväärtuse piirangud  | See väljaanne eemaldab päringu 15 agregaadid piiramist. Nüüd on mingeid piiranguid arvu agregaadid päringu kohta.
Lisatakse rühma System.Timestamp funktsioon | Funktsiooni [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) võimaldab nüüd window_type või [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Lisatud OFFSET langema ja Hopping Windowsi jaoks | Vaikimisi on null reaalajas joondatud [langema](https://msdn.microsoft.com/library/azure/dn835055.aspx) ja [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) windows (1/1/0001 12:00:00 AM UTC). (Valikuline) uus parameeter 'offsetsize' võimaldab määrata kohandatud offset (või joondus).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Märkmete 09/29/2015 versiooni voo Analytics ##

See väljaanne sisaldab järgmised värskendused.

Pealkiri | Kirjeldus
---|---
Azure'i asjade komplekti avaliku eelvaade | Voo Analytics kuulub komplekti Azure'i asjade avaliku eelvaate.
Azure'i portaalis integreerimine | Lisaks jätkumise kohalolek Azure'i haldusportaal voo Analytics on nüüd integreeritud [Azure portaali](https://azure.microsoft.com/overview/preview-portal/). Teate, et voo Analytics eelvaade portaalis funktsioonid on praegu funktsioonide alamhulk pakutud Azure haldusportaali, toetuseta brauseris päringu testimiseks Power BI väljund konfigureerimise ja sirvides või uue sisestus- ja väljundi ressursside loomine tellimused, mida teil on juurdepääs.
DocumentDB väljundi tugi | Nüüd saate voo Analytics töö väljund [DocumentDB](https://azure.microsoft.com/services/documentdb/).
Asjade jaoturi sisendi tugi | Voo Analytics tööde haldamine saate nüüd neelata asjade jaoturi andmeid.
AJATEMPLI, heterogeensete sündmuste jaoks. | Kui ühe andmevoos sisaldab mitut tüüpi sündmusi, millel erinevate väljade ajatemplid, nüüd saate [AJATEMPLI,](http://msdn.microsoft.com/library/mt573293.aspx) avaldiste abil määrata eri ajatempli väljad iga puhul.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Märkmete voo Analyticsi lubamise 09/10/2015 ##

See väljaanne sisaldab järgmised värskendused.

Pealkiri|Kirjeldus
---|---
PowerBI rühmade tugi|Power BI teiste kasutajatega ühiskasutuse andmete lubamiseks voo Analytics töö saab nüüd kirjutada [PowerBI rühmade](stream-analytics-define-outputs.md#power-bi) Power BI teie kontol.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Märkmete 08/20/2015 versiooni voo Analytics ##

See väljaanne sisaldab järgmised värskendused.

Pealkiri|Kirjeldus
---|---
Lisatud viimase funktsioon |[Viimase](http://msdn.microsoft.com/library/mt421186.aspx) funktsiooni on nüüd saadaval voo Analytics töö, mis võimaldab teil tuua viimase sündmuse mõne sündmuse voo teatud kindla ajavahemiku jooksul.
Massiivi uued funktsioonid|Massiivi [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) ja [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) kohe saadaval.
Uue kirje funktsioonid|Kirje [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) ja [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) kohe saadaval.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Märkmete 07-30-2015 versiooni voo Analytics ##

See väljaanne sisaldab järgmised värskendused.

Pealkiri|Kirjeldus
---|---
Power BI ettevõtte Id sidumata Azure'i Id|See funktsioon võimaldab [Power BI väljundi](stream-analytics-power-bi-dashboard.md) ASA tööde Azure kontotüüp (Live ID-ga või ettevõtte Id) alusel. Lisaks saate on üks ettevõtte Id Azure'i konto ja kasutada mõne muu lubada Power BI väljundi jaoks.
Teenuse siini järjekorrad väljundi tugi|[Teenuse siini järjekorrad](stream-analytics-define-outputs.md#service-bus-queues) väljundeid on nüüd saadaval voo Analytics tööde haldamine.
Teenuse siini teemade väljundi tugi|[Teenuse siini teemade](stream-analytics-define-outputs.md#service-bus-topics) väljundeid on nüüd saadaval voo Analytics tööde haldamine.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Märkmete 07-09-2015 versiooni voo Analytics ##

See väljaanne sisaldab järgmised värskendused.


Pealkiri|Kirjeldus
---|---
Kohandatud Bloobivahemälu väljundi eraldamine|Bloobimälu salvestusruumi väljundeid nüüd luba suvand määramaks sageduse selle väljundi plekid kirjutada struktuuri ja vormi väljundi andmestruktuuri tee kausta. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Märkmete voo Analyticsi lubamise 05/03/2015 ##

See väljaanne sisaldab järgmised värskendused.


Pealkiri|Kirjeldus
---|---
Jaoks välja arv hälbe akna suurendatud suurima väärtuse.|Akna Tellimuse välja arv hälbe maksimaalne maht on nüüd 59:59 (mm: ss)
JSON-vorming: Joon, mis on eraldatud või massiivist.|Nüüd on võimalus, kui kirjutamine bloobimälu või sündmuse jaoturi väljasta massiivina kummagi JSON objektide või JSON objektide koos uue rea. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Märkmete 04-16-2015 versiooni voo Analytics ##


Pealkiri|Kirjeldus
---|---
Viivitus Azure Storage konto konfigureerimine|Kui loote voo Analytics töö piirkonnas esimest korda, palutakse salvestusruumi uue konto loomine või konto voo Analytics töö selle piirkonna jälgida. Tõttu latentsus konfigureerimise jälgimine, palub loomine mõne muu voo Analytics töö piirkonna 30 minuti jooksul teise salvestusruumi konto asemel nähtaval viimati konfigureeritud üks jälgimine salvestusruumi konto ripploendis määratlemiseks. Et vältida mittevajalike salvestusruumi konto loomine, oodake 30 minutit pärast loomise töö piirkonnas enne täiendavate tööde haldamine selle piirkonna esimest korda.
Töö täiendamine|Sel ajal, ei toeta voo Analytics reaalajas muudatused määratlus või esitatava töö konfigureerimine. Sisendi, väljundi, päringu, skaala või esitatava töö konfiguratsiooni muutmiseks peate esmalt lõpetama töö.
Tuletatud sisendandmete allikas andmetüübid|Kui lause CREATE TABLE ei kasutata, sisendi tüüp on tuletatud sisestamise vorming, näiteks CSV kõik väljad on string. Väljad tuleb teisendatakse konkreetselt funktsiooni CAST tüüp lahknevuse vigade vältimiseks õige tüüp.
Puuduvad väljad on tühjad väärtused nimega väljundit|Tühiväärtuste esinemisvõimaluse väljundi sündmuse tulemuseks viitamine välja, mis ei ole sisendandmete allikas.
Laused peab eelnema SELECT-laused|Oma päringu järgima SELECT-laused alampäringud määratletud laused sisse.
Mälu-probleem|Taaskäivitage streaming Analytics töö suurte hälbega-order sündmused ja/või keerukate päringute haldamine suurt hulka olekus võib põhjustada töö otsa mälu, mille tulemuseks on töö. Käivitamise ja lõpetamise toimingud on nähtav projekti Logi sisse. Selle vältimiseks, skaala üle mitme sektsioonid päringu välja. Tulevikus vabastada, käsitletakse seda piirangut, alandav jõudluse kannatavas töökohtade asemel need taaskäivitada.
Suure bloobimälu sisendina ilma last ajatempli võib põhjustada mälu-probleem|Tarbimine suurte failide bloobimälu võib põhjustada krahhi voo Analytics töö kui ajatempli välja pole määratud kaudu AJATEMPLI. Selle probleemi vältimiseks hoidke suurus iga bloobimälu alla 10MB.
SQL-andmebaasi sündmuse helitugevuse piirangud|SQL-andmebaasi kasutamisel väljundi eesmärk on väga kõrge väljundi andmemahtusid võib põhjustada voo Analytics töö jõuda ajalõpuni. Selle probleemi lahendamiseks Helitugevuse vähendamine agregaadid või filter tehtemärkide abil või valida mõne väljundi target hoopis Azure'i bloobimälu või sündmuse jaoturi.
PowerBI andmekomplektide võib sisaldada ainult ühest tabelist|PowerBI ei toeta konkreetse rohkem kui ühe tabeli.

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
