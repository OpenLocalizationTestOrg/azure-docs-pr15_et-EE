<properties 
    pageTitle="Loogika rakenduste API loomine" 
    description="Luua kohandatud API kasutamine loogika rakendustega" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Luua kohandatud API kasutamine loogika rakendustega

Kui soovite laiendada loogika rakenduste platvormi, on mitmel viisil saate helistada API-d või süsteemide, mis pole saadaval ühte meie paljude out-of-box konnektorid.  Üks neid võimalusi, kuidas saate helistada loogika rakenduse töövoo API rakenduse loomine.

## <a name="helpful-tools"></a>Võib leida kasulikke tööriistu

API-de toimivad kõige paremini loogika rakendused, soovitame üksikasjalikult toetatud toimingute ja parameetrite oma API [ärplema](http://swagger.io) dokumendi loomisel.  On palju teekide (nt [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)), mis loob automaatselt ärplema teile.  Saate [TRex](https://github.com/nihaue/TRex) abil marginaalide ärplema töötada ka loogika rakendused (Kuva nimed, objektid, jne).  Mõned näited API Apps ehitatud loogika rakenduste jaoks kindlasti vaadake meie [GitHub hoidla](http://github.com/logicappsio) või [ajaveebi](http://aka.ms/logicappsblog).

## <a name="actions"></a>Toimingud

Lihtsa toimingu rakenduse loogika on kontrolleril, mille vastu HTTP-päringu ja vastuse (tavaliselt 200) tagastamiseks.  Siiski on erinevate mustrite saab jälgida, et laiendada toimingud vastavalt oma vajadustele.

Vaikimisi loogika rakenduse mootor ei ajalõpp taotluse pärast 1 minutit.  Siiski saate määrata oma API käivitada toiminguid, mida kauem ja on mootor oodake lõpetamise, järgides asünkroonse või webhook mustri allpool.

Standardse toimingute tegemiseks lihtsalt kirjutada HTTP taotluse meetod oma API, mis on esitatud ärplema kaudu.  Saate vaadata API rakendused, mis töötavad loogika rakenduste meie [GitHub hoidla](https://github.com/logicappsio)näited.  Allpool on levinud mustriga täita kohandatud konnektor viisid.

### <a name="long-running-actions---async-pattern"></a>Pikaajalise toimingud - asünkroonse muster

Pikk samm või tööülesande käivitamisel on peate esmalt veenduge, et mootori teab, et te pole veel aegus. Soovite suhelda mootori ja kuidas ma teada, kui olete lõpetanud ülesanne ja lõpuks, peate nii, et saate jätkata selle töövoo mootori vajalikke andmeid naasmiseks. Saate täita mis kaudu API järgides allpool voogu. Need juhised on punkt-ja-vaates kohandatud API.

1. Kui taotlus on vastu võetud, tagastab vastuse kohe (enne töid). Selle vastus on `202 ACCEPTED` vastust, lastes teil andmeid leida mootori aktsepteeritud soovitud last ja on nüüd töötlemine. 202 vastus peaks sisaldama järgmisi päised: 
 * `location`päise (nõutav): see on absoluutne tee URL-i loogika rakenduste abil saate töö oleku vaatamine.
 * `retry-after`(valikuline, kuvatakse vaikimisi 20 toimingute jaoks). See on mootor peaks oodatakse enne küsitlused päise asukoha URL oleku sekundite arv.

2. Kui töö olek on märgitud, tehke järgmised kontrollid. 
 * Kui töö on valmis: tagastada on `200 OK` vastus koos vastuse last.
 * Kui töö on ikka töötlemise: tagastada teise `202 ACCEPTED` vastuse vastus algse nimega sama päistega

See muster võimaldab teil oma kohandatud API jutulõnga väga kaua ülesandeid, kuid hoida ühendust elus loogika rakenduste mootori nii, et see ei ajalõpp või jätkake enne lõpuleviimist. Kui lisate seda oma loogika rakendusse, on oluline märkida, et teil pole vaja midagi definitsiooni loogika rakenduse jätkamiseks küsitlus ja oleku kontrollimine. Kui mootori näeb 202 aktsepteeritud vastuse pealkirjaga sobiv asukoht, see au asünkroonse mustri ja jätkake küsitlus asukoht päis, kuni mitte-202 tagastatakse.

Saate vaadata selle muster GitHub valimil [siin](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook toimingud

Oma töövoo käigus saate määrata loogika rakenduse viige ja oodake, kuni "tagasihelistamise" jätkata.  See tagasihelistamise on saadaval ka HTTP postituse.  Rakendada selle mustri, tuleb sisestada kaks lõpp-punktid kontrolleril: tellimuse tühistada.

Klõpsake 'Liitu' loogika rakenduse loomine ja tagasihelistamise URL-i, kus saate talletada oma API ja tagasihelistamise registreerida valmis mõne HTTP Postita.  Mis tahes sisu/päised rakendusse loogika küsimise ja ülejäänud töövoo saab kasutada.  Loogika rakenduse engine helistab Telli punkti täitmise niipea, kui see tabab seda toimingut.

Kui Käivita, loogika rakenduse engine on helistamine 'tellimuse lõpp-punkti.  Seejärel saate oma API unregister tagasihelistamise URL-i, vastavalt vajadusele.

Praegu loogika rakenduse Designer ei toeta avastamine nii, et seda tüüpi toimingu kasutamiseks peate "Webhook" toimingu lisada ja määrata URL-i, päised ja keha teie taotlus läbi ärplema, webhook lõpp-punkti.  Saate kasutada funktsiooni `@listCallbackUrl()` töövoo funktsioon, mis tahes tagasihelistamise URL-is edasi vajadusel neile väljadele.

Saate vaadata webhook mustri GitHub valimil [siin](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Päästikute

Lisaks toimingud, saate määrata oma kohandatud API act loogika rakenduse käivitamiseks.  Saate jälgida all loogika rakenduse käivitamiseks on kahte tüüpi on:

### <a name="polling-triggers"></a>Küsitlused päästikute

Küsitlused päästikute toimivad palju nagu ülaltoodud pikk töötab asünkroonse toimingud.  Loogika rakenduse engine on kõne päästik lõpp-punkti teatud aja jooksul (sõltub SKU-ga, Tindimärkmete Premiumi, Standard, 1 minuti ja tasuta 1 tund).

Kui ei ole andmeid, tagastab funktsioon käivitab on `202 ACCEPTED` vastust koos mõne `location` ja `retry-after` päis.  Kuid päästikute jaoks on soovitatav on `location` päises Päringuparameetri, `triggerState`.  See on teie API teadma, kui viimast loogika rakenduse töötavad mõned tunnuse.  Kui seal on andmeid, käivitab annab vastuseks `200 OK` vastus koos sisu last.  See tule loogika rakendus.

Näiteks kui mul on küsitlused, kui fail on saadaval, võib koostada küsitlused päästik, mida soovite teha järgmist:

* Kui taotluse vastu võttis koos pole triggerState API tagasi on `202 ACCEPTED` koos mõne `location` päis, mis sisaldab triggerState praegune kellaaeg ja `retry-after` 15.
* Kui taotluse vastu võttis koos mõne triggerState:
 * Kontrollige, kui kõik failid on lisatud pärast triggerState kuupäev ja kellaaeg. 
  * Kui seal on 1, tagastab soovitud `200 OK` vastuse sisu last, kus inkrementida triggerState, et ma tagasi ja määrake faili DateTime funktsiooni `retry-after` 15.
  * Kui loendis on mitu faili, mul võib tagastada 1 aega on `200 OK`, inkrementida minu triggerState klõpsake soovitud `location` päis ja määrata, `retry-after` 0.  See võimaldab leida rohkem andmeid on saadaval ja seda kohe taotleda veebisaidil mootor on `location` määratud päis.
  * Kui puuduvad failid, tagastavad on `202 ACCEPTED` vastus ja jätke selle `location` triggerState sama.  Määrake `retry-after` -15.

Saate vaadata küsitlusi päästik ka GitHub valimil [siin](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Päästikute Webhook

Päästikute Webhook toimivad palju nagu Webhook toimingud ülaltoodud.  Loogika rakenduse engine on kõne 'Liitu lõpp-punkti iga kord, kui webhook päästik on lisatud ja salvestada.  Oma API saate registreerida webhook URL-i ja kõne HTTP-posti teel, iga kord, kui andmed on saadaval.  Sisu last ja päised läks loogika rakenduse käivitamine.

Kui kunagi kustutatakse webhook päästik (rakenduse loogika täielikult, või lihtsalt webhook päästik), mootor on helistamine 'tellimuse URL, kus saate oma API unregister tagasihelistamise URL-i ja Lõpeta protsessid vastavalt vajadusele.

Praegu loogika rakenduse Designer ei toeta avastamine webhook päästik kaudu ärplema, et kasutada seda tüüpi toimingut tuleb lisada "Webhook" päästik ja määrata URL-i, päised ja keha teie taotlus.  Saate kasutada funktsiooni `@listCallbackUrl()` töövoo funktsioon, mis tahes tagasihelistamise URL-is edasi vajadusel neile väljadele.

Saate vaadata webhook päästik ka GitHub valimil [siin](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)