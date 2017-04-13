<properties
 pageTitle="Arendaja juhend - otsese meetodite | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - kasutamine otsese võimalusi autonoomsest kood teie seadmetes"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Otsest meetodit (eelvaade) seadmes

## <a name="overview"></a>Ülevaade

Asjade jaoturi annab teile võimalus autonoomsest meetodite pilvest seadmetes. Meetodite tähistavad taotlus-vasta suhtluse seadmega, mis on sarnane HTTP-kõne, et neil õnnestub või lasta teada kõne olekut kasutaja kohe (pärast kasutaja määratud ajalõpp) ei. See on kasulik stsenaariumid, kus kohe toimingu käigus erineb sõltuvalt sellest, kas seade on võimalik vastata, nt seadme SMS pärast üles saata, kui seade on ühenduseta (SMS on kõrgemad kui kõne meetod).

Mõelge meetodi nimega kaugprotseduurikutse otse seadmele. Meetodid, mida on rakendatud seadmes nimetatakse pilveteenuses. Kui seadmes, mis on määratletud meetodi meetodit proovib pilves, meetod kõne nurjub.

Iga meetodi seadme eesmärgid ühe seade. [Töö] [ lnk-devguide-jobs] võimalda autonoomsest meetodite mitmes seadmes ja järjekorda meetod kutsumise ühendatud seadmete jaoks.

Kõik, kellel on **teenuse ühenduse** õiguste asjade jaoturi võib kasutada meetodi seadmes.

### <a name="when-to-use"></a>Millal kasutada

Seadme meetodite sarnanevad [cloud-seadme sõnumite] [ lnk-devguide-messages] , et mõlemad lubada teabe edastamiseks seadme cloud tagaandmebaas, kuid need erinevad peamised viisid, kuidas. Põhimõtteliselt meetodid on sünkroonse ja mitte püsival, cloud-seadme sõnumite aga asünkroonne kestvus kuni 48 tundi.

Meetodite järgige päringu vastuse mustri ja pole püsiv. Kestvus puudumine pakub kahte kohe eeliseid, kui teil on ülem seadmed:

- **Kohe tagasisidet meetod täitmise** tähendab, et ei ole vaja hallata korrelatsiooni päringu ja vastuse vahel.
- **Suurema läbilaskevõimega** tähendab, et need toimingud saate kiiremini teha, sest asjade jaoturi ei paku mis tahes kestvus. Asjade jaoturi abil saavad rohkem meetodi kutsed ühiku kui cloud-seadme sõnumid.

Pilveteenuse-seadme sõnumeid ei pruugi käsud seadmele, kuid kujutavad pigem mõnda pilveteenusesse, läbides teabe mõned natuke seda oma vabal ajal kättesaamine seade ja mis seade võib või võib-olla ei vasta. Pilveteenuse-seadme sõnumid on ajalõpp rohkem aega (kuni 48 tundi) palju kiiremini aeguks meetodite ajal.

Kohe käsk kutsumise seadme ja ajastatud kutsumise käsu seadmes tööd jaoks kasutada seadme võimalusi.

## <a name="method-lifecycle"></a>Meetodi elutsükkel

Seadme rakendatakse ja võib olla vaja null või rohkem sisendina rakenduses meetod last õigesti väärtustada. Kutsute otsene meetod – teenus suunatud URI (`{iot hub}/twins/{device id}/methods/`). Seadme saab otse meetodite läbi seadme MQTT teema (`$iothub/methods/POST/{method name}/`). Me võivad toetada meetodite täiendavate seadme-side võrgu protokollide tulevikus.

> [AZURE.NOTE] Kui kutsute otsene meetod seadmes, atribuutide nimesid ja väärtuste võib sisaldada ainult US-ASCII prinditavad tähtedest ja numbritest koosnev, välja arvatud juhul, kui mõni järgmised määramine: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Võimalused on sünkroonse ja kas õnnestunud või nurjuda aja pärast (vaikimisi: 30 sekundi järel, kõigest üles 3600 sekundit). Meetodid on kasulikud interaktiivsed stsenaariumid soovitud seade, mis toimib ainult juhul, kui seade on võrgus ja vastuvõtmise käsud, nagu sisselülitamine kerge telefoniga. Need stsenaariumid, mida soovite näha kohe takistavaid nii pilveteenusesse saate toimivad tulemi nii kiiresti kui võimalik. Seade võib tagastada mõned sõnumi kehasse tulemusena meetodit, kuid see pole vaja teha seda meetodit. Pole mingit garantiid tellimine või mis tahes kokkulangevus semantika meetod kõnesid.

