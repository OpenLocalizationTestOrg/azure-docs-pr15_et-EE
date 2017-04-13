<properties
 pageTitle="Ajasti põhimõtet, termineid ja üksuste | Microsoft Azure'i"
 description="Azure'i Scheduler põhimõtet, terminoloogia ja üksuse hierarhia, sh tööde ja töö saidikogumid.  Kuvatakse täielik näide plaanitud projekti."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Ajasti põhimõtet, terminoloogia + üksuse hierarhia

## <a name="scheduler-entity-hierarchy"></a>Ajasti üksuse hierarhia

Järgmises tabelis kirjeldatakse peamised ressursid esitatud või ajasti API kasutamisel.

|Ressurss | Kirjeldus |
|---|---|
|**Töö saidikogumi**|Töö saidikogumi sisaldab hulga ja säilitab sätted, kvootide ja pidurdab, mis on ühiselt kogumis tööde haldamine. Töö saidikogumi on loodud tellimuse omanik ja rühma kasutus- või rakenduse piirmäärad koos tööde. See on piiratud ühe piirkonna. See lubab ka kvootide piirata kasutamist kõik selle saidikogumi töökohtade täitmist. Kvootide hulka kuuluvad MaxJobs ja MaxRecurrence.|
|**Töö**|Töö määratleb ühe korduvate toimingute täitmise lihtne või keerukas strateegiad. Toimingud võivad sisaldada HTTP, salvestusruumi järjekorda, teenuse siini järjekorda või siin teemas hooldustaotlused.|
|**Töö ajalugu**|Töö ajalugu tähistab täitmiseks, mis on töö üksikasjad. See sisaldab edu vs tõrge, kui ka vastuse üksikasju.|

## <a name="scheduler-entity-management"></a>Ajasti üksuse haldus

Kõrge, jätke ajasti ja Teenusehaldus API ressursside järgmised toimingud:

|Võimalus|Kirjelduse ja URI aadress|
|---|---|
|**Töö saidikogumite haldamine**|SAAMISEKS panna ja tugiteenuste loomiseks ja muutmiseks sisalduva töö ja töö saidikogumite kustutamine. Töö saidikogumi on ümbris töö ja kaartide kvootide ja jagatud sätted. Allpool kirjeldatakse kvootide on maksimaalne arv töö ja Korduvus väikseim väärtus. <p>LUUA ja kustutada.`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>KUVADA.`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Töö haldus**|SAAMISEKS sellele, POSTITADA, paik ja kustutada tugi loovad ja muudavad töö. Kõik tööd peab kuuluma töö kogum, mis on juba olemas, seega ei ole peidetud loomine. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Töö ajalugu haldus**|Abi saamine tõmbamine 60 päeva töö täitmise ajalugu, nt töö kulunud aeg ja töö täitmise tulemused. Lisab päringu stringi parameeter toe filtreerimine põhineb riigi ja olek. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Töö tüübid

On mitut tüüpi tööde haldamine: HTTP tööde haldamine (sh HTTPS tööd, mis toetab SSL-i), salvestusruumi ootel tööd, teenuse siini ootel tööd ja teenuse siini teema tööde haldamine. HTTP-tööde haldamine on optimaalne, kui teil on mõne olemasoleva töökoormus või teenuse lõpp-punkti. Salvestusruumi ootel tööd abil saate postitada nii, et need tööd on käepärane töökoormus, mis salvestusruumi järjekorrad järjekordade salvestusruumi. Samuti on teenuse siini töö käepärane töökoormus, mis teenuse siini järjekorrad ja teemade.

## <a name="the-job-entity-in-detail"></a>Täpsem teave "töö" üksus

Tavaline tasemel plaanitud projekti on mitu osa.

- Mida teha, kui ajastiteenuse töö käivitub toiming  

- (Valikuline) Töö aeg  

- (Valikuline) Millal ja kuidas sageli korrake töö  

- (Valikuline) Kui esmane toiming ei tule toimingu  

Sees, sisaldab plaanitud projekti antud süsteemi andmeid, nt ajastatud täitmisaeg.

Järgmine kood pakub täielik näide plaanitud projekti. Andmed on esitatud järgmistes lõikudes.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Nagu näha valimi ajastatud tööd ülal, töö määratlus on mitu osa.

- Algusaja ("startTime")  

- Toimingu ("toiming"), mis sisaldab viga toimingu ("errorAction")

- Korduvus ("kordumise")  

- State "(osariik)  

- Oleku ("status")  

- Proovige poliitika ("retryPolicy")  

Analüüsime kõik need üksikasjalikult.

## <a name="starttime"></a>startTime

"startTime" on algusaeg ja võimaldab helistaja määrata ajavööndi nihke kaabel [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601)vormingus.

## <a name="action-and-erroraction"></a>toimingu ja errorAction

"Toimingut" on kasutada iga esinemiskorra toimingu ja kirjeldatakse teenuse kutsumise tüüp. Toiming on, mis käivitatakse esitatud ajakava. Ajasti toetab HTTP, salvestusruumi järjekorda, teenuse siini teema ja teenuse siini järjekorda toimingud.

Ülaltoodud näites toimingu on HTTP toiming. Allpool on kujutatud salvestusruumi järjekorda toiming:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Allpool on kujutatud teenuse siin teemas toiming.

  "toiming": {"tüüp": "serviceBusTopic", "serviceBusTopicMessage": {"topicPath": "t1"  
      "nimeruum": "mySBNamespace", "transportType": "netMessaging" / / võib olla netMessaging või AMQP "autentimine": {"sasKeyName": "QPolicy", "tüüp": "sharedAccessKey"}, "sõnum": "Mõni sõnum", "brokeredMessageProperties": {}, "customMessageProperties": {"rakendusenimi": "FromScheduler"}},}

Allpool on kujutatud teenuse siini järjekorda toiming:


  "toiming": {"serviceBusQueueMessage": {"queueName": "Kv1"  
      "nimeruum": "mySBNamespace", "transportType": "netMessaging" / / võib olla netMessaging või AMQP "autentimine": {}  
        "sasKeyName": "QPolicy", "tüüp": "sharedAccessKey"}, "sõnum": "Mõni sõnum"  
      "brokeredMessageProperties": {}, "customMessageProperties": {"rakendusenimi": "FromScheduler"}}, "tüüp": "serviceBusQueue"}

"errorAction" on tõrge sündmuseohjuri, kui esmane tegevus nurjub toiming. Saate muutuja lõpp tõrketöötluse helistada või saata teatis. See saab kasutada kontaktidel teisene lõpp-punkti juhul, et esmast ei ole saadaval (nt puhul on lõpp-punkti saidil katastroof) või kasutada teavitades lõpp-punkti töötlemise tõrge. Nii nagu esmane tegevus toimingu tõrge võib olla liht- või kombineeritud loogika muude toimingute alusel. Saate teada, kuidas luua SAS sõnet, vaadake [loomine ja kasutamine ühiskasutusse Accessi signatuuri](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>Korduvus

Korduvus on mitu osa.

- Sagedus: Üks minut, tund, päeva-, nädala, kuu, aasta  

- Intervall: Salvestusintervalli antud sageduse kordumist  

- Ettenähtud ajakava: minutit, tundi, nädalapäevade, kuude ja monthdays kordumise määramine  

- Arv: Sündmuste arv  

- Lõpuaeg: pole töid täitma pärast määratud lõppkellaaeg  

Töö on korduv, kui see on korduva objekti, mis on määratud JSON määratlemisel. Kui määratud on nii count ja endTime, on au esimesena lõpetamise reegel.

## <a name="state"></a>olek

Töö on üks neljast väärtused: lubatud, keelatud, lõpetatud või tõrge. Saate panna või paik töö nii, et neid värskendada lubatud või keelatud olekusse. Kui tööd on lõpetatud või tehtud, mis on Viimane olukord, mida ei saa värskendada (küll endiselt KUSTUTAMIST töö). Atribuudi olekus näide on järgmine:


        "state": "disabled", // enabled, disabled, completed, or faulted
Täidetud ja tehtud töö kustutatakse 60 päeva pärast.

## <a name="status"></a>olek

Pärast käivitamist ajasti töö, tagastatakse teabe töö praeguse oleku kohta. See objekt pole kasutaja kõigest – see on seatud süsteem. Siiski kuulub töö objekti (mitte eraldi lingitud ressursi) nii, et võib saada hõlpsalt töö olek.

Töö olek sisaldab eelmise täitmise (vajaduse korral) aeg, Järgmine ajastatud täitmise (pooleliolev töö) jaoks aega ja täitmise count töö.

## <a name="retrypolicy"></a>retryPolicy

Kui ajasti töö nurjub, on abil saate määrata, uuesti poliitika otsustada, kas ja kuidas toimingu uuesti proovida. See on määratud **retryType** objekti – see on seatud **pole** kui puudub uuesti poliitika, nagu eespool näidatud. Määrake selleks **fikseeritud** kui uuesti poliitika.

Proovi uuesti poliitika määramiseks võib määratud kahe täiendavaid sätteid: uuesti intervalli (**retryInterval**) ja (**retryCount**) korduste arv.

Uuesti intervalli, määratud **retryInterval** objekti, kus on korduskatsed intervalli. Vaikeväärtus on 30 sekundit, konfigureeritav miinimumväärtus on 15 minutit ja selle maksimumväärtuse on 18 kuud. Tasuta töö saidikogumid töökohtade on konfigureeritav miinimumväärtus on 1 tund.  See on määratletud ISO 8601 vormingus. Samuti korduste arv väärtus on määratud **retryCount** objekt; See on mitu korda on proovitakse uuesti. Vaikeväärtus on 4 ja selle maksimumväärtus on 20\. nii **retryInterval** ja **retryCount** on valikulised. Need on esitatud oma vaikimisi väärtused, kui **retryType** on **fikseeritud** ja ei sisalda väärtusi määratud otseselt.

## <a name="see-also"></a>Vt ka

 [Mis on ajasti?](scheduler-intro.md)

 [Azure'i portaalis Scheduler kasutamise alustamine](scheduler-get-started-portal.md)

 [Lepingud ja arvelduse Azure'i ajasti](scheduler-plans-billing.md)

 [Kuidas luua keerukate ajakavasid ja täpsemalt Korduvus Azure'i Scheduleri abil](scheduler-advanced-complexity.md)

 [Azure'i Scheduler REST API viide](https://msdn.microsoft.com/library/mt629143)

 [Azure'i Scheduler PowerShelli cmdlet-käskude viitamine.](scheduler-powershell-reference.md)

 [Azure'i Scheduler kõrge-saadavus ja usaldusväärsus](scheduler-high-availability-reliability.md)

 [Azure'i Scheduler piirangud, vaikesätted ja kuvatavad tõrkekoodid](scheduler-limits-defaults-errors.md)

 [Azure'i Scheduler väljaminevat autentimist](scheduler-outbound-authentication.md)