Seadme meetod videokõnesid HTTP ainult cloud servast ja MQTT vaid seadme servast.

## <a name="reference-topics"></a>Teemasid:

Järgmistes teemades viide teile otsese meetodite kasutamise kohta leiate lisateavet.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Otsest meetodit tagaandmebaas rakenduse kaudu

### <a name="method-invocation"></a>Meetod kutsumise

Otsene meetod manamine seadmes on HTTP kõnesid, mis hõlmavad.

- Konkreetse seadme *URI* (`{iot hub}/twins/{device id}/methods/`)
- POSTITUSE *meetod*
- *Päised* , mis sisaldavad loa taotlemine ID, sisutüübi ja sisu kodeering
- JSON *keha* järgmises vormingus:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Ajalõpp on sekundites. Kui ajalõpp pole määratud, vaikimisi 30 sekundit.
  
### <a name="response"></a>Vastus

Funktsiooni tagaandmebaas saab vastuse, mis sisaldab:

- *HTTP olekukoodi*, mida kasutatakse tõrgete pärit asjade jaoturi, sh 404 viga pole praegu ühendatud seadmete jaoks
- *Päised* , mis sisaldavad etag, taotleda ID, sisutüübi ja sisu kodeering
- JSON *keha* järgmises vormingus:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Mõlemad `status` ja `body` on esitatud seade ja kasutada seadme enda olekukoodi ja/või kirjeldus.

## <a name="handle-a-direct-method-on-a-devcie"></a>Täitepide otsene meetod on devcie kohta

### <a name="method-invocation"></a>Meetod kutsumise

Seadmed saavad otsene meetod taotlusi MQTT teema:`$iothub/methods/POST/{method name}/?$rid={request id}`

Sisu, mida saab seadme on järgmises vormingus:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Meetodi kutsed on QoS 0.

### <a name="response"></a>Vastus

Seadme saadab vastuste `$iothub/methods/res/{status}/?$rid={request id}`, kus:

 - Funktsiooni `status` atribuut on meetod täitmise seadme esitatud olekut.
 - Funktsiooni `$rid` atribuut on taotluse ID meetod kutsumise, mis on saadud asjade keskuse kaudu.

Sisu on määratud seade ja võib olla mis tahes olek.

## <a name="additional-reference-material"></a>Täiendav viide materjali

Muud viited teemade arendaja juhend on järgmised.

- [Asjade jaoturi lõpp-punktid] [ lnk-endpoints] kirjeldatakse mitmesuguste lõpp-punktid, mis iga asjade jaoturi seab käitusaja ja haldus.
- [Pidurdamise ja kvootide] [ lnk-quotas] kirjeldatakse piirmäära, mis kehtivad teenuse asjade jaoturi ja ahendamise käitumine oodata, kui kasutate teenust.
- [Asjade jaoturi seade ja SDK-d] [ lnk-sdks] erinevate keele SDK-d on loetletud mõni kasutamine, kui teil tekib nii seade ja rakendused, mis nendega asjade jaoturi abil saate.
- [Päringu keel kaksikud, võimalused ja töö] [ lnk-query] kirjeldatakse päringukeele abil saate otsida teavet asjade keskuse kohta oma seadmes kaksikud, võimalused ja -tööde haldamine.
- [Asjade jaoturi MQTT tugi] [ lnk-devguide-mqtt] annab Lisateavet kohta asjade jaoturi tugi MQTT protokolli.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud, kuidas kasutada otsest meetodit, võib olla huvitatud arendaja juhend järgmist teemat:

- [Mitmes seadmes tööde plaanimine][lnk-devguide-jobs]

Kui soovite proovida mõnda selles artiklis kirjeldatud mõisted, võib olla huvi järgmised asjade jaoturi õpetus:

- [Kasutage cloud-seadme meetodid][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
